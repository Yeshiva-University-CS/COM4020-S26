## Deliverable Milestone Plan

**Secure Group Chat Room**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

Diagrams are required where specified and must reflect implemented behavior.

---

## Milestones

| Week | Focus | Running Artifact | Scope / Design Artifact |
|-----:|-------|------------------|--------------------------|
| **1** | System Framing & Interfaces | • Node process builds and starts locally<br>• Stub network communication (no messaging semantics yet)<br>• Logs show node lifecycle | • Initial scope document<br>• Subsystem boundaries and responsibilities<br>• **High-level system diagram** |
| **2** | Message Transport Skeleton | • Nodes exchange basic messages over private network<br>• Message framing and dispatch visible<br>• No chat semantics yet | • Message flow description<br>• **Inter-node communication diagram** |
| **3** | Identity & Membership Observation | • Stable node identity visible<br>• Membership view populated<br>• Joins observable | • Identity and membership model<br>• **Membership state diagram** |
| **4** | Failure Detection Without Corruption | • Node crash detected by peers<br>• Membership updates visible<br>• Messaging does not corrupt state | • Failure semantics and evidence<br>• **Failure scenario diagram** |
| **5** | Restart & Incarnation Handling | • Restarted node re-enters correctly<br>• Old messages rejected<br>• Membership stabilizes | • Restart and incarnation rules<br>• **Restart sequence diagram** |
| **6** | Group Messaging (Partial Delivery) | • Group messages delivered peer-to-peer<br>• Partial delivery under churn demonstrated<br>• Duplicates handled | • Group message semantics<br>• **Group message flow diagram** |
| **7** | Private Messaging Semantics | • Point-to-point DMs delivered<br>• Delivery state visible | • DM delivery and timeout semantics<br>• **DM interaction diagram** |
| **8** | Ambiguity Under Failure | • DM behavior under crash/restart<br>• Timeouts observable<br>• No false delivery claims | • Evidence-based delivery rules<br>• **Failure timing diagram** |
| **9** | Group Message Recovery | • Restarted node requests recent group messages<br>• Peers return retained messages<br>• Gaps visible | • Group recovery behavior and bounds<br>• **Recovery interaction diagram** |
| **10** | Partitioned Operation | • Network partition simulated<br>• Messaging continues independently<br>• Membership diverges | • Partition assumptions<br>• **Partition topology diagram** |
| **11** | Healing & Convergence | • Partition healed<br>• Membership converges<br>• Messaging resumes safely | • Convergence rules<br>• **Post-heal convergence diagram** |
| **12** | Compound Failures & Explainability | • Crash during partition or recovery<br>• System remains explainable<br>• Logs show causal chains | • Cross-scenario analysis<br>• **Combined failure + observability diagram** |
| **13** | Final Demo & Submission | • Live demo exercising joins, crashes, restarts, partitions, recovery<br>• Group chat and DMs exercised together | • Final design document<br>• Final scope and guarantees<br>• **Final system diagram set** |

---

## Notes

* All three subsystems must be active throughout the project.
* Students may migrate between subsystems as needed.
* Diagrams must reflect **implemented behavior**, not intent.
* Partial delivery, duplicates, and gaps are acceptable when explained.
* Grading prioritizes correctness, recovery, and explanation over completeness.
