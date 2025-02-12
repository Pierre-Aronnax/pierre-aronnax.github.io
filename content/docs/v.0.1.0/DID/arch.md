---
title: "Architecture"
description: ""
summary: ""
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 997
toc: true
sidebar:
  collapsed: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---
Nautilus DID is built with a modular and extensible architecture, enabling seamless management of decentralized identities (DIDs), verifiable credentials (VCs), and cryptographic key management. The framework follows W3C DID standards while integrating post-quantum cryptography and trustless authentication.

The architecture is structured around Rust traits, allowing developers to extend functionality without modifying core components.

---

## Crate Structure

The Nautilus DID crate consists of multiple modules, each responsible for a different aspect of identity management.

| Module              | Description  |
|-------------------------|-----------------|
| `did.rs`               | Core DID creation and management. |
| `did_document.rs`      | Handles DID Document structure, including public keys, authentication, and services. |
| `identity_mgmt.rs`     | Manages identity storage, retrieval, and updates. |
| `identity_flow.rs`     | Defines identity lifecycle operations, including authentication. |
| `vc.rs`               | Implements Verifiable Credentials (VCs) for identity verification. |
| `credential_issuance.rs` | Issues signed identity credentials for secure verification. |
| `key_mgmt.rs`         | Manages cryptographic keys for DIDs and credentials. |
| `pki_factory.rs`      | Provides pluggable PKI implementations supporting multiple cryptographic algorithms. |
| `identity_error.rs`   | Standardized error handling across identity operations. |

Each module operates independently, making it possible to extend or replace components without breaking existing functionality.

---

## Trait-Based Design

Nautilus DID follows a trait-based approach, defining core operations that any new identity system or cryptographic scheme can implement.

### `DIDCore` Trait – Core DID Functionality

Defines fundamental operations required for DID generation, updates, and verification.

```rust
pub trait DIDCore {
    fn generate_did(&self) -> String;
    fn resolve_did(did: &str) -> Result<DIDDocument, IdentityError>;
    fn update_did(&mut self, updates: DIDUpdate) -> Result<(), IdentityError>;
}
```


- Allows different DID methods (e.g., `did:nautilus`, `did:web`, `did:peer`).
- Standardized DID resolution across different storage backends.

---

### `VerifiableCredential` Trait – Digital Identity Proofs

Defines how VCs are issued, stored, and verified.

```rust

pub trait VerifiableCredential {
    fn issue_credential(&self, subject: &str, claims: HashMap<String, String>) -> Result<VC, IdentityError>;
    fn verify_credential(&self, credential: &VC) -> Result<bool, IdentityError>;
}

```

- Supports cryptographic proofs using Ed25519, Falcon, Dilithium, and RSA.
- Verifies identity claims with cryptographic signatures.

---

### `KeyManagement` Trait – Secure Key Handling

Defines how cryptographic key pairs are generated, stored, and retrieved.

```rust
pub trait KeyManagement {
    fn generate_keypair(&self, algorithm: Algorithm) -> Result<(Vec<u8>, Vec<u8>), IdentityError>;
    fn get_public_key(&self, did: &str) -> Result<Vec<u8>, IdentityError>;
    fn sign(&self, message: &[u8]) -> Result<Vec<u8>, IdentityError>;
    fn verify(&self, message: &[u8], signature: &[u8]) -> Result<bool, IdentityError>;
}

```

- Supports multiple cryptographic algorithms for signing and verification.
- Ensures compatibility with W3C DID standards.

---

## Identity Flow

The Nautilus DID system follows a four-step identity lifecycle:

1. DID Creation
    - Users generate a new DID and DID Document.
    - The public key is stored in the document for authentication.
2. Verifiable Credential Issuance
    - Trusted issuers create signed credentials for users.
    - Credentials are stored securely and linked to the user’s DID.
3. Identity Verification
    - Verifiers check credentials against the issuer’s public key.
    - If valid, the user is authenticated.
4. DID Updates & Revocation
    - Users can update their DID Document (e.g., add a new key).
    - Credentials can be revoked by the issuer.

---

## Supported Cryptographic Schemes

Nautilus DID is crypto-agnostic, allowing multiple cryptographic schemes for identity verification.

| Algorithm | Purpose |
| --- | --- |
| RSA | Standard PKI-based authentication. |
| Ed25519 | Efficient signature verification. |
| Falcon | Post-quantum signature scheme. |
| Dilithium | Post-quantum secure authentication. |
| SPHINCS+ | Stateless hash-based signatures. |

Each identity can choose a cryptographic scheme, ensuring future-proof security.



For a detailed technical overview, refer to our **Code Documentation**:

<a href="/docs/attachments/nautilus-decentralized-identity-code-documentation.pdf" download type="application/pdf">📄 Download Decentralized Identity Code Documentation (PDF)</a>
