# Distributed Secrets Vault

## Project Overview

In this project, you will design and implement a distributed secrets vault that stores opaque secret values durably and serves them to authorized callers across multiple nodes.

The system exposes a simple HTTP API for storing and retrieving secrets. Storing a secret is a state-creating operation whose correctness depends on durable, replicated storage. Retrieving a secret must enforce access boundaries and isolation rules derived from the caller’s authenticated identity.

Multiple nodes cooperate to store secret state, serve requests concurrently, and remain correct under failure and restart. The system may use centralized coordination internally, but must behave correctly as a distributed system.

Secrets are stored encrypted at rest using envelope encryption. Secret values are treated as opaque bytes by the system outside of encryption and history management. The project does not include key rotation or key lifecycle management beyond using a fixed master key configuration.

The emphasis of the project is on distributed systems behavior: clear authority over secret existence, durable recording, replication, consistent reads under partial failure, concurrency safety, and explainable recovery. System behavior must be observable through logs and state inspection.

You are not expected to implement production hardening, external secret manager integrations, or key rotation. The focus is on correctness and explainable behavior.

---
### 1. [Components](components.md)
### 2. [Deliverables & Milestones](milestones.md)
---

## Resulting System Behavior

When the system is running correctly:

* Clients can store secrets through a simple HTTP API
* Clients can retrieve secrets through a simple HTTP API when permitted
* Clients can view the version history of a secret, including when each value was valid
* Clients can submit an `.env` file and:
  * replace `secret(NAME)` references with the caller’s current secret values
  * replace `enc(NAME)` references by storing the referenced value as a secret and returning `secret(NAME)`
* Secrets are considered valid only after they are durably recorded
* Secret state is replicated across vault nodes
* Secret values are never stored in plaintext at rest
* Each stored secret is encrypted with a per-secret data key that is wrapped by a configured master key
* Concurrent requests do not create conflicting or duplicate secret records
* Retrieval enforces access boundaries and isolation rules
* Requests for secrets not valid for the caller return *not found*
* Node failures do not corrupt secret state
* Restarted nodes recover state and resume correct operation
* If the configured master key is unavailable or incorrect, retrieval fails deterministically and secrets are not returned
* System behavior under concurrency, failure, and restart is observable

---

## Distributed Systems Challenges You Will Need to Address

You are expected to design, implement, and explain how your system handles:

* Agreement on secret existence across nodes
* Durable recording of secret writes
* Versioned updates to existing secrets
* Tracking and serving historical secret versions
* Defining validity intervals for secret values
* Replication of authoritative state
* Correct handling of retries
* Isolation between different callers or tenants
* Coordinated creation and retrieval of multiple secrets in a single operation
* Deterministic failure when referenced secrets cannot be resolved
* Deterministic transformation of `.env` files
* Node crashes during read or write operations
* Restart and recovery without manual intervention
* Convergence after partial failure
* Making behavior observable and explainable
* Encrypting stored state without changing authority boundaries or introducing non-authoritative caches

---

## Team Size and Time Expectations

This project is designed for **3 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
