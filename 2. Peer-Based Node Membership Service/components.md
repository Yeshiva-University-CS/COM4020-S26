## Components You Will Build

You will build a peer-based node membership system. It includes a running per-node service and a small set of required definitions that make system behavior clear and usable by other systems.

---

## A. Running Component (Process)

### 1. Per-Node Membership Service

A long-running service that runs on each participating machine and manages its local participation in the membership system.

It will:

* Maintain a stable local node identity
* Allow the node to join and leave the system on demand
* Discover and communicate with peer nodes
* Exchange membership and liveness information
* Detect unresponsive peers using periodic signals
* Update local membership state as information changes
* Expose local status and membership information for inspection

A node that leaves voluntarily must be reflected correctly in the membership views of remaining nodes.

Each node independently updates its local view until it reflects the observed state of the system.

---

## B. Required Artifacts (Not Processes)

These are not separate running services. They are required artifacts that define what your running system means and how other systems interact with it.

### 2. Membership and Failure Model

A shared behavioral model implemented consistently across all nodes.

It defines:

* What it means for a node to be considered active, unreachable, or removed
* How voluntary departure is represented and propagated
* How long a node may be unresponsive before being treated as failed
* How conflicting or outdated information is handled
* How membership changes propagate and converge over time

The model must be simple, documented, and observable in practice.

---

### 3. External Control Interface (Code Interface)

A small, explicit code-level interface that other systems can use to interact with the node membership service.

It will:

* Provide the current membership view
* Indicate which nodes are considered reachable
* Reflect voluntary joins and departures
* Surface membership changes over time
* Hide internal coordination mechanics from the caller

The interface exists to decouple coordination behavior from any specific consumer.
