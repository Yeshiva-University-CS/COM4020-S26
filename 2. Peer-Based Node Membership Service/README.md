# Peer-based Node Membership Service

## Project Overview

In this project, you will design and implement a peer-based node membership service that allows machines to discover each other, track membership, detect failure, and recover — without relying on any single central coordination service.

Each machine runs a background service that communicates directly with peers to establish a shared view of which machines are currently part of the system and which are reachable. Nodes may join and leave the system voluntarily at any time, and may also become unreachable due to failure. The system must handle all of these cases correctly.

Nodes exchange state, detect failures, and converge on a consistent membership view over time.

The system exposes a clean, well-defined interface that another system can use to obtain membership and liveness information without knowing how coordination is implemented internally.

The focus of the project is on distributed systems behavior: agreement on membership, correct handling of voluntary departure and unexpected failure, convergence after disruption, and clear, observable behavior under change.

You are not expected to implement production-grade discovery mechanisms, security frameworks, or performance optimizations. The emphasis is on correctness and explainable behavior in a failure-prone distributed system.

---
### 1. [Components](components.md)
### 2. [Deliverables & Milestones](milestones.md)
---

## Resulting System Behavior

When the system is running correctly:

* Multiple machines form a peer-based coordination group
* Machines may join and leave the system voluntarily at any time
* Each machine maintains a local view of current membership
* Membership updates propagate without a central authority
* Failed or unreachable machines are detected and reflected in membership state
* Restarted or rejoining machines converge without manual cleanup
* External systems can retrieve membership and liveness through a stable interface
* System behavior under change and failure is visible through logs and state inspection

---

## Distributed Systems Challenges You Will Need to Address

You are expected to design, implement, and explain how your system handles:

* Discovering peers and forming an initial group
* Voluntary node joins and voluntary departures
* Tracking which machines are currently part of the system
* Detecting when machines are alive, unreachable, or have returned
* Distinguishing clean departure from unexpected failure
* Updating membership as machines join, leave, fail, or restart
* Converging on a consistent membership view after disruption
* Operating without a single always-on coordinator
* Exposing coordination state through a clean interface
* Dealing with temporarily outdated or incomplete information
* Making system behavior observable through logs and inspection of state

---

## Team Size and Time Expectations

This project is designed for **4 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
