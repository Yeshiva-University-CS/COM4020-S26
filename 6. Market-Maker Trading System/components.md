## Components You Will Build

This section defines the components of the system, the responsibilities of each component, and the authority boundaries between them.

---

## Running Services

### 1. Exchange Service

The exchange service accepts external orders and executes them against the currently active quotes.

Responsibilities:

- Exposes an API for submitting external buy and sell orders.
- Accepts quote publish/replace requests for symbols.
- Maintains, per symbol, the currently active quote (if any) and its expiration time.
- Rejects execution against expired quotes.
- Matches external orders against the active quote for the symbol.
- Supports partial fills when the external order quantity is smaller than the remaining quote quantity.
- Produces fill events when an external order executes against a quote.

Authority boundaries:

- The exchange is the authority for whether an external order executes based on the currently active quote and its remaining quantity.
- The exchange is not the authority for positions.
- The exchange is not the authority for global exposure limits.
- The exchange does not decide whether a quote is allowed under the global exposure limit.

---

### 2. Trading State Service

The trading state service records fills durably and maintains authoritative positions.

Responsibilities:

- Accepts fill events from the exchange.
- Records fills durably and applies them exactly once.
- Maintains the current net position per symbol.
- Publishes committed position updates for consumption by market-making nodes.
- Provides an API for reading current positions.

Authority boundaries:

- The trading state service is the authority for positions.
- A fill is not considered reflected in the system’s position state until it is durably recorded and applied by the trading state service.
- Market makers must treat committed position updates as authoritative and must not compute position from raw fill events.

---

### 3. Exposure Reservation Service

The exposure reservation service enforces the global exposure limit based on currently active quotes.

Responsibilities:

- Maintains the global exposure limit of ±100.
- Tracks exposure usage exclusively from the quantities of currently active quotes.
- Decides whether a proposed quote may become active based on current exposure usage.
- Grants, reduces, or rejects exposure reservations for proposed quotes.
- Updates exposure reservations when a quote is partially filled or fully filled.
- Releases exposure reservations when a quote is replaced or expires.
- Ensures exposure capacity is not leaked after crashes, retries, or expirations.

Authority boundaries:

- The exposure reservation service is the authority for global exposure usage and quote-backed exposure reservations.
- A quote must not become active unless a corresponding exposure reservation has been granted for it.
- Market makers and the exchange must not infer global exposure availability from local state.

Notes:

- The service may be implemented as a standalone service or co-located with another service, but it must behave as a single coherent authority for exposure reservation decisions.

---

### 4. Market-Maker Nodes

Market-maker nodes publish quotes and keep quotes valid as the system evolves.

Responsibilities:

- Consume committed position updates from the trading state service.
- Produce deterministic quote proposals (bid/ask price and quantities) based on the latest committed position state.
- Obtain exposure reservations for proposed quotes before attempting to activate them.
- Publish or replace quotes through the exchange service.
- Refresh quotes as needed to prevent expiration.
- Ensure stale or out-of-order quote attempts do not replace newer quotes.

Authority boundaries:

- Market-maker nodes are not the authority for fills.
- Market-maker nodes are not the authority for positions.
- Market-maker nodes are not the authority for exposure usage.
- Market-maker nodes must not publish a quote unless exposure has been granted for that quote.

---

### 5. External Order Publisher

The external order publisher generates external trading activity by calling the exchange API.

Responsibilities:

- Connects to the exchange API and submits external buy and sell orders.
- Generates orders across symbols with configurable rates and quantities.
- Can issue multiple orders concurrently.
- Does not read internal trading state as input for order generation.

Authority boundaries:

- The external order publisher has no authority over quotes, fills, positions, or exposure.
- The external order publisher exists only to drive trading activity and create observable load and concurrency.

---

### 6. Position Display UI

A UI that displays live and updated positions.

Responsibilities:

