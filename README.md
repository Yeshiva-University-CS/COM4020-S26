# Project Proposals

### 1. [Distributed Mesh VPN](1.%20Distributed%20Mesh%20VPN/README.md#distributed-mesh-vpn)
### 2. [Peer-based Node Membership Service](2.%20Peer-Based%20Node%20Membership%20Service/README.md#peer-based-node-membership-service)
### 3. [Distributed Token Vault](3.%20Distributed%20Token%20Vault/README.md#distributed-token-vault)
### 4. [Distributed Secrets Vault](4.%20Distribtued%20Secrets%20Vault/README.md#distributed-secrets-vault)
### 5. [Video Upload & Streaming System](5.%20Video%20Upload%20&%20Streaming%20System/README.md#video-upload-and-streaming-system)
### 6. [Market-Maker Trading System](6.%20Market-Maker%20Trading%20System/README.md#market-maker-trading-system)

---
---

# Expected Project Deliverables

Each team is expected to produce running systems, clear documentation, and observable evidence of correct behavior.

---

## 1. Working System

A functioning distributed system that meets the core requirements of the project.

This includes:

* A multi-node deployment
* Clearly defined system components with distinct responsibilities
* Correct behavior under normal operation
* Correct behavior as components start, stop, fail, and recover

The system must be runnable, demonstrable, and reproducible by the course staff.

---

## 2. Design and Scope Documents

A set of documents that describe what the system is intended to do and what it actually does.

This includes:

* An initial scope document describing goals, assumptions, and non-goals
* An updated scope document reflecting implemented behavior
* A final design document describing:

  * Overall architecture
  * Core coordination and consistency model
  * Failure and recovery behavior
  * Reconfiguration semantics
  * Guarantees, limitations, and known tradeoffs

Documentation must accurately describe the running system, not an aspirational design.

---

## 3. Test and Demo Artifacts

Artifacts that demonstrate correctness, robustness, and observability.

This includes:

* Scripts or instructions for deploying the system across multiple nodes
* Demonstrations of failure scenarios and recovery behavior
* Logs or traces that explain system behavior over time
* A small demonstration application or workload used to validate end-to-end behavior

These artifacts should make system behavior visible and explainable.

---

## 4. Final Demo

A live demonstration of the running system.

The demo must show:

* Normal operation across multiple nodes
* Dynamic changes such as nodes joining, leaving, or failing
* Automatic recovery without manual intervention
* Application-level behavior continuing or failing in expected ways

The final demo should clearly connect observed behavior to the system’s design and stated guarantees.
