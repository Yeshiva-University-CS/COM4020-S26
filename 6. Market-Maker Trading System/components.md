## Components You Will Build

You will build a market-maker trading system composed of running services and a small set of required models and interfaces. Authority boundaries must be explicit and unambiguous.

---

## A. Running Components (Processes)

### 1. Exchange Simulator Service

A long-running service that simulates an exchange for a fixed set of symbols.

It will:

* Maintain a deterministic reference price per symbol
* Accept order submission and cancellation requests
* Match orders deterministically
* Emit fill events with unique identifiers
* Generate deterministic external orders
* Expose inspection endpoints for prices, quotes, and fills

The exchange is the sole authority for reference prices and fill creation.

---

### 2. Trading State Service

A long-running service responsible for authoritative shared state.

It will:

* Record fills durably and idempotently
* Maintain authoritative positions per symbol
* Expose inspection endpoints for current positions and fill history
* Ensure each fill affects position exactly once

No market-maker node owns or mutates positions directly.

---

### 3. Authority and Membership Coordination Service

A long-running service responsible for node presence and symbol authority.

It will:

* Track active nodes based on time-bounded presence renewal
* Maintain authoritative symbol-to-node responsibility assignments
* Grant exclusive, time-bounded authority per symbol
* Ensure at most one node holds authority for a symbol at a time
* Expire authority automatically when renewal stops
* Recompute and apply symbol redistribution on node join, leave, or failure
* Expose inspection endpoints for membership, assignments, and authority state

This service is the authority for who may act.

---

### 4. Market-Maker Node Service

A long-running service that publishes quotes for assigned symbols.

It will:

* Observe current symbol responsibility assignments
* Attempt to acquire and renew authority for assigned symbols
* Query the exchange for reference prices
* Read authoritative positions from the Trading State Service
* Compute deterministic bid/ask quotes
* Publish one active quote per symbol
* Replace the quote immediately after a fill
* Refresh quotes when observed state changes and at least once every 5 seconds
* Verify authority immediately before issuing any cancel or replace
* Resume safely after crash or restart

Market-maker nodes may compute quotes at any time, but may act only while authorized.

---

### 5. Operator Interface and GUI

A user-facing component that provides observability.

It will:

* Display current positions per symbol
* Show recent fills and quote updates
* Indicate current authority holder per symbol
* Reflect changes during failure and rebalancing

The GUI is read-only and does not influence system behavior.

---

## B. Required Models and Definitions

You will define models for:

* Symbols
* Quotes (bid, ask, quantity)
* Orders and cancellations
* Fills (with unique identifiers)
* Positions
* Authority grants and expirations
* Node presence and membership
* Symbol responsibility assignments

---

## C. Required Interfaces

You will define interfaces for:

* Submitting and canceling orders
* Querying reference prices
* Emitting and consuming fills
* Recording and querying positions
* Registering and renewing node presence
* Acquiring and renewing symbol authority
* Inspecting authority, membership, and assignments

Interfaces must make authority boundaries explicit.
