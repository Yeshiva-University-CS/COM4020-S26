# Project Proposal: Distributed Mesh VPN

## Summary 

This project designs and implements a distributed mesh VPN composed of a centralized control plane and autonomous node daemons. The system forms a full-mesh overlay network using WireGuard to establish secure, point-to-point tunnels between all participating nodes. The control plane manages membership, public key distribution, and liveness, while node daemons independently configure WireGuard, maintain direct tunnels to peers, and react to membership changes. Each node is assigned a unique private mesh IP, providing stable identity and clean routing within the overlay. The system intentionally excludes multi-hop routing and broadcast; all communication occurs via direct unicast between peers.

---

## Deliverables

* A running multi-node mesh VPN deployment
* A control-plane service with a documented API
* A node daemon capable of dynamic WireGuard configuration
* Demonstrations of secure multi-node communication, membership changes, and failure handling

---

## Core Components

### 1. Control Plane

A centralized coordination service responsible for defining authoritative mesh state.

**Responsibilities:**

* Maintaining a registry of node metadata
* Assigning unique mesh IP addresses
* Distributing public keys and peer lists
* Tracking node liveness via periodic heartbeats
* Exposing a simple API for registration and state retrieval

The control plane defines participation and membership but does not carry data-plane traffic.

---

### 2. Node Daemon

A daemon process running on each participating node that consumes control-plane state and manages local networking.

**Responsibilities:**

* Registering with the control plane and sending heartbeats
* Fetching updated peer lists and cryptographic material
* Dynamically configuring WireGuard interfaces and peers
* Establishing direct tunnels to all peers (full mesh)
* Installing routing rules mapping mesh IPs to WireGuard tunnels
* Updating or tearing down tunnels as peers join, leave, or fail

Each node independently converges on the correct mesh configuration.

---

### 3. Secure Data Plane

A decentralized, encrypted overlay network built on WireGuard.

**Responsibilities:**

* Providing authenticated, encrypted point-to-point transport
* Using static AllowedIPs mappings to associate mesh IPs with peer endpoints
* Maintaining a stable overlay independent of underlying LANs, WANs, or NATs

Once configured, the data plane operates without centralized involvement.

---

## Distributed Systems Behaviors

- **Membership convergence**  
  Nodes independently update their local view of the mesh based on control-plane state

- **Failure handling**  
  Unresponsive nodes are detected via liveness checks and removed from peer configurations

- **Dynamic reconfiguration**  
  Nodes add, update, or remove tunnels and routes as membership changes

- **Control/data plane separation**  
  Coordination is centralized; connectivity and data transfer are decentralized

---

## Constraints (not in scope)

* Full-mesh topology only
* No multi-hop routing
* No broadcast or multicast traffic
* All communication is direct unicast between mesh IPs

