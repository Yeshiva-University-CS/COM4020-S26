# Video Upload and Streaming System

## Project Overview

In this project, you will design and implement a distributed system responsible for accepting large video uploads, storing video data in S3-compatible object storage, preparing videos for adaptive streaming, and serving video streams to clients.

The system exposes HTTP APIs for upload, processing status, and streaming. Uploading a video is a state-creating operation whose correctness depends on durable storage and clearly defined ownership of video data as it moves through processing and streaming stages.

Uploaded videos are stored in external object storage (MinIO or AWS S3). The system is responsible for coordinating segmentation, transcoding, and streaming behavior on top of this storage.

Multiple nodes cooperate to process videos, serve adaptive streams, and handle concurrent clients. The emphasis of the project is on distributed systems behavior: coordination between services, correctness under concurrency, failure handling, and recovery. Media quality and encoding efficiency are not the focus.

System behavior must be observable through logs and state inspection.

You are not expected to implement a distributed object store, CDN, or production media pipeline. The focus is on correctness and explainable behavior.

---
### 1. [Components](components.md)
### 2. [Deliverables & Milestones](milestones.md)
---

## Resulting System Behavior

When the system is running correctly:

* Clients can upload large video files through an HTTP API
* Uploaded video data is stored durably in S3-compatible storage
* Each uploaded video has a stable identifier and observable lifecycle state
* Videos are segmented and transcoded asynchronously after upload
* Processing outputs are written back to object storage
* Streaming is possible only after processing completes
* Adaptive bitrate streaming is supported through segmented outputs
* Bitrate changes during playback may be served by the same or different streaming nodes
* Concurrent uploads, processing, and streaming do not corrupt shared state
* Repeated or retried operations do not duplicate processing
* Node failures do not result in lost or partially visible video data
* Restarted nodes recover state and resume correct operation
* System behavior under concurrency, failure, and recovery is observable

---

## Distributed Systems Challenges

You are expected to design, implement, and explain how your system handles:

* Durable acceptance of large uploads
* Coordination between upload, processing, and streaming services
* Ownership and lifecycle of video data
* Idempotent processing and retry behavior
* Concurrent uploads and concurrent streaming
* Segmentation and transcoding under partial failure
* Serving adaptive streams while other operations are ongoing
* Bitrate changes in the presence of multiple streaming nodes
* Node crashes during processing or streaming
* Restart and recovery without manual intervention
* Consistent behavior across multiple nodes
* Making system behavior observable and explainable

---

## Team Size and Time Expectations

This project is designed for **4 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.
