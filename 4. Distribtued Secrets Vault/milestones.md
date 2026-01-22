## Deliverable Milestone Plan  
**Distributed Secrets Vault**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Secrets service starts and responds to `/health`<br>• Single-node deployment reproducible | • Initial scope document (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2** | Architecture & API Contract | • `/store` and `/retrieve` endpoints callable (stubbed allowed)<br>• Authentication enforced | • API contract frozen<br>• Initial architecture diagram |
| **3** | Secret Identity & Lookup Semantics | • Secrets can be stored with a stable identifier<br>• Same identifier resolves consistently across restarts | • Secret identity and lookup assumptions |
| **4** | Single-Node Vault Semantics | • Secret writes persisted durably<br>• Versioned updates supported | • Single-node correctness guarantees |
| **5** | Encryption-at-Rest Record Format | • Secret values are stored encrypted at rest<br>• Stored records include wrapped data keys and required metadata | • Encryption and storage record definition |
| **6** | Idempotency & Retry Semantics | • Safe retries demonstrated for store and retrieve | • Idempotency rules and limitations |
| **7** | Multi-Node Architecture & Replication Path | • Replication between two vault nodes exercised<br>• Stored secrets become visible on peers | • Replication model and failure assumptions |
| **8** | Multi-Node Consistency | • Consistent behavior with all nodes healthy<br>• Reads resolve using authoritative replicated state | • Updated scope reflecting implemented behavior |
| **9** | Failure Handling | • One vault node killed during operation<br>• System remains usable | • Failure behavior and degraded modes |
| **10** | Concurrency, History, and Isolation Safety | • Concurrent store and update tested<br>• Historical secret values retained correctly<br>• Validity timestamps assigned correctly | • Concurrency, history, and isolation guarantees |
| **11** | Multi-Secret Resolution | • `.env` encryption and expansion creates and resolves secrets correctly<br>• All-or-nothing failure demonstrated | • `.env` transformation and secret creation semantics |
| **12** | History Retrieval | • Secret history endpoint returns ordered versions with validity ranges | • History query semantics and authorization rules |
| **13** | Observability Under Change | • Logs show store, retrieve, history access, transformation, failure, and recovery | • Observed behavior with supporting logs |
| **14** | Final Demo & Submission | • Live multi-node demo showing concurrency, failure, recovery, history access, and `.env` encryption/expansion | • Final scope document<br>• Clear statement of guarantees and limitations |

---

## Notes

* Running systems matter: every claim must be observable.
* Documentation must describe what actually runs, not intended design.
* Correctness and clarity are the main goal.
