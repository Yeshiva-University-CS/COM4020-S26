# Deliverable Milestone Plan
**Distributed Token Vault for PHI Tokenization**

Each week requires:
- a **Running Artifact** (deployable and demonstrable), and
- a **Scope / Design Artifact** (documentation describing what actually runs).
- You will be graded weekly against the milestones.

---

## Deliverables

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Tokenization API service starts and responds to `/health`<br>• Single-node deployment reproducible from scratch | • *Initial Scope Document* (1–2 pages)<br>• Components aligned with project brief<br>• Explicit assumptions and non-goals |
| **2** | Tokenization API Service & Architecture | • `/tokenize` and `/detokenize` endpoints live (stubbed allowed)<br>• Configuration file loaded and validated at startup | • API contract frozen<br>• Data model frozen<br>• **Diagram:** Initial Architecture Diagram |
| **3** | Token Semantics & Epochs | • Deterministic token generator wired into `/tokenize`<br>• Tokens include an epoch identifier (self-describing)<br>• Determinism verified across restarts | • Token semantics documented<br>• Owner- and epoch-scoped behavior<br>• Collision and epoch assumptions |
| **4** | Token Vault (Single-Node Baseline) | • Persistent token vault implemented<br>• End-to-end tokenize/detokenize works<br>• Lookups scoped by `(owner_id, epoch, token)`<br>• Detokenization returns *not found* only<br>• Local event log implemented | • Single-node correctness guarantees<br>• Explicit statement of non-guarantees |
| **5** | Idempotency & Retry Semantics | • Safe retry behavior demonstrated<br>• Duplicate token issuance prevented under retries | • Idempotency rules<br>• Retry assumptions and limitations |
| **6** | Distributed Token Vault – Replication Model | • Three-node deployment running (replication may be stubbed)<br>• Active epoch stored/replicated or read consistently across nodes<br>• Inter-node communication visible in logs | • Replication model chosen and frozen (leader-based or quorum-based)<br>• Failure assumptions<br>• **Diagram:** Distributed Coordination Diagram |
| **7** | Distributed Token Vault – Replication (Happy Path) | • Real replication implemented<br>• Consistent behavior with all nodes healthy | • Scope update: implemented vs deferred behavior |
| **8** | Failure Handling & Recovery | • One node killed during operation<br>• System remains available<br>• Cross-owner detokenization returns *not found* | • Failure-handling behavior documented<br>• Degraded-mode behavior and non-guarantees |
| **9** | Concurrency, Isolation & Epoch Safety | • Concurrent tokenization test harness<br>• Same plaintext tokenized by different owners and/or epochs produces different tokens<br>• No duplicate issuance under concurrency | • Concurrency guarantees<br>• Isolation and epoch-consistency rationale |
| **10** | Deployment Hardening & Rotation Procedure | • Clean startup and shutdown scripts<br>• Configuration validation and error handling<br>• Restart behavior verified | • System bring-up, shutdown, and restart instructions (reproducible demo steps)<br>• Documented token rotation procedure (epoch rotation + migration window) |
| **11** | DuckDB Integration | • DuckDB batch detokenization working<br>• Demonstrable join between tokenized data and recovered values (including a rotation/migration scenario) | • DuckDB integration instructions<br>• Concrete usage examples (commands/queries with expected output)<br>• Known limitations and assumptions |
| **12** | Observability & Final Design | • Event logs explain system behavior<br>• Failure and rotation scenarios replayable | • Final design document matching reality<br>• **Diagram:** Final As-Built System Diagram |
| **13** | Final Demo & Submission | • Live multi-node demo<br>• Failure tolerance demonstrated<br>• Token rotation (via API) demonstrated<br>• DuckDB integration demonstrated | • Final scope/design document<br>• Clear statement of guarantees and limitations |

---

## Notes

- **Testing is required:** Concurrency, failure, and rotation behavior must be demonstrated with tests or reproducible demos.
- **Documentation must match reality:** Claims must be supported by observed behavior.
- **Correctness over performance:** Determinism and isolation matter more than throughput.