- Reads positions from the trading state service.
- Displays current per-symbol positions and their latest update time/version.
- Updates as new committed position updates occur.

Authority boundaries:

- The UI is read-only with respect to trading state.
- The UI does not publish quotes or submit external orders.

---

## Required Models / Definitions

### Quote

A quote is the current executable liquidity for a symbol.

Required fields:

- symbol
- bid_price, bid_quantity
- ask_price, ask_quantity
- quote_id (unique identifier)
- expires_at (30-second TTL from activation or refresh)

Rules:

- At most one quote is active per symbol at a time.
- An expired quote is not eligible for execution.
- Quote updates must not allow stale state to override newer state.

---

### External Order

An external order is a request submitted to the exchange.

Required fields:

- order_id (unique identifier)
- symbol
- side (buy or sell)
- quantity
- limit_price (or equivalent bounded price rule)

Rules:

- An order executes only if it crosses the active quote.
- Orders may partially fill against the remaining quote quantity.

---

### Fill

A fill is the result of a successful execution against a quote.

Required fields:

- fill_id (unique identifier)
- order_id
- symbol
- side
- quantity
- price
- quote_id (the quote that was executed)
- created_at

Rules:

- A fill is immutable once created.
- Fills are delivered to the trading state service for durable recording.

---

### Position

A position is the authoritative net quantity held for a symbol.

Required fields:

- symbol
- net_quantity (positive for long, negative for short)
- position_version (monotonic per symbol)
- last_fill_id (or equivalent linkage)

Rules:

- Positions are updated only by applying recorded fills.
- Position versions are monotonic per symbol.

---

### Exposure Reservation

An exposure reservation represents exposure capacity allocated to an active quote.

Required fields:

- reservation_id (unique identifier)
- quote_id
- reserved_bid_quantity
- reserved_ask_quantity
- expires_at (must align with the quote TTL)
- state (active, released, expired)

Rules:

- Global exposure usage is computed from active reservations.
- A quote must not be active unless a corresponding reservation is active.
- When a quote is replaced or expires, the associated reservation must be released or expire.

---

## Required Interfaces

### Exchange API

- Submit external order:
  - `POST /orders`
- Publish or replace quote:
  - `PUT /quotes/{symbol}`
- Read active quote (for debugging/verification):
  - `GET /quotes/{symbol}`

Observable requirements:

- The exchange rejects execution against expired quotes.
- Fills reference the quote_id that was executed.
- Partial fills decrement the remaining executable quantity for the active quote.

---

### Trading State API

- Submit fill:
  - `POST /fills`
- Read positions:
  - `GET /positions`
  - `GET /positions/{symbol}`
- Stream or poll committed position updates:
  - the interface must allow market makers to consume committed position updates in order per symbol.

Observable requirements:

- A committed position update is produced only after fills are recorded durably.
- Position versions are monotonic per symbol.

---

### Exposure Reservation API

- Request exposure for a proposed quote:
  - `POST /reservations`
- Update reservation after fill (or equivalent adjustment call):
  - `POST /reservations/{reservation_id}/apply-fill`
- Release reservation for a quote (optional if expiration is sufficient):
  - `POST /reservations/{reservation_id}/release`
- Read current global exposure usage (for debugging/verification):
  - `GET /exposure`

Observable requirements:

- A reservation is either granted, reduced, or rejected according to the exposure rules.
- Retrying the same reservation request must not double-reserve exposure.
- Exposure associated with expired or replaced quotes is eventually released without manual intervention.

---

### Position Display UI Interface

- Read positions:
  - uses the Trading State API (`GET /positions` and/or updates stream)

Observable requirements:

- The UI reflects committed positions only.
- The UI updates as new committed position versions are observed.

---

### Required Logging

Market makers must produce logs sufficient to verify:

- which position version triggered a quote attempt
- whether exposure was granted, reduced, or rejected
- which quote_id became active
- when quotes expired and were refreshed
- how failures and retries converged to correct state
