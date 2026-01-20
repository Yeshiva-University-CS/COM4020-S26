# Project Brief: Decentralized Node Plane  
**(Ledger-Backed Control Plane for a Distributed Mesh VPN)**

## Overview

In this project, you will design and implement a Decentralized Node Plane: a distributed control plane responsible for node discovery, identity, health, and membership coordination across a peer-to-peer mesh.

This project is designed to converge with the Distributed Mesh VPN project by replacing its control plane. The Mesh VPN’s data plane (tunneling and packet forwarding) is assumed to exist separately. Your system provides the control-plane services required for nodes to form, maintain, and heal the mesh.

A core requirement is to abstract all control-plane communication behind a well-defined interface, allowing the Mesh VPN to operate independently of any specific control-plane implementation. Your decentralized node plane will serve as the reference implementation of this interface.

This interface must be designed and agreed upon in conjunction with the Mesh VPN team. Both teams are expected to collaborate on the interface contract, including method signatures, semantics, and failure behavior, so that the Mesh VPN depends *only* on the interface and not on any implementation details of the control plane.

As part of the control plane, the system incorporates a ledger abstraction inspired by blockchain systems. The ledger provides an append-only, tamper-evident history of membership and control-plane events and supports eventual convergence across nodes. The goal is *not* to implement a cryptocurrency or heavyweight blockchain protocol, but to apply ledger-based reasoning to distributed coordination.

This project emphasizes **distributed systems fundamentals**: interface-driven design, membership, failure detection, convergence, ledger semantics, and correctness under partial failure.  
It does **not** require VPN data-plane work, cryptographic protocol design, or performance optimization.

---

## Core Semantic Rules

These rules define the required behavior of your system.

1. **Node identity is stable and self-asserted**  
   Each node has a stable `node_id` derived from local configuration. Identity is never assigned remotely.

2. **No central authority**  
   The system must not rely on a single coordinator or bootstrap service after initial startup.

3. **Control plane is interface-driven**  
   The Mesh VPN must interact with the control plane *only* through a defined interface. The interface must be stable, documented, and testable via a stub implementation.

4. **Convergent membership view**  
   Nodes must eventually converge on the same membership view in the absence of new failures.

5. **Ledger-backed state evolution**  
   Control-plane state is derived from an append-only ledger of events. Nodes reason about current state by replaying ledger history rather than mutating shared global state.

6. **Failure visibility without certainty leakage**  
   Nodes may suspect failures, but must not assert global certainty without documented convergence rules.

7. **Correctness over performance**  
   Determinism, explainability, and safety are more important than speed.

---

## What You Will Build

### 1. Control Plane Interface (Required)

A language-level interface that abstracts how the Mesh VPN interacts with any control plane.

At minimum, the interface must support:

- `join(seed_peers)`
- `leave()`
- `get_members()`
- `publish_heartbeat()`
- `subscribe_membership_updates(handler)`
- `get_peer_metadata(peer_id)` *(optional but useful)*

You must provide:
- a written interface specification,
- a stub/mock implementation for testing,
- and a real implementation backed by your decentralized node plane.

---

### 2. Node Plane Agent (Reference Implementation)

A long-running agent that runs on each node and implements the control plane interface.

- Maintains node identity and configuration  
- Communicates with peers over a control channel (HTTP, TCP, or QUIC)  
- Exposes introspection endpoints (`/status`, `/members`, `/ledger`)  
- Emits structured logs and ledger-derived events for all control-plane decisions  

---

### 3. Membership & Discovery Protocol

A decentralized protocol for discovering and tracking nodes.

- Static seed list for bootstrap  
- Dynamic peer discovery after join  
- Periodic membership exchange (gossip or anti-entropy)  
- Membership states: `alive`, `suspect`, `dead`  

---

### 4. Ledger Integration

A ledger component (Hyperledger Fabric) providing an append-only, tamper-evident history of control-plane events.

- Records joins, leaves, suspicions, confirmations, and recoveries  
- Entries are immutable and ordered  
- State is derived via replay  
- Ledgers converge across nodes through exchange and reconciliation  
- Ledger design is inspired by blockchain systems but avoids cryptocurrency or consensus-heavy protocols  

---

### 5. Failure Detection

A failure-detection mechanism with explicit semantics.

- Heartbeats or probes  
- Tunable suspicion thresholds  
- Deterministic state transitions  
- Clear documentation of false positives and limitations  

---

### 6. Local Event Log

Each node maintains a local append-only event log.

- Optimized for observability and debugging  
- Aligned with (but distinct from) the ledger  
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
- Restart and rejoin behavior  
- Observability and postmortem analysis  

---

## System Diagrams (Required)

You must provide **three diagrams**:

1. **Initial Architecture Diagram** (Week 2)  
   Includes Mesh VPN ↔ Control Plane Interface boundary.

2. **Membership, Ledger & Failure Model Diagram** (Week 6)

3. **Final As-Built Diagram** (Week 12)

Diagrams must reflect **running systems**, not aspirational designs.

---

## What Is Explicitly Out of Scope

- VPN data plane (tunneling, routing, NAT traversal)  
- Cryptographic key exchange or PKI  
- Cryptocurrency or token economics  
- Proof-of-work / proof-of-stake consensus  
- Performance-optimized gossip  
- UI dashboards  
- Kubernetes or existing service meshes  

---

## Expected Deliverables

1. **Working System**
   - Control Plane Interface + stub  
   - Multi-node node plane agents  
   - Ledger-backed membership and failure handling  
   - Mesh VPN integration using the interface  

2. **Design & Scope Documents**
   - Initial scope  
   - Updated scope reflecting reality  
   - Final design document (architecture, semantics, limitations)  

3. **Test Suite**
   - Interface conformance tests  
   - Membership convergence tests  
   - Failure injection tests  
   - Restart/rejoin tests  

4. **Demo**
   - Control plane replacement demonstrated  
   - Failure and recovery shown through VPN integration  

---

## Team Size and Time Expectations

This project is designed for **4 students**, working approximately **10 hours per week per student** over **14 weeks**.  
All documentation must describe **observed behavior**, not intended design.
