---
title: "Architecture"
description: ""
summary: ""
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 995
toc: true
sidebar:
  collapsed: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
Nautilus KAD is a Rust-based Kademlia Distributed Hash Table (DHT) implementation, designed for decentralized peer-to-peer networking, efficient routing, and distributed storage. It enables scalable, fault-tolerant, and low-latency network communication, making it ideal for Web3 applications, decentralized storage, and P2P networks.

The system structures nodes in a distributed overlay network, where each node is responsible for routing, storing, and retrieving data based on XOR distance calculations.

---

### Core Components of Nautilus KAD

- kad_protocol.rs – Handles Kademlia query logic, including `FindNode`, `FindValue`, and `Store` operations.
- kad_message.rs – Defines the message format for communication between nodes.
- routing_table.rs – Manages peer organization using XOR distance-based buckets.
- xor_distance.rs – Implements the XOR metric to determine which nodes are closest to a given target.
- node.rs – Represents a network node with a unique `NodeID` and network address.
- bootstrap.rs – Manages the bootstrap process for new nodes joining the network.
- utils.rs – Provides helper functions such as random `NodeID` generation.

---

### How Nautilus KAD Works

1. Bootstrapping & Peer Discovery
    - A new node must connect to at least one known peer to join the network.
    - The bootstrap node provides a list of active peers, allowing the new node to populate its routing table.
2. XOR Distance & Routing
    - Nautilus KAD determines node proximity using XOR distance calculations.
    - Nodes maintain a routing table structured into buckets, each containing peers at increasing XOR distances.
    - Queries are routed progressively closer to the target, ensuring efficient lookups in O(log(n)) time.
3. Storing & Retrieving Data
    - Data is stored on the K closest nodes to a given key using a `Store` message.
    - To retrieve stored data, a node sends a `FindValue` query, which iteratively searches through the network until the data is found.
4. Self-Healing & Network Maintenance
    - Nodes periodically refresh routing tables to remove inactive peers.
    - If a node leaves the network, its stored data is redistributed among other nodes to maintain availability.

---

### Routing Table & XOR Distance Calculation

- Nautilus KAD uses binary XOR distance to determine which nodes are closest to a given key.
- Each node organizes peers into buckets based on their XOR distance from itself.
- Closer nodes are stored in more frequently accessed buckets, optimizing search performance.

Example Calculation:

If Node A has the ID `101010` and Node B has the ID `110110`:

```vbnet
XOR(101010, 110110) = 011100  (Binary)
Distance = 28 (Decimal)

```

A query is routed through the closest nodes first, progressively moving towards the target.

---

### Message Types & Network Communication

- Ping – Ensures a node is still active.
- FindNode – Requests the closest nodes to a given target.
- FindValue – Searches for a stored value within the network.
- Store – Saves a value on the K closest nodes to a given key.

All messages are serialized using bincode + serde, ensuring compact and efficient transmission.

---

### Current Capabilities

Implemented Features:

- Fully decentralized peer-to-peer routing.
- Efficient XOR distance-based lookups.
- Self-healing network that removes inactive peers automatically.
- Compact, fast message serialization using `bincode + serde`.

Limitations & Future Enhancements:

- No NAT traversal support for nodes behind firewalls.
- No data redundancy or multi-node replication.
- No cryptographic authentication for securing messages.

Nautilus KAD is designed to be scalable and adaptable, providing a foundation for secure, high-performance decentralized networks.
