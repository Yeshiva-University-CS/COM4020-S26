# Deliverable Milestone Plan  
**Decentralized Node Plane (Control Plane Replacement for a Distributed Mesh VPN)**

Each week requires:
- a **Running Artifact**, and
- a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Deliverables

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Bootstrap & Integration Scope | • Node Plane agent runs on one node<br>• `/status` endpoint responds | • Initial scope document<br>• Explicit statement: *This project replaces the Mesh VPN control plane* |
| **2** | Control Plane Interface & Architecture | • Control Plane Interface defined **in conjunction with the Mesh VPN team**<br>• Stub implementation usable by Mesh VPN | • Interface specification frozen<br>• **Diagram:** Initial Architecture Diagram |
| **3** | Interface Compliance & Identity | • Node Plane implements interface<br>• Mesh VPN calls interface methods | • Interface semantics<br>• Identity assumptions |
| **4** | Cluster Bootstrap & Membership | • Multi-node cluster boots via seed list<br>• Mesh VPN receives membership updates | • Membership states defined (`alive`, `suspect`, `dead`) |
| **5** | Ledger Integration | • Ledger integrated and recording control-plane events<br>• Membership derived via ledger replay | • Ledger semantics documented |
| **6** | Reconciliation & Convergence | • Ledger exchange implemented<br>• Membership converges when all nodes are healthy | • **Diagram:** Membership, Ledger & Failure Model |
| **7** | Failure Detection | • Heartbeats/probes implemented<br>• Failure suspicion surfaced via interface | • Failure-detection semantics |
| **8** | Failure Injection | • Node killed mid-run<br>• Suspicion propagated and recorded in ledger | • Degraded-mode behavior |
| **9** | Recovery & Rejoin | • Restarted node rejoins cluster<br>• Ledger replay restores state | • Restart and rejoin guarantees |
| **10** | Partition Analysis | • Simulated network partition (where feasible)<br>• Observed divergence and reconvergence | • Split-brain analysis |
| **11** | Control Plane Replacement Validation | • Mesh VPN runs using **only** the Node Plane control plane<br>• Stub implementation disabled | • Control plane swap procedure |
| **12** | Observability & Final Design | • Logs + ledger explain system behavior<br>• Clean startup/shutdown scripts | • Final design document<br>• **Diagram:** Final As-Built Diagram |
| **13** | Final Demo & Submission | • Live multi-node demo<br>• Failure + recovery demonstrated through Mesh VPN | • Final scope document<br>• Clear statement of guarantees and limitations |

---

## Notes

- Ledger semantics matter more than internal mechanics.
- Documentation must reflect actual observed behavior.
