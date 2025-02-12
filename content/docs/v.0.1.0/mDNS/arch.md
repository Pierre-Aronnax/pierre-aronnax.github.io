---
title: "Architecture"
description: "A breakdown of Nautilus mDNS architecture, its modular components, and multicast-based service discovery."
summary: "Understand how Nautilus mDNS is structured, including its packet processing, service registry, and event-driven architecture."
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 997
toc: true
sidebar:
  collapsed: true
seo:
  title: "Nautilus mDNS Architecture"
  description: "An in-depth look at how Nautilus mDNS is designed using multicast communication and decentralized service discovery."
  canonical: ""
  robots: ""
---

# Architecture

## Overview

Nautilus mDNS follows a modular, event-driven design to enable decentralized service discovery. The system consists of independent components responsible for packet handling, service registration, name resolution, and event management.

It operates using UDP multicast for queries and responses, following the Multicast DNS (mDNS) standard.

---

## Core Components

| Module              | Functionality  |
|-------------------------|------------------|
| `mdns_service.rs`      | Registers and announces local services. |
| `mdns_registry.rs`     | Maintains a record of discovered services and nodes. |
| `mdns_event.rs`        | Handles incoming mDNS queries and responses. |
| `mdns_error.rs`        | Defines error handling for mDNS packet processing. |
| `packet.rs`            | Constructs and parses mDNS packets. |
| `name.rs`              | Resolves and formats service names. |
| `record.rs`            | Manages DNS-like records (`A`, `PTR`, `SRV`, `TXT`). |

Each component works independently but integrates seamlessly to ensure efficient service discovery.

---

## How It Works

### 1️⃣ Service Registration
- When a device or service starts, it registers itself using SRV (Service) and TXT (Metadata) records.
- This registration is broadcast over UDP multicast to inform other devices.

### 2️⃣ Service Discovery
- A device sends a multicast DNS query to the network to locate available services.
- Devices that match the query respond with their service details.

### 3️⃣ Name Resolution
- Devices can map hostnames (e.g., `printer.local`) to local IP addresses using `A` records.
- No centralized DNS server is required; the system resolves names dynamically.

---

## Multicast Communication

| Protocol  | Purpose  | Transport  |
|--------------|-------------|--------------|
| mDNS Queries | Request information about services | UDP Multicast |
| mDNS Responses | Provide service details to requesting nodes | UDP Unicast/Multicast |
| Periodic Announcements | Broadcast service availability updates | UDP Multicast |
| TTL Expiration | Remove inactive services | Internal Cleanup |

### Multicast Address & Port
- IPv4 Multicast Address: `224.0.0.251`
- Port: `5353` (Standard mDNS Port)

All devices listen to this multicast address for service announcements and queries.


## Why This Architecture Works
- Fully Decentralized – No central DNS server required.
- Lightweight & Efficient – Uses UDP multicast for fast service discovery.
- Modular & Extendable – Each module can be expanded to support security layers, advanced caching, and IPv6.
- Compatible with existing mDNS solutions – Works alongside Apple Bonjour, Avahi, and other mDNS-based systems.

Nautilus mDNS enables seamless peer-to-peer communication, making it ideal for IoT, local networking, and decentralized applications.

