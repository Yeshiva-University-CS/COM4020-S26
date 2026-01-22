## Deliverable Milestone Plan  
**Video Upload and Streaming System**

Each week requires:

* a **Running Artifact** (deployable and demonstrable), and
* a **Scope / Design Artifact** describing what actually runs.

You will be graded weekly against these milestones.

---

## Milestones

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Upload service starts and responds to `/health`<br>• MinIO or S3 connectivity verified | • Initial scope document (1–2 pages)<br>• Explicit assumptions and non-goals |
| **2** | Architecture & API Contract | • Upload endpoint accepts files (stubbed allowed)<br>• Video ID returned | • API contract frozen<br>• Initial architecture diagram |
| **3** | Durable Upload Semantics | • Uploaded videos written to object storage<br>• Upload acknowledged only after durability | • Upload correctness guarantees |
| **4** | Video Lifecycle State | • Video status endpoint implemented<br>• Lifecycle states observable | • Video lifecycle and processing model |
| **5** | Segmentation & Transcoding (Single-Node) | • Processing service segments and transcodes videos<br>• Outputs written to object storage | • Single-node processing semantics |
| **6** | Multi-Node Processing Architecture | • Processing and streaming services run on separate nodes<br>• Inter-node coordination exercised | • Updated architecture diagram<br>• Failure assumptions |
| **7** | Streaming Baseline | • Streaming service serves processed segments<br>• Playback works for completed videos | • Streaming responsibilities and guarantees |
| **8** | Failure Handling | • Processing or streaming node killed mid-operation<br>• System remains usable | • Failure behavior and degraded modes |
| **9** | Retry & Idempotency | • Safe retry of processing demonstrated<br>• No duplicate segmentation or transcoding | • Idempotency rules |
| **10** | Adaptive Bitrate Streaming | • Multiple representations served<br>• Bitrate switches observed during playback<br>• Playback remains correct whether segments are served by the same or different nodes | • Adaptive streaming semantics |
| **11** | Concurrency & Observability | • Concurrent uploads and streams supported<br>• Logs explain adaptive behavior | • Observed concurrency behavior |
| **12** | Final Design & Observability | • Logs clearly explain upload, processing, and streaming | • Final design document<br>• Final architecture diagram |
| **13** | Final Demo & Submission | • Live multi-node demo showing upload, processing, adaptive streaming, failure, and recovery | • Final scope document<br>• Clear statement of guarantees and limitations |

---

## Notes

* Running systems matter: every claim must be observable.
* Documentation must describe what actually runs, not intended design.
* Correctness and clarity are the main goal.
