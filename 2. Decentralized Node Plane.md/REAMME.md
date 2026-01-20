# Project Brief: Decentralized Node Plane  
**(Ledger-Backed Control Plane for a Distributed Mesh VPN)**

## Overview

In this project, you will design and implement a Decentralized Node Plane: a small but rigorous distributed system responsible for node discovery, identity, health, and control-plane coordination across a peer-to-peer mesh.

This project is designed to converge with the Distributed Mesh VPN project by replacing its control plane. Concretely: the Mesh VPN’s data plane (tunneling / forwarding) is assumed to exist separately, and your work provides the control plane services needed for nodes to form and maintain the mesh.

A key requirement is to abstract control-plane communication behind a clean interface so that the Mesh VPN can swap control planes without rewriting its core logic. Your system must therefore define and implement:
- a Control Plane Interface (the abstraction),
- a reference implementation of that interface (your decentralized node plane),
- and a compatibility mode sufficient for integration tests with the Mesh VPN.

As part of the control plane, the system incorporates a distributed ledger to record membership events and control-plane state transitions, borrowing core ideas from blockchain systems such as append-only history, tamper-evident linkage, and eventual convergence across replicas. The goal is *not* to implement heavyweight blockchain consensus, but to apply ledger-based reasoning to distributed coordination.

This project emphasizes **distributed systems fundamentals**: membership, gossip vs coordination, failure detection, interface-driven design, ledger semantics, and correctness under partial failure.  
It does **not** require packet routing, NAT traversal, cryptography research, or high-performance data-plane engineering.

---

## Core Semantic Rules

These rules define the required behavior of your system.

1. **Node identity is stable and self-asserted**  
   Each node has a stable `node_id` derived from local configuration. Identity is never assigned remotely.

2. **No central authority**  
   The system must not rely on a single coordinator or bootstrap service after initial startup.

3. **Control plane is interface-driven**  
   The Mesh VPN must interact with the control plane only through a defined interface. The interface must be stable, documented, and testable with a stub implementation.

4. **Convergent membership view**  
   Nodes must eventually converge on the same membership view in the absence of new failures.

5. **Ledger-backed state evolution**  
   Membership changes and control-plane decisions are recorded in an append-only ledger. Nodes reason about current state as a function of ledger history rather than mutable global state.

6. **Failure visibility without certainty leakage**  
   Nodes may suspect failures, but must not assert global certainty without time-based convergence and/or quorum-style confirmation rules (as documented).

7. **Correctness over speed**  
   The system must prioritize determinism, explainability, and safety over rapid convergence.

---

## What You Will Build

### 1. Control Plane Interface (Required)

A language-level interface (or protocol) that abstracts how the Mesh VPN communicates with any control plane.

At minimum, the interface must support:

- `join(seed_peers) -> membership_view`
- `leave()`
- `get_members() -> membership_view`
- `publish_heartbeat()`
- `report_peer_state(peer_id, state)`
- `subscribe_membership_updates(handler)`
- `get_peer_metadata(peer_id)` *(optional but useful for VPN integration)*

You will provide:
- a written spec (inputs/outputs, error semantics),
- a stub/mock implementation for testing,
- your decentralized node plane as the real implementation.

---

### 2. Node Plane Agent (Reference Implementation)

A long-running agent that runs on each node and implements the control plane interface.

- Maintains local identity and configuration
- Communicates with peers over a control channel (HTTP, TCP, or QUIC)
- Exposes introspection API (`/status`, `/members`, `/ledger/head`)
- Emits structured logs and ledger entries for all control-plane decisions

---

### 3. Membership & Discovery Protocol

A decentralized protocol for node discovery and membership maintenance.

- Static seed list for first contact
- Dynamic peer discovery after bootstrap
- Periodic membership exchange (gossip or anti-entropy)
- Membership states: `alive`, `suspect`, `dead`

---

### 4. Ledger-Based State Tracking

A local, append-only ledger maintained by each node.

- Records joins, leaves, suspicions, confirmations, and recoveries
- Ledger entries are immutable and ordered
- State is derived by replaying the ledger
- Ledgers converge across nodes via exchange and reconciliation
- Ledger linkage should be tamper-evident (e.g., hash-chain style linking)

---

### 5. Failure Detection

A failure-detection mechanism with explicit semantics.

- Heartbeats or probes
- Tunable suspicion thresholds
- Deterministic transitions between membership states
- Clear documentation of false-positive behavior

---

### 6. Control-Plane Messaging

A structured control-plane message format.

- Join / leave announcements
- Membership and ledger updates
- Failure suspicion propagation
- Versioned message schema

---

### 7. Local Event Log

Each node maintains an append-only local event log.

- Aligned with the ledger but optimized for observability
- Used for debugging, demos, and correctness validation
- Not replicated

---

## Distributed Systems Challenges You Must Address

You are expected to **design, implement, and explain** how your system handles:

- Interface-driven integration with the Mesh VPN
- Cluster bootstrapping without central coordination
- Membership convergence using ledger reconciliation
- Failure detection vs false positives
- Ledger divergence and reconciliation
- Split-brain scenarios
- Partial network partitions
- Restart behavior and re-joins
- Observability of distributed behavior

---

## System Diagrams (Required)

You must provide **three diagrams** at specific milestones:

1. **Initial Architecture Diagram** (Week 2)  
   Components, Mesh VPN ↔ Control Plane Interface boundary, and ledger placement.

2. **Membership, Ledger & Failure Model Diagram** (Week 6)  
   Node states, message flow, reconciliation, and failure assumptions.

3. **Final As-Built Diagram** (Week 12)  
   Reflects what actually runs, including the integration boundary.

Diagrams must reflect **running systems**, not aspirational designs.

---

## What Is Explicitly Out of Scope

To keep the project focused and achievable, you will *not* implement:

- VPN data plane (tunneling, forwarding, NAT traversal)
- Cryptographic key exchange or PKI
- Cryptocurrency or token economics
- Proof-of-work / proof-of-stake consensus
- Performance-optimized gossip protocols
- UI dashboards
- Kubernetes or existing service meshes

---

## Expected Deliverables

### 1. Working System

- Control Plane Interface spec + stub
- Multi-node node-plane agents implementing the interface
- Decentralized membership and discovery
- Ledger-backed state evolution
- Failure detection with documented semantics
- Deterministic behavior under failure
- Integration demo with the Mesh VPN (control plane replaced)

---

### 2. Design & Scope Documents

- Initial scope document
- Updated scope reflecting actual behavior
- Final design document describing:
  - Interface contract and integration boundary
  - Membership model
  - Ledger structure and reconciliation
  - Failure semantics
  - Convergence guarantees
  - Known limitations

---

### 3. Test Suite

- Interface conformance tests (stub vs real)
- Membership convergence tests
- Ledger reconciliation tests
- Failure injection tests
- Restart and rejoin tests
- Partition simulation (where feasible)

---

### 4. Demo

A live demonstration showing:

- Mesh VPN uses the control plane interface (not hardwired networking calls)
- Nodes join and form membership through your decentralized node plane
- Membership events recorded in the ledger
- Failure detection and suspicion propagation
- Membership convergence via ledger reconciliation
- Recovery after restart
- Control plane swap demonstrated (stub → real)

---

## Team Size and Time Expectations

This project is designed for **3 students**, working approximately **10 hours per week per student** over **14 weeks**.  
Every milestone requires a **running artifact** and documentation that reflects reality.
