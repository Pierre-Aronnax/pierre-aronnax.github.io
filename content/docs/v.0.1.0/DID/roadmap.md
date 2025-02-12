---
title: "Roadmap"
description: "The development roadmap for Nautilus DID, outlining upcoming improvements and planned features."
summary: "Explore the phased roadmap for Nautilus DID, including security enhancements, interoperability, and decentralized identity expansion."
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 998
toc: true
sidebar:
  collapsed: true
seo:
  title: "Nautilus DID Roadmap"
  description: "An outline of Nautilus DID's development roadmap and planned improvements."
  canonical: ""
  robots: ""
---


Nautilus DID is designed to provide self-sovereign, decentralized identity management with strong cryptographic security and interoperability. As the landscape of decentralized identity evolves, Nautilus DID will continue to expand its capabilities, ensuring compliance with emerging standards, enhancing security, and improving usability.

This roadmap outlines the phased development plan for improving Nautilus DID, focusing on security, scalability, and real-world adoption.

---

## Current Status

Nautilus DID currently provides a fully functional decentralized identity framework with support for:

- DID Creation & Management – Users can generate and manage DIDs with associated public keys.
- Verifiable Credentials (VCs) – Issuers can generate signed credentials that are cryptographically verifiable.
- Multi-Scheme Cryptographic Support – RSA, Ed25519, Falcon, Dilithium, and SPHINCS+ for post-quantum security.
- DID Document Resolution – Supports JSON-based DID Documents for identity verification.
- Basic Authentication – Users can authenticate by signing messages with their DID-linked private key.

### Limitations & Areas for Improvement
While the system is functional, the following enhancements are planned:

- Federated Identity Support – No trust registry for verifying third-party identity issuers.
- Credential Revocation – No built-in method for revoking previously issued credentials.
- Decentralized Storage – DID Documents and credentials are stored locally without external storage support.
- OAuth & OpenID Connect – No direct integration for traditional authentication methods.

---

## Phase 1: Security & Interoperability Enhancements

- Expanded Support for Federated Identity Providers (IdPs)
  - Enable multi-authority verification to allow trusted third parties to authenticate users.
  - Introduce a DID Trust Registry for managing federated issuers.

- Improved Credential Issuance & Verification
  - Support for credential expiration & revocation lists to prevent misuse.
  - Enhance multi-signature credential issuance for stronger security.

- Cross-Chain DID Anchoring
  - Implement Ethereum, Solana, and Hyperledger integrations for DID anchoring.
  - Allow blockchain-based verification of Verifiable Credentials (VCs).

---

## Phase 2: Decentralized Storage & Authentication

- Decentralized Storage Integration
  - Enable IPFS, AWS KMS, and decentralized cloud support for storing DID documents.
  - Improve DID resolution speed by caching decentralized storage results.

- Delegated & Multi-Party Authentication
  - Allow users to delegate authentication to trusted authorities.
  - Introduce multi-party verification, where multiple authorities sign off on a credential.

- Post-Quantum Cryptography (PQC) Readiness
  - Expand post-quantum support with more NIST-approved cryptographic algorithms.
  - Improve Dilithium, Falcon, and SPHINCS+ optimizations for better performance.

---

## Phase 3: Future Innovation & Standardization

- Zero-Knowledge Proofs (ZKPs) for Privacy-Preserving Identity
  - Implement ZK-SNARKs & ZK-STARKs for selective disclosure of identity attributes.
  - Enable privacy-focused verifiable credentials where users can prove identity without exposing all details.

- OAuth & OpenID Connect (OIDC) Compatibility
  - Develop a DID-to-OAuth bridge, allowing seamless authentication in existing web applications.
  - Enable passwordless login with DID-based authentication in traditional systems.

- Self-Sovereign & Decentralized Identity Networks
  - Expand DID resolution to support peer-to-peer identity verification without centralized services.
  - Develop governance models for decentralized identity ecosystems.

---

## Conclusion

The Nautilus DID roadmap reflects the project's commitment to security, interoperability, and usability. By integrating federated authentication, decentralized storage, and blockchain-based verification, Nautilus DID aims to become a leading solution for self-sovereign identity management.
