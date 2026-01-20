# Project Brief: Distributed Mesh VPN

## Overview

In this project, you will design and implement a distributed mesh VPN system composed of a centralized control plane and autonomous node daemons that together form a secure, full-mesh overlay network using WireGuard.

Each node participates equally in the data plane, establishing direct, encrypted, point-to-point tunnels with every other active node. A centralized control plane defines authoritative mesh membership, assigns stable private mesh IPs, distributes cryptographic material, and tracks liveness — but never carries data-plane traffic.

This project emphasizes **distributed systems fundamentals**: membership coordination, convergence, idempotent reconfiguration, failure handling, and clear separation of control and data planes. It does **not** require building custom VPN protocols, kernel networking code, or advanced cryptography.

---

## Core Semantic Rules

These rules define the required behavior of your system.

1. **Full-mesh topology**
   Every participating node must establish a direct WireGuard tunnel to every other active node.

2. **Stable mesh identity**
   Each node is assigned a unique, stable private mesh IP that serves as its identity within the overlay.

3. **Control/data plane separation**

   * The control plane defines membership and distributes state.
   * All encrypted application traffic flows directly between nodes.

4. **Autonomous convergence**
   Nodes independently converge on the correct WireGuard configuration based on control-plane state.

5. **Failure-driven reconfiguration**
   When a node becomes unreachable, remaining nodes must remove or disable its tunnels without manual intervention.

6. **Correctness over performance**
   Deterministic behavior and correctness under change and failure matter more than tunnel setup speed or throughput.

---

## What You Will Build

### 1. Control Plane Service

A centralized coordination service that defines authoritative mesh state.

**Responsibilities**

* Maintain a registry of node metadata, including a human-readable node name or alias
* Assign unique mesh IP addresses
* Store and distribute WireGuard public keys
* Track node liveness via periodic heartbeats
* Expose a documented HTTP API for registration and state retrieval

The control plane never forwards VPN traffic.

---

### 2. Node Daemon

Each node is identified by a stable mesh IP address and may additionally publish a human-readable node name (alias) for display and logging purposes. The node name is advisory metadata only and is not used for routing, addressing, or security decisions.

### 2. Node Daemon

A long-running daemon on each participating node that consumes control-plane state and configures local networking.

**Responsibilities**

* Register with the control plane
* Send periodic heartbeats
* Fetch peer lists, cryptographic material, and peer metadata (including node names)
* Dynamically configure WireGuard interfaces and peers
* Install routing rules mapping mesh IPs to tunnels
* Add, update, or remove peers as membership changes

Each node independently applies configuration changes and converges on the correct local state.

---

### 3. Secure Data Plane

A decentralized encrypted overlay network implemented using WireGuard.

**Responsibilities**

* Authenticated, encrypted point-to-point communication
* Static `AllowedIPs` mappings between mesh IPs and peers
* Operation independent of underlying LANs, WANs, or NATs

Once configured, the data plane operates without centralized coordination.

---

### 4. Messaging Demonstration Application (Data Plane Validation)

To demonstrate that the mesh VPN supports real application traffic, you will build a small messaging demonstration application that runs on top of the mesh network.

This application exists solely as a demonstration artifact and is not part of the VPN system itself.

**Purpose**

* Validate that applications can communicate directly over mesh IPs
* Demonstrate correct routing, tunnel configuration, and failure behavior
* Provide a concrete, user-visible proof that the VPN functions as intended

**Functional Requirements**

* The application runs on each participating node
* The UI displays:

  * A list of currently connected peers (left pane), showing each peer’s node name (alias) and mesh IP address
  * A message view for a selected peer
* Selecting a peer allows the user to send messages directly to that peer
* Messages are sent point-to-point using mesh IPs
* Communication uses standard networking (e.g., TCP or UDP)
* The application relies entirely on the VPN for connectivity

**Implementation Notes**

* The messaging application may be generated using an AI-assisted development tool
* Students are responsible for integration, configuration, and explanation
* The application must use mesh IPs for all network communication; node names are for display only and must not be used for addressing or routing
* No centralized relay or server may be used

The messaging application is evaluated only as evidence that the VPN data plane functions correctly.

---

## Distributed Systems Challenges You Must Address

You are expected to **design, implement, and explain** how your system handles:

* Membership registration and convergence
* Heartbeat-based liveness detection
* Dynamic reconfiguration under membership change
* Node crashes and restarts
* Partial failures and stale state
* Idempotent re-application of configuration
* Control-plane unavailability vs data-plane stability
* Demonstrating application-layer communication over a dynamically changing mesh network
* Observability through logs and system state inspection

---

## Required System Diagrams

You must provide **three diagrams** during the project:

1. **Initial Architecture Diagram** (early)
   Components, APIs, and data flow.

2. **Distributed Coordination Diagram** (mid-project)
   Control plane, nodes, liveness, and failure paths.

3. **Final As-Built Diagram** (end of project)
   Matches the implemented system exactly.

Diagrams must reflect **running systems**, not aspirational designs.

---

## What Is Explicitly Out of Scope

To keep the project focused and achievable, you will *not* implement:

* Multi-hop routing or packet forwarding
* Broadcast or multicast traffic
* Custom VPN protocols
* Advanced NAT traversal mechanisms
* Kernel-space networking code
* Policy-based routing or access control systems
* UI dashboards beyond the messaging demo

---

## Expected Deliverables

### 1. Working System

* Multi-node mesh VPN
* Centralized control plane
* Autonomous node daemons
* Dynamic membership handling
* Secure peer-to-peer communication

### 2. Design & Scope Documents

* Initial scope document
* Updated scope reflecting implemented behavior
* Final design document describing:

  * Architecture
  * Membership and liveness model
  * Failure behavior
  * Reconfiguration semantics
  * Guarantees and limitations

### 3. Test & Demo Artifacts

* Scripts for multi-node deployment
* Failure-injection demonstrations
* Logs showing convergence and recovery
* Messaging application used to validate data-plane behavior

### 4. Final Demo

A live demonstration showing:

* Nodes joining and leaving the mesh
* Automatic tunnel creation and teardown
* Failure detection and recovery
* Application-level messaging succeeding and failing appropriately

---

## Team Size and Time Expectations

This project is designed for **3 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
