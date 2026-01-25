## Components You Will Build

You will build a decentralized group chat system composed of running peer processes and a small set of clearly defined subsystems. Authority boundaries must be explicit and unambiguous.

The system is structured into three subsystems:

* Node Runtime and Communication  
* Membership and Presence  
* Messaging and Recovery  

---

## A. Running Components (Processes)

### 1. Chat Node Service

A long-running peer process that hosts all subsystems.

It will:

* Bind to the private network interface
* Maintain a stable node identity
* Track restart incarnation
* Send and receive framed messages
* Maintain local membership state
* Maintain local messaging state for group and private messages
* Expose inspection output and structured logs

No node is authoritative for membership or routing globally.

---

## B. Subsystems, Responsibilities, and Boundaries

### Subsystem 1 — Node Runtime and Communication

#### Responsibilities

* Manage node startup, shutdown, and restart behavior
* Ensure stable node identity across restarts
* Send and receive messages to and from peers
* Dispatch incoming messages to other subsystems
* Expose node-level status and logs

#### Boundaries

* Does not decide who is a member of the system
* Does not define chat semantics
* Does not interpret message meaning beyond basic framing

This subsystem exists to ensure that nodes can run, communicate, and explain their local activity.

---

### Subsystem 2 — Membership and Presence

#### Responsibilities

* Track which peers appear to be participating
* Detect peer joins, failures, and recoveries
* Maintain a decentralized view of membership
* Distinguish restart from stale or delayed traffic
* Notify other subsystems of membership changes

#### Boundaries

* No node is authoritative for membership globally
* Membership views may temporarily disagree across nodes
* Failure detection is best-effort and based on observation

This subsystem answers the question: *“Who do I believe is currently participating?”*

---

### Subsystem 3 — Messaging and Recovery

#### Responsibilities

* Implement the single shared group chat room
* Implement point-to-point private messaging
* Assign unique identifiers to messages
* Safely handle duplicate and replayed messages
* Base delivery claims on explicit evidence
* Support recovery of recent group messages after failure or restart

Group message recovery works as follows:

* A recovering node may request recent group messages from peers
* Peers respond with messages they still retain
* Recovery is best-effort and bounded
* Missing messages and gaps are allowed and must be visible

Private messaging includes:

* Explicit acknowledgments
* Delivery timeouts
* Bounded retry rules
* Safe behavior across sender and receiver restart

#### Boundaries

* No central broadcaster or message router
* No global ordering guarantees
* No promise of universal delivery
* No permanent or complete message history

This subsystem defines *what messages mean* and *what outcomes are allowed*.


