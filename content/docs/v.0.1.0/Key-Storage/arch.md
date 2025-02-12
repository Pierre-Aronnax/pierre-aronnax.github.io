---
title: "Crate Architecture"
description: "An overview of the Nautilus PKI architecture, including its modular design, Rust trait-based implementation, and cryptographic scheme integration."
summary: "Learn how Nautilus PKI is structured, how it uses Rust traits for extensibility, and how it integrates various cryptographic schemes."
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 997
toc: true
sidebar:
  collapsable: true
seo:
  title: "Nautilus PKI Architecture"
  description: "An in-depth look at how Nautilus PKI is designed using Rust traits and cryptographic schemes."
  canonical: ""
  robots: ""
---

Nautilus Key Storage is built around a trait-based architecture, ensuring modularity and extensibility. Developers can integrate new storage solutions by implementing a common set of traits.

### `KeyStorage` (Core Storage API)
Defines the core interface for storing and retrieving cryptographic keys.

```rust
pub trait KeyStorage {
    type StoredType: Serialize + for<'a> Deserialize<'a>;
    type Error;

    fn initialize(&self, config: Option<&str>) -> Result<(), Self::Error>;
    fn save(&self, keypair: &Self::StoredType, location: &str, encrypt: bool) -> Result<(), Self::Error>;
    fn load(&self, location: &str, decrypt: bool) -> Result<Self::StoredType, Self::Error>;
    fn remove(&self, location: &str) -> Result<(), Self::Error>;
    fn list(&self) -> Result<Vec<String>, Self::Error>;
}
```

### `KeyStorageError` (Standardized Error Handling)

Defines a common error structure for all storage backends.

```rust
pub enum KeyStorageError {
    SaveError(String),
    LoadError(String),
    RemoveError(String),
    EncryptionError(String),
    BackendError(String),
    NotSupported(String),
    Unknown(String),
    FileError(String),
    SerializationError(String),
}

```

### `FileFormat` (Serialization Support)

Enables multiple key serialization formats such as JSON, PEM, and CBOR.

```rust

pub trait FileFormat {
    fn serialize<T: Serialize>(data: &T) -> Result<Vec<u8>, String>;
    fn deserialize<T: DeserializeOwned>(bytes: &[u8]) -> Result<T, String>;
}

```

## What This Means for Developers

The trait-based design ensures that developers can:

- Easily add new storage backends by implementing `KeyStorage`.
- Standardize error handling using `KeyStorageError` across different storage solutions.
- Support multiple serialization formats using `FileFormat`, allowing interoperability with external cryptographic tools.

Developers can choose between in-memory, file-based, OS-secured, or cloud storage without modifying their applications.

## Supported Systems and Solutions

| Storage Type | Implementation | Use Case |
| --- | --- | --- |
| In-Memory | `in_memory_key_storage.rs` | Fast, ephemeral key storage for temporary use. |
| File-Based | `file_storage.rs` | Stores keys persistently in JSON, PEM, or CBOR format. |
| Linux Keyring | `linux_keyring_storage.rs` | Uses Linux `keyctl` for OS-secured key storage. |
| Windows Keyring | `windows_key_ring_storage.rs` | Uses Windows Credential Manager for secure key storage. |
| Windows DPAPI | `windows_tsm_storage.rs` | Uses DPAPI to encrypt and store keys securely on disk. |
| Cloud (AWS KMS) | `amazon_s3_storage.rs` | Secure cloud-based key storage with AWS KMS. |

Nautilus Key Storage ensures that cryptographic keys are securely managed across different environments, offering a pluggable and extensible solution for developers.


For a detailed technical overview, refer to our **Code Documentation**:

<a href="/docs/attachments/nautilus-key-storage-code-documentation.pdf" download type="application/pdf">📄 Download Nautilus Secure-Storage Code Documentation (PDF)</a>
