## Deliverable Milestone Plan

**Market-Maker Trading System**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week   | Focus                                 | Running Artifact                                                                                        | Scope / Design Artifact                                                      |
| ------ | ------------------------------------- | ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **1**  | Project Bootstrap & Exchange Skeleton | • Exchange simulator starts and responds to `/health`<br>• Deterministic reference prices visible       | • Initial scope document (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2**  | Exchange Order & Fill Flow            | • Order submission and cancellation endpoints callable<br>• External orders generated deterministically | • Exchange behavior and matching rules                                       |
| **3**  | Trading State Service                 | • Fills recorded durably<br>• Positions derived correctly from fills                                    | • Position semantics and durability guarantees                               |
| **4**  | Deterministic Market Maker            | • Single-node market maker publishing deterministic quotes<br>• Quote refresh behavior observable       | • Quote computation and lifecycle                                            |
| **5**  | Quote Supersession & Idempotency      | • Cancel/replace enforced<br>• Duplicate quote effects prevented                                        | • Idempotency rules and failure assumptions                                  |
| **6**  | Multi-Node Market Makers              | • Multiple market-maker nodes running concurrently<br>• No conflicting quote actions                    | • Multi-node interaction model                                               |
| **7**  | Symbol Authority                      | • Exclusive symbol authority enforced<br>• Authority state inspectable                                  | • Authority semantics and ownership rules                                    |
| **8**  | Authority Failure & Takeover          | • Authorized node killed mid-operation<br>• Authority transfers without overlap                         | • Failure handling and takeover behavior                                     |
| **9**  | Membership Tracking                   | • Node presence registered and expired<br>• Active membership observable                                | • Membership model and assumptions                                           |
| **10** | Symbol Redistribution                 | • Symbols redistributed on node join or leave<br>• Quoting resumes under new assignments                | • Redistribution rules and constraints                                       |
| **11** | Restart & Convergence                 | • Full cluster restart<br>• Authority, positions, and quotes recover correctly                          | • Recovery and convergence behavior                                          |
| **12** | Observability & GUI                   | • GUI displays live positions, authority, and activity<br>• Logs explain state transitions              | • Observability surfaces and guarantees                                      |
| **13** | Final Demo & Submission               | • Live demo showing failure, redistribution, and recovery                                               | • Final design document<br>• Final scope and guarantees                      |

---

## Notes

* Every component must be exercised by a running system.
* Authority, state, and membership behavior must be observable.
* Documentation must reflect implemented behavior, not intent.
