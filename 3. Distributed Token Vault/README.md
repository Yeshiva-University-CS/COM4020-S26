# Project Brief: Distributed Token Vault for PHI Tokenization

## Overview

In this project, you will design and implement a small distributed system that replaces sensitive information with deterministic tokens and stores the mappings in a replicated token vault. The system must behave consistently across multiple nodes, tolerate failures, and support controlled detokenization using owner-scoped isolation.

The system exposes a simple HTTP API for tokenization and detokenization and includes a lightweight integration that allows DuckDB to join tokenized analytics tables with your service to recover original values when permitted.

This project emphasizes **distributed systems fundamentals**: replication, consistency, coordination, idempotency, failure handling, and correctness under concurrency. It does **not** require healthcare expertise, NLP, or complex security or policy frameworks.

---

## Core Semantic Rules

These rules define the required behavior of your system and drive grading.

1. **Owner-scoped tokens**  
   Tokens are scoped by `owner_id`. The same plaintext tokenized by different owners must produce different tokens.

2. **Authentication derives identity**  
   Requests are authenticated using an API key. The server derives the caller’s `owner_id` from authentication and does not trust `owner_id` supplied by the client.

3. **No cross-owner existence leakage**  
   Detokenization failures must return **not found** when a token does not exist for the authenticated owner. The system must never reveal whether a token exists for a different owner.

4. **Token epochs and rotation readiness**  
   Tokens must be self-describing with respect to epoch (the epoch is encoded in the token value, e.g., a prefix).  
   The system maintains an active token epoch per owner. Tokenization always uses the active epoch; detokenization must support tokens issued under older epochs.  
   The system exposes a controlled API operation to rotate the active epoch for an owner (monotonic, idempotent).  

5. **Correctness over performance**  
   The system must prioritize correctness, determinism, and well-defined behavior under failure over throughput or latency.

---

## What You Will Build

### 1. Tokenization API Service

A stateless HTTP service that accepts structured JSON records and returns tokenized versions.

- Endpoints: `/tokenize`, `/detokenize`, `/health`, and an epoch rotation endpoint (e.g., `/rotate-epoch`)
- Deterministic token generation for configured fields
- Batch detokenization for analytics workflows
- Authentication via API key
- All requests execute under the authenticated owner’s identity

---

### 2. Deterministic Token Generator

A module that produces stable, opaque tokens.

- Same input → same token for the same owner and epoch
- Different owners and/or epochs → different tokens for the same plaintext
- Collision-resistant
- Reversible only through the vault

---

### 3. Distributed Token Vault

A replicated key-value store mapping tokens to original values.

- Three-node deployment
- Leader-based or quorum-based consistency (your choice)
- Idempotent writes
- Duplicate token issuance prevented
- Vault lookups always scoped by `(owner_id, epoch, token)`
- Must remain available if one node fails

The vault is the authoritative source of truth for reversibility and isolation.

---

### 4. Configuration File

A static configuration file defining which JSON fields should be tokenized.

- Loaded at startup
- No dynamic updates
- No schema inference

---

### 5. Local Event Log

Each node maintains an append-only log of tokenization, detokenization, and epoch-rotation events.

- Used for observability, debugging, and demonstrations
- Not replicated
- Not intended for compliance or auditing systems

---

### 6. DuckDB Integration

A small client-side utility or table function that allows DuckDB to perform batch detokenization.

- Sends tokens to the service under authenticated identity
- Returns original values when permitted
- Demonstrates joining tokenized analytics data with recovered values

---

## Distributed Systems Challenges You Must Address

You are expected to **design, implement, and explain** how your system handles:

- Replication of vault state and active epoch across nodes
- Deterministic behavior under concurrency
- Idempotent operations and safe retries
- Node crashes and restarts
- Replication lag and degraded modes
- Isolation correctness under concurrency and failure
- Observability of system behavior through logs

---

## System Diagrams (Required)

You must provide **three diagrams** at specific milestones:

1. **Initial Architecture Diagram** (Week 2)  
   Shows system components and data flow.

2. **Distributed Coordination Diagram** (Week 6)  
   Shows nodes, replication model, read/write paths, and failure assumptions.

3. **Final As-Built Diagram** (Week 12)  
   Documents what actually runs and matches your implementation.

Diagrams must reflect **running systems**, not aspirational designs. Simple boxes-and-arrows notation is sufficient.

---

## What Is Explicitly Out of Scope

To keep the project focused and achievable, you will *not* implement:

- Free-text de-identification or NLP
- Healthcare-specific schemas (e.g., FHIR)
- Multi-tenant policy systems
- Role-based access control
- Format-preserving tokenization
- Encryption key management services
- Distributed audit logs
- User interfaces or dashboards
- Cross-region replication

---

## Expected Deliverables

### 1. Working System
- Multi-node token vault with replication
- Tokenization and detokenization API
- Owner-scoped isolation with not found semantics
- Epoch rotation via API and multi-epoch detokenization support
- DuckDB integration for batch detokenization

### 2. Design & Scope Documents
- Initial scope document (early)
- Updated scope reflecting implemented behavior
- Final design document describing:
  - Architecture
  - Consistency model
  - Failure behavior
  - Epoch rotation semantics
  - Guarantees and limitations

### 3. Test Suite
- Unit tests for core logic
- Concurrency tests
- Failure-injection tests (e.g., killing a node mid-request)
- Rotation tests (e.g., rotate epoch, confirm new tokens differ, old tokens still detokenize)

### 4. Demo
A live demonstration showing:
- Multi-node behavior
- Correctness under failure
- Isolation correctness
- Epoch rotation and migration behavior on a small dataset
- DuckDB integration

---

## Team Size and Time Expectations

This project is designed for **3 students**, working approximately **10 hours per week per student** over **14 weeks**.  
Every milestone requires a **running, deployable artifact**; design documents must describe what actually runs.
