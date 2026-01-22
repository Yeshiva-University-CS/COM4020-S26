## Components You Will Build

You will build a distributed video upload and streaming system. It includes running services and a small set of required definitions that make system behavior clear and usable by other systems.

---

## A. Running Components (Processes)

### 1. Video Upload API Service

A long-running HTTP service responsible for accepting video uploads and recording upload state.

It will:

* Accept large video uploads via HTTP
* Persist uploaded video data to S3-compatible storage
* Generate and return a stable video identifier
* Record upload completion state
* Reject or safely handle duplicate or retried uploads
* Expose video status endpoints
* Expose basic health and status endpoints

The upload service does not perform segmentation, transcoding, or streaming.

---

### 2. Video Processing Service

A background service responsible for preparing videos for adaptive streaming.

It will:

* Detect uploaded videos requiring processing
* Segment videos into streamable units
* Transcode segments into multiple representations
* Write all processing outputs to object storage
* Record processing progress and completion state
* Avoid duplicate work under retries
* Remain correct under partial failure
* Recover in-progress processing after restart

Processing behavior must be deterministic and observable.

---

### 3. Video Streaming Service

A long-running HTTP service responsible for serving adaptive video streams.

It will:

* Serve streaming manifests and segments derived from processed videos
* Support adaptive bitrate streaming behavior
* Serve requests from one or more nodes without assuming fixed server assignment
* Reject requests for videos not yet ready
* Handle concurrent client requests
* Remain correct under node failure
* Expose basic health and status endpoints

The streaming service does not accept uploads or perform processing.

---

## B. Required Artifacts (Not Processes)

These are not separate running services. They are required artifacts that define what your running system means and how components interact.

---

### 4. Video Lifecycle and Processing Model

A shared behavioral model implemented consistently across all components.

It defines:

* Video identity and storage locations
* Valid lifecycle states
* State transitions for upload, processing, and readiness
* When segmentation and transcoding may occur
* When streaming is permitted
* How retries and concurrent requests are handled
* Visibility rules for partially processed videos

The model must be documented and observable in practice.

---

### 5. External API Contract

A small, explicit interface describing how clients interact with the system.

It will:

* Define upload, status, and streaming endpoints
* Specify request and response formats
* Specify error and *not found* behavior
* Describe lifecycle and visibility guarantees
* Hide internal coordination, storage layout, and processing details

The contract exists to decouple system behavior from implementation details.
