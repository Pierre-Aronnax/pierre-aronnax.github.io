---
title: "Integrating new Backend"
description: ""
summary: ""
date: 2023-09-07T16:12:37+02:00
lastmod: 2023-09-07T16:12:37+02:00
draft: false
weight: 999
toc: true
sidebar:
  collapsed: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  robots: "" # custom robot tags (optional)
---



Nautilus Registry is designed with a storage-agnostic architecture, allowing developers to extend it by implementing custom storage backends. This guide walks through the process of integrating an SQL-based storage backend while keeping compatibility with the existing registry system.

---

### Step 1: Understand the Storage Trait Requirements

All storage backends in Nautilus Registry implement the `RegistryStorage` trait from `registry_traits.rs`. This ensures that any new backend follows a consistent API for storing, retrieving, updating, and deleting records.


Located in `registry_traits.rs`, this trait defines the required methods:

```rust

pub trait RegistryStorage {
    type Error;

    fn store(&mut self, key: &str, value: &str) -> Result<(), Self::Error>;
    fn retrieve(&self, key: &str) -> Result<Option<String>, Self::Error>;
    fn remove(&mut self, key: &str) -> Result<(), Self::Error>;
    fn contains(&self, key: &str) -> Result<bool, Self::Error>;
}

```

---

### Step 2: Create a New SQL Storage Backend

1. Define a new struct to represent the SQL storage.
2. Implement `RegistryStorage` for it, ensuring full compatibility.
3. Use an async-compatible SQL library like `sqlx` for database operations.

---

#### Step 2.1: Add Dependencies for SQL Support

In `Cargo.toml`, add the required dependencies:

```toml

[dependencies]
sqlx = { version = "0.7", features = ["postgres", "runtime-tokio-native-tls"] }
tokio = { version = "1", features = ["full"] }
serde = "1"

```

---

#### Step 2.2: Implement the SQL Registry Backend

Create a new file: `sql_registry.rs`, then define the storage struct:

```rust

use async_trait::async_trait;
use sqlx::{Pool, Postgres};
use crate::registry_traits::RegistryStorage;

pub struct SqlRegistry {
    pool: Pool<Postgres>,
}

impl SqlRegistry {
    pub async fn new(database_url: &str) -> Result<Self, sqlx::Error> {
        let pool = Pool::connect(database_url).await?;
        Ok(Self { pool })
    }
}

```

---

#### Step 2.3: Implement `RegistryStorage` for SQL Registry

Now, implement the required trait methods:

```rust

#[async_trait]
impl RegistryStorage for SqlRegistry {
    type Error = sqlx::Error;

    async fn store(&mut self, key: &str, value: &str) -> Result<(), Self::Error> {
        sqlx::query("INSERT INTO registry (key, value) VALUES ($1, $2) ON CONFLICT (key) DO UPDATE SET value = $2")
            .bind(key)
            .bind(value)
            .execute(&self.pool)
            .await?;
        Ok(())
    }

    async fn retrieve(&self, key: &str) -> Result<Option<String>, Self::Error> {
        let result: Option<(String,)> = sqlx::query_as("SELECT value FROM registry WHERE key = $1")
            .bind(key)
            .fetch_optional(&self.pool)
            .await?;
        Ok(result.map(|(val,)| val))
    }

    async fn remove(&mut self, key: &str) -> Result<(), Self::Error> {
        sqlx::query("DELETE FROM registry WHERE key = $1")
            .bind(key)
            .execute(&self.pool)
            .await?;
        Ok(())
    }

    async fn contains(&self, key: &str) -> Result<bool, Self::Error> {
        let result: Option<(i64,)> = sqlx::query_as("SELECT COUNT(*) FROM registry WHERE key = $1")
            .bind(key)
            .fetch_optional(&self.pool)
            .await?;
        Ok(result.map(|(count,)| count > 0).unwrap_or(false))
    }
}

```

---

### Step 3: Integrate SQL Registry into Nautilus Registry

#### Step 3.1: Modify `mod.rs` to Include the SQL Backend

In `mod.rs`, register the new backend:

```rust
#[cfg(feature = "sql")]
pub mod sql_registry;

```

#### Step 3.2: Enable SQL Registry in `lib.rs`

Modify `lib.rs` to expose the SQL backend conditionally:

```rust
[cfg(feature = "sql")]
pub use sql_registry::SqlRegistry;

```

---

### Step 4: Using the SQL Backend in an Application

#### Step 4.1: Setup a Database Table for the Registry

Before running the registry, create the necessary database table:

```sql
CREATE TABLE registry (
    key TEXT PRIMARY KEY,
    value TEXT NOT NULL
);

```

#### Step 4.2: Initialize the SQL Registry in an App

```rust
#[tokio::main]
async fn main() -> Result<(), sqlx::Error> {
    let database_url = "postgres://user:password@localhost/nautilus_registry";
    let mut registry = SqlRegistry::new(database_url).await?;

    registry.store("example_key", "example_value").await?;
    let value = registry.retrieve("example_key").await?;
    println!("Retrieved: {:?}", value);

    registry.remove("example_key").await?;
    println!("Contains 'example_key': {}", registry.contains("example_key").await?);

    Ok(())
}

```

---

## Conclusion

By following this guide, you can extend Nautilus Registry to support SQL storage, ensuring:

- ✅ Seamless integration with existing Nautilus Registry traits
- ✅ Full compatibility with async Rust environments
- ✅ Scalable, persistent record storage beyond in-memory and Redis
