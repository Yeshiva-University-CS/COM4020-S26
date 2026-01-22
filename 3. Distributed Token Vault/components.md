## Components You Will Build

You will build a distributed token vault system. It includes running services and a small set of required definitions that make system behavior clear and usable by other systems.

---

## A. Running Components (Processes)

### 1. Tokenization API Service

A long-running HTTP service that accepts requests for tokenization and detokenization.

It will:

* Accept structured JSON input
* Deterministically compute tokens for configured fields using the active epoch
* Authenticate requests and derive the caller’s identity
* Query authoritative epoch state as needed
* Submit proposed token mappings to the vault
* Accept detokenization requests for previously issued tokens
* Enforce owner-scoped isolation on reads
* Expose basic health and status endpoints

The API computes token identity but does not determine token existence or epoch authority.

---

### 2. Token Vault Service

A long-running service responsible for authoritative storage of token mappings and epoch state.

It will:

* Validate and persist proposed token mappings
* Enforce insert-once semantics for token issuance
* Maintain a single active epoch per owner
* Validate and persist epoch changes
* Replicate token mappings and epoch state to peer vault instances
* Serve detokenization requests based on authoritative state
* Remain correct under partial failure
* Recover state on restart

All token mappings and epoch state are fully replicated across vault nodes. There is no key partitioning or rebalancing.

---

## B. Required Artifacts (Not Processes)

These are not separate running services. They are required artifacts that define what your running system means and how other systems interact with it.

### 3. Token and Isolation Model

A shared behavioral model implemented consistently across all components.

It defines:

* How tokens are computed deterministically
* How tokens are scoped to an owner and epoch
* What it means for an epoch to be active
* When a token is considered to exist
* Insert-once semantics for token issuance
* How epoch changes affect subsequent tokenization
* How isolation is enforced during detokenization
* How retries and concurrent requests are handled

The model must be documented and observable in practice.

---

### 4. External API Contract

A small, explicit interface describing how clients interact with the system.

It will:

* Define request and response formats
* Specify error and *not found* behavior
* Describe determinism, epoch, and isolation guarantees
* Hide internal replication and coordination mechanics

The contract exists to decouple system behavior from implementation details.
