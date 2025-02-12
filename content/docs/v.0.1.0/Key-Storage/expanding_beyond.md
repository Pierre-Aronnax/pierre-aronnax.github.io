---
title: "Integrating new Storage"
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
Nautilus Key Storage is designed with a modular and extensible architecture, enabling developers to add their own storage backends without modifying the core infrastructure. By leveraging Rust traits, new storage solutions can seamlessly integrate into the system while maintaining compatibility with existing backends.

This guide will walk developers through implementing their own storage backend using the core traits provided by Nautilus Key Storage.

---

## Understanding the Trait-Based Approach

Nautilus Key Storage uses Rust traits to define standard storage operations. Any new storage backend must implement the following key traits:

- KeyStorage – Defines the core functionality for storing, retrieving, and managing cryptographic keys.
- KeyStorageError – Ensures a unified error handling mechanism across different storage implementations.
- FileFormat – Provides serialization support for different storage formats like JSON, PEM, and CBOR.

By implementing these traits, new storage backends can be easily plugged into the framework without affecting existing functionality.

---

## Implementing a Custom Storage Backend

#### Step 1: Define Your Storage Structure

First, create a new struct that represents your storage backend.

```rust
pub struct MyCustomStorage {
    storage_path: String,
}

```

#### Step 2: Implement `KeyStorage` for Storage Operations

The `KeyStorage` trait ensures that the new storage backend can initialize, save, load, remove, and list stored keys.

```rust

impl KeyStorage for MyCustomStorage {
    type StoredType = Vec<u8>;
    type Error = String;

    fn initialize(&self, config: Option<&str>) -> Result<(), Self::Error> {
        // Implement initialization logic here
        Ok(())
    }

    fn save(&self, keypair: &Self::StoredType, location: &str, encrypt: bool) -> Result<(), Self::Error> {
        // Implement key saving logic
        Ok(())
    }

    fn load(&self, location: &str, decrypt: bool) -> Result<Self::StoredType, Self::Error> {
        // Implement key loading logic
        Ok(vec![])
    }

    fn remove(&self, location: &str) -> Result<(), Self::Error> {
        // Implement key removal logic
        Ok(())
    }

    fn list(&self) -> Result<Vec<String>, Self::Error> {
        // Implement listing logic
        Ok(vec!["key1".to_string(), "key2".to_string()])
    }
}

```

---

#### Step 3: Implement `KeyStorageError` for Standardized Error Handling

To maintain consistency across different storage implementations, use `KeyStorageError` for handling errors.

```rust

fn save(&self, keypair: &Self::StoredType, location: &str, encrypt: bool) -> Result<(), KeyStorageError> {
    if location.is_empty() {
        return Err(KeyStorageError::SaveError("Invalid storage location".to_string()));
    }
    Ok(())
}

```

---

#### Step 4: Implement `FileFormat` for Serialization Support

If the storage backend requires key serialization, implement the `FileFormat` trait.

```rust

impl FileFormat for MyCustomStorage {
    fn serialize<T: Serialize>(data: &T) -> Result<Vec<u8>, String> {
        // Serialize the key to bytes
        serde_json::to_vec(data).map_err(|e| e.to_string())
    }

    fn deserialize<T: DeserializeOwned>(bytes: &[u8]) -> Result<T, String> {
        // Deserialize the key from bytes
        serde_json::from_slice(bytes).map_err(|e| e.to_string())
    }
}

```

---

## Deploying the Custom Storage Backend

Once the traits are implemented, your new storage backend can now be seamlessly used within the Nautilus Key Storage framework.

### Example Usage

```rust

fn main() {
    // Initialize the storage
    let storage = MyCustomStorage { storage_path: "/tmp".to_string() };
    storage.initialize(None).expect("Initialization failed");

    // Save a key
    let key_data = vec![1, 2, 3, 4];
    storage.save(&key_data, "test_key", false).expect("Save failed");

    // Load the key
    let loaded_key = storage.load("test_key", false).expect("Load failed");

    // Verify the key
    assert_eq!(key_data, loaded_key);
    println!("Key storage successful!");
}

```

---

## Why This Approach Works

- Seamless Integration – The new backend follows the existing Nautilus Key Storage framework without modifying core infrastructure.
- Future-Proof – Developers can add custom storage solutions, such as cloud storage or hardware-backed storage, without breaking compatibility.
- Interoperable – Ensures all key types follow a unified serialization format for easy storage and retrieval.

By following this trait-based blueprint, developers can extend Nautilus Key Storage with new storage solutions while ensuring compatibility, security, and scalability.
