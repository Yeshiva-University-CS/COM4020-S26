# Distributed Mesh VPN

## Project Overview

In this project, you will design and implement a distributed mesh VPN that allows a group of machines to connect to each other securely and directly, without routing traffic through a central server.

Each machine runs a background service that discovers other machines in the mesh, establishes encrypted WireGuard tunnels to them, and updates those connections as machines join, leave, or fail. All application traffic flows peer-to-peer over these tunnels.

A small coordination service maintains basic network information, including which machines are part of the mesh, their assigned private mesh IP addresses, and the connection details needed to form tunnels. VPN traffic never passes through this service.

The focus of the project is on distributed systems behavior: agreement on membership, correct handling of failure and restart, safe reconfiguration over time, and a clear separation between coordination and data flow.

You are not expected to implement VPN protocols, kernel networking code, or cryptographic algorithms. The emphasis is on correctness and clear behavior in a dynamic, failure-prone distributed system.

---
### 1. [Components](components.md)
### 2. [Deliverables & Milestones](milestones.md)
---

## Resulting System Behavior

When the system is running correctly:

* All active machines form a full mesh of encrypted, point-to-point connections
* Application traffic flows directly between peers
* Stable mesh IPs are used for communication
* Existing connections continue to operate if the coordination service is temporarily unavailable

---

## Distributed Systems Challenges You Will Need to Address

You are expected to design, implement, and explain how your system handles:

* Tracking which machines are part of the mesh and keeping that information consistent
* Detecting when machines are alive, unreachable, or have returned
* Updating connections correctly as machines join, leave, fail, or restart
* Handling machine crashes and restarts without manual cleanup
* Dealing with temporarily outdated or incomplete information
* Safely reapplying local VPN configuration without breaking existing connections
* Continuing peer-to-peer communication when the coordination service is unavailable
* Demonstrating real application traffic over a mesh that changes over time
* Making system behavior observable through logs and inspection of state

---

## Team Size and Time Expectations

This project is designed for **3 students**, working approximately **10 hours per week per student** over **14 weeks**.

Every milestone requires a **running, deployable artifact**; documentation must describe what actually runs.

