## Deliverable Milestone Plan  
**Distributed Token Vault**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Tokenization service starts and responds to `/health`<br>• Single-node deployment reproducible | • Initial scope document (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2** | Architecture & API Contract | • `/tokenize` and `/detokenize` endpoints callable (stubbed allowed)<br>• Authentication enforced | • API contract frozen<br>• Initial architecture diagram |
| **3** | Deterministic Token Generation | • Deterministic token computation implemented<br>• Same input produces same token across restarts | • Token semantics and assumptions |
| **4** | Single-Node Vault Semantics | • Token mappings persisted durably<br>• Insert-once behavior enforced | • Single-node correctness guarantees |
| **5** | Idempotency & Retry Semantics | • Safe retries demonstrated<br>• Duplicate issuance prevented | • Idempotency rules and limitations |
| **6** | Epoch State & Multi-Node Architecture | • Active epoch stored authoritatively<br>• Epoch state visible across nodes<br>• Replication path exercised | • Epoch semantics and coordination rules<br>• Replication model and failure assumptions |
| **7** | Multi-Node Consistency | • Consistent behavior with all nodes healthy<br>• Tokenization uses authoritative epoch | • Updated scope reflecting implemented behavior |
| **8** | Failure Handling | • One vault node killed during operation<br>• System remains usable | • Failure behavior and degraded modes |
| **9** | Concurrency, Isolation & Epoch Safety | • Concurrent tokenization tested<br>• No mixed-epoch issuance within a request | • Concurrency, isolation, and epoch safety guarantees |
| **10** | Restart & Recovery | • Failed vault node restarts and rejoins<br>• Token and epoch state recovered | • Restart and recovery behavior explained |
| **11** | Observability Under Change | • Logs show token issuance, epoch changes, failure, and recovery | • Observed behavior with supporting logs |
| **12** | Final Design & Observability | • Logs clearly explain system behavior | • Final design document<br>• Final architecture diagram |
| **13** | Final Demo & Submission | • Live multi-node demo showing concurrency, failure, recovery, and epoch change | • Final scope document<br>• Clear statement of guarantees and limitations |

---

## Notes

* Running systems matter: every claim must be observable.
* Documentation must describe what actually runs, not intended design.
* Correctness and clarity are the main goal.
