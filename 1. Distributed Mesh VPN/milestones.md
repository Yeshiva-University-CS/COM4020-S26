## Deliverable Milestone Plan

Distributed Mesh VPN

Each week requires:

* a running, deployable system artifact, and
* a scope or design artifact describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week | Focus                                | Running Artifact                                                                                                                   | Scope / Design Artifact                                                  |
| ---- | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| 1    | Project Bootstrap & Scope Definition | Coordination service starts and responds to a health endpoint<br>Per-node background service starts and runs                       | Initial scope document (1–2 pages)<br>Explicit assumptions and non-goals |
| 2    | Registration & Basic Coordination    | Per-node service registers with coordination service<br>Coordination service records machines and returns a static or stubbed list | API contracts frozen<br>Initial architecture diagram                     |
| 3    | Machine Identity & Metadata          | Machines assigned stable mesh IPs<br>Public keys and basic peer metadata exchanged                                                 | Identity and mesh IP assumptions documented                              |
| 4    | Local VPN Configuration Baseline     | Per-node service configures a local WireGuard interface<br>Manual or static peer connectivity verified                             | Networking and environment assumptions                                   |
| 5    | Dynamic Connection Updates           | Nodes dynamically add and remove WireGuard peers based on coordination state<br>Repeated updates do not break existing connections | Description of how configuration changes are applied safely              |
| 6    | Liveness Detection                   | Nodes report liveness periodically<br>Coordination service detects unresponsive machines                                           | Heartbeat mechanism and timeout assumptions                              |
| 7    | Multi-Node Mesh Formation            | Three or more machines automatically form a full mesh<br>Encrypted peer-to-peer connectivity verified                              | Updated scope reflecting implemented behavior                            |
| 8    | Failure Handling                     | Machine failure detected<br>Remaining machines update connections accordingly                                                      | Failure behavior and known limitations                                   |
| 9    | Restart & Rejoin Behavior            | Failed machine restarts and rejoins the mesh<br>Connections are restored without manual cleanup                                    | Restart and rejoin behavior explained                                    |
| 10   | Messaging Demonstration              | Messaging application runs on each machine<br>Messages sent directly over mesh IPs                                                 | Messaging application architecture and traffic flow explanation          |
| 11   | Application Behavior Under Failure   | Messaging succeeds or fails appropriately as machines join, leave, or fail                                                         | Observed behavior and supporting logs                                    |
| 12   | Observability & Final Design         | Logs clearly show membership changes, failures, and recovery                                                                       | Final design document<br>Final architecture diagram                      |
| 13   | Final Demo & Submission              | Live multi-node demo showing join, failure, recovery, and messaging behavior                                                       | Final scope document<br>Clear statement of guarantees and limitations    |

---

## Notes

* Running systems matter: every claim must be observable.
* Documentation must describe what actually runs, not intended design.
* Correctness and clarity are the main goal.
