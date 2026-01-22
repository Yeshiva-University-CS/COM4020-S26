## Components You Will Build

You will build a distributed mesh VPN system composed of three cooperating components. Each component has a clear responsibility and runs as its own process.

---

### 1. Coordination Service

A small service responsible only for helping machines find and connect to each other.

It will:

* Keep track of which machines are currently part of the mesh
* Assign each machine a stable private mesh IP address
* Store and distribute WireGuard public keys and connection details
* Track whether machines are responsive via periodic heartbeats
* Expose a simple HTTP API for registration and state retrieval

This service never handles VPN traffic. It exists only to provide machines with the information needed to form correct peer-to-peer connections.

---

### 2. Per-Node Background Service

A long-running service that runs on each participating machine and manages its local VPN configuration.

It will:

* Register the machine with the coordination service
* Periodically report that the machine is alive
* Fetch the current list of other machines in the mesh
* Create, update, and remove WireGuard tunnels as membership changes
* Configure routing so mesh IPs are reached through the correct tunnels

Each machine independently updates its local configuration until it reflects the current set of active peers, without requiring manual intervention.

---

### 3. Messaging Demonstration Application

A small application used only to demonstrate that the mesh VPN works correctly.

It will:

* Run on each machine
* Display the list of currently connected peers
* Allow a user to select a peer and send messages directly to it
* Use only mesh IPs for communication

The messaging application exists solely to demonstrate that real application traffic can flow correctly over the mesh VPN.
