# Project Brief: Distributed Video Upload and Adaptive Streaming Platform

## Overview

In this project, you will design and implement a distributed video ingestion, processing, and adaptive bitrate streaming platform. The system accepts large video uploads, reliably stores them in object storage, processes them into adaptive bitrate formats, and serves segmented video streams under orchestration and failure conditions.

The emphasis of the project is **distributed systems design**, not media engineering. Video transcoding and streaming are used as a concrete workload to explore task orchestration, idempotency, failure handling, caching, routing semantics, and correctness under concurrency.

The system exposes a small set of HTTP APIs for uploading videos, tracking processing state, and retrieving video segments. A third-party adaptive streaming player is used to validate correct adaptive behavior.

---

## Core Semantic Rules

These rules define the required behavior of your system.

1. **Content-addressed video objects**  
   Uploaded videos are identified by a stable `video_id` derived from content and metadata. Re-uploads of the same content must be idempotent and must not trigger duplicate processing.

2. **Separation of concerns**  
   - Object storage provides durability and retrieval  
   - Processing workers perform transcoding and segmentation  
   - Orchestration manages task lifecycle and correctness  
   - Segment services handle delivery, caching, and routing  

3. **At-least-once processing with idempotent stages**  
   Video processing tasks may be retried due to failure. All stages must be safe under retries and partial completion.

4. **Observable state transitions**  
   Each video must expose observable lifecycle states (e.g., uploaded → processing → ready → failed).

5. **Correctness over throughput**  
   The platform must prioritize correctness, determinism, and well-defined behavior under failure over raw performance.

6. **Standards-compatible adaptive streaming**  
   The platform must expose adaptive streaming outputs consumable by at least one third-party adaptive bitrate (ABR) player (e.g., HLS or MPEG-DASH). The team must demonstrate playback using a player they did not author.

7. **Sticky playback routing with failure fallback**  
   Playback sessions use sticky routing: once a client begins streaming, all segment requests for that playback session are served by the same segment-fetching node unless that node becomes unavailable, in which case the session may be reassigned to another node. Reassignment does not require cache preservation.

8. **Node-local caching semantics with required read-ahead**  
   Segment caching is node-local and non-coherent. Cache-on-read is required, and read-ahead caching is required: when serving segment `k` for a given variant, the segment service must asynchronously prefetch segment `k+1` (and may optionally prefetch `k+2`) for the same variant. Read-ahead must never delay serving the requested segment and must not compromise correctness.

9. **Bitrate switching must be provable**  
   The system must produce server-side logs sufficient to prove bitrate/resolution changes during playback, based on which variant segments were requested over time.

---

## What You Will Build

### 1. Video Upload & Metadata API

A stateless HTTP service that accepts video uploads and tracks metadata.

- Endpoints:
  - `/upload` – upload video content
  - `/videos/{id}` – retrieve processing status
  - `/health`
- Uploads may be chunked or multipart
- Metadata stored separately from video data
- Duplicate uploads must not trigger duplicate processing

---

### 2. Object Storage (S3-backed)

The platform stores raw uploads and processed segments in an S3-compatible object store (AWS S3 or MinIO).

- Raw videos and derived segments stored as objects
- Deterministic or content-addressed object naming
- Support for large uploads (multipart or chunked)
- Storage durability and replication are provided by S3

**Out of scope:** implementing a distributed object store.

---

### 3. Video Processing & Segmentation Pipeline

A background processing service that:

- Transcodes videos into multiple resolutions and bitrates
- Segments outputs for adaptive streaming
- Writes outputs and manifests to object storage
- Emits progress and completion events

Processing may be parallelized internally, but all stages must be idempotent and retry-safe.

---

### 4. Orchestration & Workflow Manager

A coordination service that manages video lifecycle.

- Creates processing jobs after upload
- Tracks task state and retries on failure
- Detects stuck or failed jobs
- Ensures each video is processed exactly once logically, even if tasks are retried physically

