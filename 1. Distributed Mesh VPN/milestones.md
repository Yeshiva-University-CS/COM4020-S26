# Deliverable Milestone Plan  
**Distributed Mesh VPN**

Each week requires:
- a **Running Artifact** (deployable and demonstrable), and
- a **Scope / Design Artifact** (documentation describing what actually runs).

You will be graded weekly against these milestones.

---

## Milestones

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Control plane service starts and responds to `/health`<br>• Node daemon skeleton starts and runs | • *Initial Scope Document* (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2** | Architecture & APIs | • Node registers with control plane (stubbed allowed)<br>• Control plane returns static peer list | • API contracts frozen<br>• **Diagram:** Initial Architecture Diagram |
| **3** | Node Identity & Metadata | • Each node publishes a public key and human-readable node name (alias)<br>• Control plane distributes mesh IPs, keys, and node metadata | • Identity model (mesh IP vs node name)<br>• Metadata semantics and non-guarantees |
| **4** | Single-Node WireGuard Baseline | • Node daemon configures local WireGuard interface<br>• Manual peer configuration verified | • Baseline networking assumptions |
| **5** | Dynamic Peer Configuration | • Node dynamically adds/removes WireGuard peers based on control-plane state<br>• Configuration is idempotent | • Reconfiguration semantics |
| **6** | Liveness & Coordination Model | • Heartbeats implemented<br>• Control plane tracks node liveness | • Failure assumptions<br>• **Diagram:** Distributed Coordination Diagram |
| **7** | Multi-Node Mesh Formation | • Three or more nodes form a full mesh automatically<br>• Secure peer-to-peer communication verified | • Scope update: implemented vs deferred behavior |
| **8** | Failure Handling | • Node failure detected via liveness<br>• Remaining nodes remove or disable tunnels automatically | • Failure behavior and degraded modes |
| **9** | Restart & Rejoin Semantics | • Failed node restarts and rejoins the mesh correctly<br>• Configuration converges without manual intervention | • Convergence guarantees |
| **10** | Messaging Demo Integration | • Messaging app runs on each node<br>• Left pane lists peers by node name and mesh IP<br>• Direct peer-to-peer messaging over mesh IPs | • Messaging demo architecture<br>• Explanation of why traffic flows over the VPN |
| **11** | Messaging Failure & Deployment Hardening | • Messaging succeeds and fails appropriately as nodes join/leave<br>• Clean startup and shutdown scripts | • Operational instructions<br>• Demo scripts |
| **12** | Observability & Final Design | • Logs clearly explain membership changes, failures, and recovery<br>• Messaging behavior correlates with VPN state | • Final design document<br>• **Diagram:** Final As-Built Diagram |
| **13** | Final Demo & Submission | • Live demo showing:<br> – Node join<br> – Messaging success<br> – Node failure<br> – Messaging failure and recovery | • Final scope/design document<br>• Clear statement of guarantees and limitations |

---

## Notes

- **Running systems matter:** Every claim must be observable.
- **Correctness over cleverness:** Simplicity and clarity are valued.
- **Documentation must match reality:** Design documents must describe what actually runs.

