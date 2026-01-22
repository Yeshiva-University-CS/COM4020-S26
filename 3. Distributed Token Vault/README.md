# Distributed Token Vault

## Project Overview

In this project, you will design and implement a distributed token vault that replaces sensitive values with deterministic tokens and stores the mappings across multiple nodes.

The system exposes a simple HTTP API for tokenization and detokenization. Tokenization deterministically computes tokens for configured fields using the currently active epoch. Detokenization returns original values only when permitted by isolation rules.

Issuing a token is a state-creating operation whose correctness depends on durable, replicated storage. The system maintains a single active epoch per owner, which determines how new tokens are computed. Epoch changes are coordinated as distributed state and affect subsequent tokenization operations.

Multiple nodes cooperate to store token mappings and epoch state, serve requests concurrently, and remain correct under failure and restart. The system may use centralized coordination internally, but must behave correctly as a distributed system.

The emphasis of the project is on distributed systems behavior: agreement on token existence and epoch state, insert-once semantics, isolation, replication, failure handling, and recovery. System behavior must be observable through logs and state inspection.

You are not expected to implement advanced security frameworks, encryption key management, or production hardening. The focus is on correctness and explainable behavior.

---
### 1. [Components](components.md)
### 2. [Deliverables & Milestones](milestones.md)
---

## Resulting System Behavior

When the system is running correctly:

* Clients can submit values for tokenization through a simple HTTP API
* The same input produces the same token for the same owner and epoch
* The system maintains a single active epoch per owner
* Tokens are considered valid only after their mappings are durably recorded
* Token mappings and epoch state are replicated across vault nodes
* Concurrent requests do not produce conflicting or duplicate mappings
* Detokenization returns original values only when permitted
* Tokens not valid for the caller return *not found*
* Node failures do not corrupt token mappings or epoch state
* Restarted nodes recover state and resume correct operation
* System behavior under concurrency, failure, and epoch change is observable

---

## Distributed Systems Challenges You Will Need to Address

You are expected to design, implement, and explain how your system handles:

* Agreement on token existence across nodes
* Agreement on the active epoch per owner
* Durable recording of token issuance and epoch changes
* Insert-once semantics under concurrency
* Replication of authoritative state
* Correct handling of retries
* Isolation between different owners
* Node crashes during read or write operations
* Restart and recovery without manual intervention
* Convergence after partial failure
* Making behavior observable and explainable

---

## Team Size and Time Expectations

This project is designed for **3 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