This service is the primary distributed-systems component of the project.

---

### 5. Segment Fetching, Caching & Adaptive Streaming Interface

A service responsible for serving video segments to clients.

- Exposes a standards-based manifest:
  - HLS (master + variant playlists), or
  - MPEG-DASH (MPD)
- Fetches segments from object storage
- Implements node-local caching
- Serves all segments for a playback session from the same node (except on failure)
- Supports adaptive bitrate switching driven by the client player

#### Read-Ahead Caching Requirements (Mandatory)

- On each segment request for `(video_id, variant, segment_index = k)`, the service must:
  - Serve segment `k` immediately (from cache or origin)
  - Trigger an asynchronous prefetch of segment `k+1` for the same variant
- Read-ahead is best-effort:
  - Prefetch may be skipped if the node is overloaded
  - Prefetch may be wasted if the player switches variants
- Read-ahead must never delay the response for the requested segment.

#### Logging Requirements (Mandatory)

Each segment request and prefetch must generate log entries including:

- Timestamp  
- Video ID  
- Playback session ID  
- Serving node ID  
- Variant (resolution / bitrate)  
- Segment index  
- Cache hit or miss  
- Event type (`SERVE`, `PREFETCH_START`, `PREFETCH_RESULT`)

These logs are used to prove adaptive behavior, caching effectiveness, and read-ahead behavior.

---

### 6. Third-Party Player Integration

The system must be tested using a third-party ABR player (e.g., hls.js, Shaka Player, VLC, Safari HLS).

The final demo must show:

- Playback under changing network conditions
- Bitrate/resolution switching
- Corresponding server-side logs proving variant changes and cache behavior

---

### 7. Local Event Logs

Each service maintains an append-only local event log.

- Upload events
- Processing state transitions
- Task retries and failures
- Segment fetch, cache, and read-ahead behavior

Logs are not replicated and are intended for debugging, reasoning, and demonstration.

---

## Distributed Systems Challenges You Must Address

You are expected to **design, implement, and explain** how your system handles:

- Idempotent uploads and processing
- Task orchestration under partial failure
- Retry semantics and duplicate suppression
- Concurrent uploads and playback
- Sticky routing and failure reassignment
- Cache and read-ahead behavior under adaptive streaming
- Observability of system behavior via logs

---

## System Diagrams (Required)

You must provide **three diagrams** during the project:

1. **Initial Architecture Diagram**  
   Upload, storage, processing, orchestration, and streaming components.

2. **Distributed Coordination Diagram**  
   Job lifecycle, retries, caching and read-ahead behavior, routing semantics, and failure paths.

3. **Final As-Built Diagram**  
   Matches what actually runs in your implementation.

Diagrams must reflect **real behavior**, not aspirational designs.

---

## What Is Explicitly Out of Scope

To keep the project focused and achievable:

- Implementing a distributed object store
- DRM or encryption
- User authentication systems
- Full-featured web UI
- Commercial CDN features (geo-routing, DNS tricks, global invalidation)
- Live streaming
- Video editing features
- Multi-region deployment

---

## Expected Deliverables

### 1. Working System
- Upload, processing, and streaming pipeline
- Orchestration with retries and failure handling
- Standards-compatible adaptive streaming with caching and read-ahead
- Third-party player playback with provable ABR behavior

### 2. Design & Scope Documents
- Initial scope document
- Updated scope reflecting implemented behavior
- Final design document describing:
  - Architecture
  - Orchestration semantics
  - Routing, caching, and read-ahead semantics
  - Failure behavior and guarantees

### 3. Tests & Demo Artifacts
- Upload idempotency tests
- Failure injection (e.g., kill worker or segment node)
- Adaptive bitrate switching demonstration
- Server logs proving cache hits, read-ahead, and variant selection

---

## Team Size and Time Expectations

This project is designed for **4 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
