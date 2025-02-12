---
title: "Crate Architecture"
description: "An overview of the Nautilus PKI architecture, including its modular design, Rust trait-based implementation, and cryptographic scheme integration."
summary: "Learn how Nautilus PKI is structured, how it uses Rust traits for extensibility, and how it integrates various cryptographic schemes."
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 996
toc: true
sidebar:
  collapsable: true
seo:
  title: "Nautilus PKI Architecture"
  description: "An in-depth look at how Nautilus PKI is designed using Rust traits and cryptographic schemes."
  canonical: ""
  robots: ""
---

Nautilus PKI is designed with a modular and extensible architecture, leveraging Rust traits to define cryptographic behaviors while supporting multiple cryptographic schemes. This approach ensures flexibility, security, and future-proofing for both classical and post-quantum cryptography.

Key aspects of Nautilus PKI's architecture:

- Trait-based abstraction for cryptographic functions
- Interchangeable cryptographic schemes using a unified API
- Optimized serialization for key management and transmission
- Secure key exchange and digital signatures for authentication and encryption
- Performance benchmarking for evaluating cryptographic efficiency

---

## Rust Traits for Cryptographic Operations

Nautilus PKI uses Rust traits to define standard cryptographic functionalities. This allows different algorithms to share a common interface while maintaining their unique cryptographic properties.

### 1. PKITraits (Core Cryptographic Functionality)

The `PKITraits` trait defines the essential operations required for a cryptographic key pair.

```rust
pub trait PKITraits {
    type PublicKey;
    type PrivateKey;
    type Error;

    fn generate_key_pair() -> Result<Self, Self::Error>;
    fn sign(&self, message: &[u8]) -> Result<Vec<u8>, Self::Error>;
    fn verify(&self, message: &[u8], signature: &[u8]) -> Result<bool, Self::Error>;
}
```

### 2. KeyExchange (Secure Key Agreement)
The KeyExchange trait enables secure key agreement, allowing encrypted communication between parties.

```rust

pub trait KeyExchange {
    type PublicKey;
    type PrivateKey;
    type Error;

    fn encapsulate(
        public_key: &Self::PublicKey,
        context: Option<&[u8]>
    ) -> Result<(Vec<u8>, Vec<u8>), Self::Error>;

    fn decapsulate(
        private_key: &Self::PrivateKey,
        ciphertext: &[u8],
        context: Option<&[u8]>
    ) -> Result<Vec<u8>, Self::Error>;
}


```
- Defines encapsulation and decapsulation for key exchange.
- Used by RSA-OAEP, ECDH (SECP256K1, P-256), and Kyber.


### 3. KeySerialization (Efficient Key Storage & Transmission)
The KeySerialization trait enables serialization and deserialization of cryptographic keys for storage and communication.


```rust
pub trait KeySerialization {
    fn to_bytes(&self) -> Vec<u8>;
    fn from_bytes(bytes: &[u8]) -> Result<Self, String> where Self: Sized;
}


```
- Supports CBOR, DER, and PEM formats.
- Ensures compatibility with existing security frameworks like TLS and OpenPGP.

## What This Means for Developers

By structuring Nautilus PKI around Rust traits, the system remains modular, flexible, and easy to extend. This means that developers can integrate different cryptographic schemes without modifying the core infrastructure.

#### Advantages of Using Traits

- Uniform API: All cryptographic schemes implement the same interface, simplifying usage.
- Extensibility: Developers can add new cryptographic algorithms by implementing the provided traits.
- Code Reusability: Traits allow multiple cryptographic implementations to share common logic.
- Security & Performance: By defining strict interfaces, Nautilus PKI enforces best practices in cryptographic operations.

#### How Developers Can Take Advantage of This

1. Swap Cryptographic Schemes Easily

    Developers can choose between RSA, ECDSA, Ed25519, or post-quantum schemes by simply switching implementations without rewriting application logic.

2. Extend Nautilus PKI with Custom Algorithms

    If a new cryptographic standard is introduced, developers can integrate it by implementing the required traits.

## Supported Cryptographic Schemes

Nautilus PKI is algorithm-agnostic, supporting both classical and post-quantum cryptographic algorithms.

| Operation | Classical Algorithms | Post-Quantum Algorithms |
| --- | --- | --- |
| Key Exchange | RSA-OAEP, ECDH (SECP256K1, P-256) | Kyber |
| Digital Signatures | RSA, ECDSA (P-256, SECP256K1), Ed25519 | Falcon, Dilithium, SPHINCS+ |
| Hashing | SHA-256, SHA-512 | Shake256, BLAKE2b |
| Key Serialization | DER, PEM | CBOR |

This modular approach allows developers to swap cryptographic schemes without modifying core infrastructure.

For a detailed technical overview, refer to our **Code Documentation**:

<a href="/docs/attachments/nautilus-public-key-infrastructure-code-documentation.pdf" download type="application/pdf">📄 Download Nautilus PKI Code Documentation (PDF)</a>
