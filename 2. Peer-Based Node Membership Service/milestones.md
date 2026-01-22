## Deliverable Milestone Plan  
**Peer-Based Node Membership Service**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Per-node membership service starts and runs on a single machine<br>• `/status` or equivalent endpoint responds | • Initial scope document (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2** | Architecture & Interface | • Control interface defined and callable locally<br>• Static or stubbed membership returned | • Interface contract frozen<br>• Initial architecture diagram |
| **3** | Node Identity & Local State | • Stable node identity established<br>• Local membership state represented explicitly | • Identity and state model documented |
| **4** | Multi-Node Startup | • Two or more nodes start and discover each other<br>• Membership visible on all nodes | • Bootstrap and discovery assumptions |
| **5** | Voluntary Join & Leave | • Nodes can join and leave on demand<br>• Membership updates propagate correctly | • Join and leave semantics |
| **6** | Liveness Detection | • Periodic liveness signals implemented<br>• Unresponsive nodes detected | • Failure-detection rules and timeouts |
| **7** | Multi-Node Convergence | • Three or more nodes converge on a shared membership view | • Updated scope reflecting implemented behavior |
| **8** | Failure Handling | • Node failure detected<br>• Remaining nodes update membership automatically | • Failure behavior and known limitations |
| **9** | Restart & Rejoin | • Failed or voluntarily departed node rejoins correctly<br>• Membership converges without manual cleanup | • Restart and rejoin behavior explained |
| **10** | Interface Integration | • External system uses membership interface | • Interface usage and guarantees |
| **11** | Behavior Under Change | • Membership updates observed during join, leave, failure, and recovery | • Observed behavior with supporting logs |
| **12** | Observability & Final Design | • Logs clearly show membership changes and failure handling | • Final design document<br>• Final architecture diagram |
| **13** | Final Demo & Submission | • Live multi-node demo showing join, leave, failure, and recovery | • Final scope document<br>• Clear statement of guarantees and limitations |

---

## Notes

* Running systems matter: every claim must be observable.
* Documentation must describe what actually runs, not intended design.
* Correctness and clarity are the main goal.
