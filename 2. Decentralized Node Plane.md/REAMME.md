# Project Proposal: Decentralized Node Control Plane

### Summary

This project develops a generic, decentralized control plane for node identity and membership management, backed by a replicated, consensus-driven distributed ledger. The control plane maintains node identities, membership changes, revocations, and application-defined metadata in a tamper-resistant, append-only log, providing a consistent source of truth without reliance on a centralized authority. Nodes interact with the system through a well-defined client interface that supports registration, membership retrieval, and event-driven updates. As a required component of the project, the control plane is consumed by the VPN mesh project, demonstrating its ability to serve as a reusable, decentralized backend.

---

## Deliverables

- A functioning decentralized node control plane backed by a Fabric cluster  
- A complete control-plane client library with documented interfaces  
- A node-side agent supporting registration, membership retrieval, event-driven updates, and distributed coordination  
- Chaincode implementing the on-chain data model and validation logic  
- An integration module that allows the VPN mesh system to consume the control plane as its authoritative backend for identity, membership, and trust management  
- A working demonstration showing the VPN mesh dynamically configuring itself based on control-plane state and events  
- Demonstrations of membership convergence, distributed coordination, revocation handling, and failure recovery  

---

## Core Components

### 1. Control Plane Client

A client-side library that exposes a generic, application-agnostic interface for interacting with the decentralized control plane.

**Responsibilities:**
- Submitting transactions to the distributed ledger  
- Reading ledger state and interpreting stored metadata  
- Subscribing to block and state-change events for real-time updates  
- Coordinating the claiming of unique, application-defined identifiers using ledger-based conflict resolution  

This component abstracts all ledger-specific interactions behind a stable interface.

---

### 2. Node-Side Agent

A lightweight daemon running on each participating node that consumes the control plane client.

**Responsibilities:**
- Publishing node identity and metadata to the ledger  
- Claiming unique identifiers through distributed coordination  
- Retrieving membership state and peer metadata  
- Detecting revocations and stale or inconsistent state  
- Maintaining a consistent local view of decentralized control-plane state  

The agent reacts to ledger events and updates local state accordingly.

---

### 3. On-Chain Data Model (Chaincode)

A deterministic smart-contract module deployed to the distributed ledger.

**Responsibilities:**
- Defining representations for identity, membership, metadata, and revocation events  
- Validating updates and enforcing invariants  
- Ensuring all state transitions are consistent, auditable, and verifiable  
- Maintaining an append-only record of all control-plane changes  

All control-plane state transitions are governed by chaincode logic.

---

### 4. VPN Control Plane Connector

An integration component that connects the decentralized control plane to the VPN mesh system, enabling the VPN control plane to use the decentralized controller as its authoritative backend. This integration is implemented and committed in the **VPN repository**, not the control-plane repository.

**Responsibilities:**
- Translating control-plane membership and identity state into VPN-specific configuration inputs  
- Subscribing to control-plane events to trigger VPN reconfiguration  
- Ensuring VPN membership reflects revocations and membership changes in the decentralized log  
- Preserving separation of concerns: the control plane remains generic; the VPN applies its own semantics  

---

## Required External Infrastructure

A pre-existing deployment provides the distributed ledger substrate for the control plane.

The Fabric cluster includes:
- **Peers** that store the replicated ledger and execute chaincode  
- **Orderers** that provide consensus and global transaction ordering  
- A **Certificate Authority** for identity issuance and enrollment  
- **Event streams** for block and state-change notifications  

The Fabric cluster serves exclusively as the **replicated, authoritative source of truth** for all control-plane state.

---

## Distributed Systems Behaviors

- **Membership convergence**  
  Nodes independently observe ledger updates and converge on a consistent view  

- **Distributed coordination**  
  Unique identifiers are claimed without a central allocator  

- **Event-driven reconfiguration**  
  Nodes react to ledger events and update local state  

- **Failure handling**  
  Nodes reconcile stale state after crashes, restarts, or network partitions  

- **Distributed identity lifecycle**  
  Identity publication, updates, and revocations are consistently processed  

- **Control/data plane separation**  
  The control plane defines authoritative state; consuming systems act independently  

---

## Constraints (not in scope)

- The distributed ledger is external infrastructure and used solely as a substrate  
- The control plane must remain generic and application-agnostic  
- No application-specific networking, routing, or transport logic is included  
- The ledger backend must remain general-purpose and free of embedded application semantics  
