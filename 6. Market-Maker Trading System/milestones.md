## Deliverable Milestone Plan

**Market-Maker Trading System**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week   | Focus                                   | Running Artifact                                                                                              | Scope / Design Artifact                                                     |
| ------ | --------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **1**  | Project Bootstrap & Exchange Skeleton   | • Exchange service starts and responds to `/health`<br>• Static reference prices visible per symbol           | • Initial scope document (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2**  | External Orders & Exchange API          | • External order publisher submits orders via API<br>• Orders accepted and validated by exchange              | • External order model and exchange API semantics                            |
| **3**  | Fill Generation & Matching              | • External orders match against quotes<br>• Fills generated deterministically                                 | • Matching and fill generation rules                                        |
| **4**  | Trading State Service                   | • Fills recorded durably<br>• Positions derived correctly from fills                                          | • Position semantics and durability guarantees                               |
| **5**  | Deterministic Market Maker              | • Single market-maker node publishes deterministic quotes<br>• Quote TTL and refresh observable               | • Quote computation and lifecycle                                            |
| **6**  | Quote Replacement & Idempotency         | • Quotes replaced only after committed position updates<br>• Duplicate quote effects prevented                | • Idempotency rules and ordering assumptions                                 |
| **7**  | Quote Expiration (TTL)                  | • Quotes expire after 30 seconds and cannot be filled<br>• Expired quotes removed automatically               | • Expiration semantics and refresh behavior                                  |
| **8**  | Global Exposure Reservation             | • Exposure reservation service enforces ±100 limit based on active quotes                                      | • Exposure model and reservation rules                                       |
| **9**  | Exposure-Constrained Quoting            | • Quotes reduced or omitted when exposure is insufficient<br>• No limit violations observed                   | • Exposure enforcement and worst-case evaluation                              |
| **10** | Concurrent Market Makers & Contention   | • Multiple market-maker nodes quoting concurrently<br>• Exposure remains correct under contention              | • Concurrency model and coordination assumptions                              |
| **11** | Failure During Active Exposure          | • Market maker or reservation service killed mid-quote<br>• Exposure eventually released or corrected         | • Failure handling and recovery behavior                                     |
| **12** | Restart & State Convergence             | • Full system restart<br>• Positions, exposure, and quotes converge correctly                                  | • Recovery and convergence guarantees                                        |
| **13** | Observability, GUI & Final Demo         | • GUI displays live positions and activity<br>• End-to-end demo with orders, fills, exposure, failure          | • Final design document<br>• Final scope and guarantees                       |

---

## Notes

* Every component must be exercised by a running system.
* Exposure usage, quote state, and position updates must be observable.
* Documentation must reflect implemented behavior, not intent.
