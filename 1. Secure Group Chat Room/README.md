# Secure Group Chat Room

## Project Overview

In this project, you will design and implement a decentralized group chat system that runs with no central chat server.

Each node runs the same program and communicates directly with other nodes over a provided private network. Together, the nodes form a single shared group chat room and support point-to-point private messages. There is no server that owns the room, tracks membership, or routes messages once the system is running.

Nodes may join, leave, crash, restart, or become temporarily unreachable. The system must continue operating and eventually recover to a consistent state. When messages are delivered, missed, duplicated, or recovered after failure, the system must make those outcomes visible and explainable.

The system is intentionally structured into **three subsystems**, each with a clear responsibility and a clear boundary. No subsystem may rely on a hidden leader, coordinator, or centralized service.

---

## Subsystems and Responsibilities

### Subsystem 1 — Node Runtime and Communication

This subsystem is responsible for running the node and moving messages between nodes.

It ensures that:
- Each node has a stable identity
- A restarted node can be recognized as the same participant
- Messages can be sent to and received from peers
- Local activity is visible through logs and status output

This subsystem does **not** decide who is a member of the system and does **not** define chat behavior. It only provides the basic ability for nodes to run, communicate, and explain what they are doing.

---

### Subsystem 2 — Membership and Presence

This subsystem is responsible for determining who appears to be participating in the system.

It ensures that:
- Nodes learn about each other through peer-to-peer communication
- Nodes track which peers seem reachable or unreachable
- Crashes and restarts are handled safely
- Membership views may temporarily differ but eventually converge

There is no central membership list. Membership is based on observation and evidence, not authority. This subsystem does not guarantee instant or perfect failure detection.

---

### Subsystem 3 — Messaging and Recovery

This subsystem is responsible for all chat behavior.

It ensures that:
- There is a single shared group chat room
- Nodes can send private messages directly to one another
- Duplicate messages are handled safely
- Message delivery claims are based on evidence, not assumption

Group messages are sent directly to peers and may be partially delivered if a node fails mid-send. After a crash or restart, a node may ask peers for recent group messages it missed. This recovery is best-effort and bounded; missing messages are allowed and must be visible.

Private messages use acknowledgments and timeouts so that delivery outcomes are explicit and explainable.

This subsystem does not promise global ordering, universal delivery, or permanent message history.

---
### 1. [Components](components.md)
### 2. [Deliverables & Milestones](milestones.md)
---

## Resulting System Behavior

When the system is running correctly:

* Nodes participate as equals with no central server
* Membership converges over time after joins, failures, and restarts
* A single shared group chat room is available
* Group messages may be partially delivered under failure
* Duplicate messages are safely handled
* Private messages use acknowledgment and timeout semantics
* Nodes can recover recent group messages after restart
* Recovery may be partial and must be visible
* Logs and inspection output explain all outcomes

---

## Distributed Systems Challenges You Will Need to Address

You are expected to design, implement, and explain how your system handles:

* Decentralized membership without a central authority
* Failure and recovery during message transmission
* Partial delivery and duplicate handling
* Evidence-based message delivery
* Best-effort recovery without global coordination
* Convergence after crashes and partitions
* Making behavior observable and explainable

---

## Team Size and Time Expectations

This project is designed for **6 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
