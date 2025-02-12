---
title: "Crates Structure"
description: ""
summary: ""
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 887
toc: true
sidebar:
  collapsed: true
seo:
  title: "V.0.1.0 Crates Structure" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---


## Overview

Nautilus is a decentralized networking and security project composed of multiple modular crates, each responsible for a specific aspect of the ecosystem. Below is a table summarizing the different crates and their functionalities.

## Crate Breakdown

| Crate Name              | Description |
|-------------------------|-------------|
| `nautilus_pki`         | Handles Public Key Infrastructure (PKI) for decentralized security. Implements cryptographic algorithms like Dilithium, ECDSA, Falcon, Kyber, etc. |
| `nautilus_key_storage` | Manages secure key storage, integrates with external key managers (e.g., HashiCorp Vault, AWS KMS). |
| `nautilus_did`        | Implements Decentralized Identifiers (DIDs) and supports Verifiable Credentials (VCs) for authentication. |
| `nautilus_mdns` | Implemnted mDNS for peer discovery and allows state-based backoff with support for IPv6, and DNS Tunneling coming in |
| `nautilus_kad` | Mainly responsible for maintaining a routing table internally |
| `nautilus_handshake` | Mainly responsible for setting up a Handshake framework to allow developers to customize the handshake for different situations |
| `nautilus_negotitation` | Its a negotiation framework that can allow negotitation to be implemented in Nautilus protocols |
| `nautilus_ping` | The Ping is a simple protocol can ping other devices to see if its reachable |
| `nautilus_tls` | The Custom TLS Layer we use in our Nautilus that incoporates PQC and Traditional Symmetric Encryptions |
| `nautilus_data_encryption` | The Data Encryption Layer will employ common symmetric schemes like AES, 3DES, and ChaCha20 |
| `nautilus_Data_authentication` | The data Authnetication layer will employ common schemes like hmac, cmac and hash-chain for verification and authentication |
| `nautilus_tcp` | The nautilus TCP is our implementation of TCP to open up TCP Communication lines |
| `nautilus_udp` | The nautilus UDP is our implementation of UDP to open up UDP Communication lines |
| `nautilus_registry` | The Nautilus registry is a utility crate that contains data storage related methods like pipeline management and sharding that leads to databases like Redis and many other for storage |
| `nautilus_certificate` | The Nautilus Certificate is another uitlity that will be responsible for certificate formmating within Nautilus Ecosystem |

## Functional Overview

1. Security & Cryptography

   - `nautilus_pki`: Manages the Public Key Infrastructure (PKI), implementing various cryptographic algorithms such as Dilithium, ECDSA, Falcon, Kyber, and more to provide robust decentralized security.
   - `nautilus_tls`: Implements a custom TLS layer, incorporating both Post-Quantum Cryptography (PQC) and traditional symmetric encryption methods for secure communication.
   - `nautilus_data_encryption`: Provides a data encryption layer utilizing well-known symmetric encryption schemes such as AES, 3DES, and ChaCha20 for data protection.
   - `nautilus_Data_authentication`: Focuses on data authentication using common techniques such as HMAC, CMAC, and hash-chain methods to verify data integrity and authenticity.

2. Identity & Authentication

   - `nautilus_did`: Handles Decentralized Identifiers (DIDs) and supports the use of Verifiable Credentials (VCs) for decentralized authentication and identity management.
   - `nautilus_certificate`: Manages certificate formatting within the Nautilus ecosystem, providing standardized support for certificates used in authentication processes.

3. Networking & Discovery

   - `nautilus_kad`: Implements the Kademlia distributed hash table (DHT), maintaining a decentralized routing table for peer-to-peer communication.
   - `nautilus_mdns`: Implements mDNS (Multicast DNS) for local peer discovery, with state-based backoff support and IPv6 compatibility. Future enhancements will include DNS tunneling capabilities.
   - `nautilus_ping`: Provides a simple protocol to ping other devices and determine their reachability within the network.
   - `nautilus_tcp`: A custom implementation of the TCP protocol to open up TCP communication lines for reliable data transfer.
   - `nautilus_udp`: Implements the UDP protocol for communication, providing an unreliable, connectionless data transfer method for high-speed applications.

4. Key & Storage Management

   - `nautilus_key_storage`: Manages secure key storage, integrating with external key management systems such as HashiCorp Vault and AWS KMS for enhanced security.
   - `nautilus_registry`: A utility crate for data storage management, including pipeline management, sharding, and integration with databases like Redis for efficient data storage and retrieval.

5. Handshake & Negotiation

   - `nautilus_handshake`: Provides a framework for setting up customizable handshakes, allowing developers to adapt the handshake process to specific use cases or protocols.
   - `nautilus_negotitation`: A negotiation framework for enabling different Nautilus protocols to engage in dynamic negotiation for secure communications or resource sharing.

## Conclusion

The Nautilus ecosystem is designed as a highly modular, decentralized networking and security system. By breaking down each functionality into specialized crates, Nautilus ensures a flexible, scalable, and secure platform for building decentralized applications. The integration of cryptographic, networking, identity management, and storage solutions makes Nautilus a comprehensive system that can be easily extended and adapted to new protocols or requirements in the rapidly evolving landscape of decentralized technologies.
