# Deliverable Milestone Plan  
**Distributed Video Upload and Adaptive Streaming Platform**

Each week requires:
- a **Running Artifact** (deployable and demonstrable), and
- a **Scope / Design Artifact** (documentation describing what actually runs).

You will be graded weekly against the milestones.

---

## Deliverables

| Week | Focus | Running Artifact | Scope / Design Artifact |
|------|------|------------------|-------------------------|
| **1** | Project Bootstrap & Scope Definition | • Repo, build, and local deployment working<br>• Services start and respond to `/health`<br>• S3/MinIO connectivity verified | • *Initial Scope Document* (1–2 pages)<br>• Components aligned with project brief<br>• Explicit assumptions and non-goals |
| **2** | Upload API & Architecture | • `/upload` endpoint accepts files (stubbed metadata allowed)<br>• Deterministic object naming scheme defined<br>• Objects written to S3/MinIO | • API contract frozen<br>• Object naming and metadata model frozen<br>• **Diagram:** Initial Architecture Diagram |
| **3** | Upload Idempotency & Metadata | • Re-upload of same video does not duplicate objects<br>• Video status endpoint (`/videos/{id}`) working<br>• Metadata persisted durably | • Idempotency rules documented<br>• Upload and re-upload semantics clarified |
| **4** | Orchestration Baseline (Single-Node) | • Orchestrator creates processing jobs after upload<br>• Job state machine implemented (uploaded → processing → ready/failed)<br>• Manual trigger of processing supported | • Orchestration responsibilities and guarantees<br>• Single-node correctness assumptions |
| **5** | Video Processing & Segmentation | • Worker transcodes video into multiple bitrates<br>• Segments and manifests written to S3/MinIO<br>• End-to-end upload → processed output works | • Processing pipeline documented<br>• Retry and partial-output assumptions |
| **6** | Orchestration with Retries & Failure | • Failed processing tasks are retried<br>• Partial results handled safely<br>• Orchestrator detects stuck jobs | • Retry semantics and failure handling<br>• **Diagram:** Distributed Coordination Diagram |
| **7** | Segment Fetching Service | • Segment server serves manifests and segments<br>• Third-party player can begin playback (no caching yet required)<br>• Basic request logging implemented | • Segment service responsibilities<br>• Playback routing assumptions |
| **8** | Sticky Routing & Node Failure | • Playback sessions routed to a single segment node<br>• Segment node failure causes reassignment<br>• Playback continues after reassignment | • Sticky routing semantics<br>• Failure fallback behavior and non-guarantees |
| **9** | Caching & Read-Ahead | • Node-local cache implemented<br>• Cache-on-read working<br>• Required read-ahead (`k+1`) implemented asynchronously<br>• Cache hit/miss and prefetch logs visible | • Caching and read-ahead semantics<br>• Tradeoffs and limitations documented |
| **10** | Adaptive Bitrate Proof | • Third-party ABR player integrated<br>• Bitrate switches occur under network changes<br>• Server logs prove variant switching | • ABR behavior explanation<br>• Mapping between player behavior and logs |
| **11** | Failure Injection & Observability | • Worker killed mid-job and recovers<br>• Segment node killed mid-playback and recovers<br>• Logs explain observed behavior | • Failure scenarios documented<br>• Observability and debugging strategy |
| **12** | Final System Hardening | • Clean startup/shutdown scripts<br>• Restart behavior verified<br>• End-to-end demo reproducible from scratch | • Final design document<br>• **Diagram:** Final As-Built System Diagram |
| **13** | Final Demo & Submission | • Live demo: upload → process → stream<br>• ABR switching shown<br>• Failure handling demonstrated | • Final scope/design document<br>• Clear statement of guarantees and limitations |

---

## Notes

- **Every milestone must produce a running system**, not just code.
- **Documentation must match observed behavior**, not intended design.
- **Correctness and clarity** matter more than performance optimizations.
- Logs are a first-class artifact and part of the grading criteria.
