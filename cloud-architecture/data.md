# Data

## Overview
Data is where state lives and who is allowed to reach it. Of the five pillars it is the hardest to change: compute can be replaced and networks reconfigured, but moving or restructuring live data is slow and risky. Data decisions therefore deserve the most deliberation early.

---

## Storage types

| Type | Holds | Typical use |
|---|---|---|
| **Managed database** | Structured, queryable state | Transactional data, relational or document models |
| **Object storage** | Large unstructured blobs | Files, backups, media, data lakes |
| **Cache** | Hot, ephemeral copies | Reducing read latency and database load |

How state is structured, evolved, and indexed is covered in [Data Architecture](../system-design/data-architecture.md); caching strategies and invalidation in [Caching](../system-design/caching.md).

---

## Data residency and sovereignty

Where data physically lives is a legal and architectural constraint, not only a latency concern. Regulations may require that certain data stay within a jurisdiction. Residency requirements influence region selection, replication topology, and which managed services are usable — decide them before choosing products.

---

## Replication and consistency

Cloud data services replicate data for durability and availability. Replication introduces more than one copy of the same data, which raises the question of what a read guarantees — see [Consistency Models](../system-design/consistency-models.md). Backups and recovery targets are part of [Reliability](reliability.md).

---

## Trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| Managed database | No patching, backups, or failover to operate | Less control; dependency on the provider |
| Object storage for large blobs | Cheap, durable, scales without planning | Higher latency than block or in-memory storage |
| Proprietary data service | Powerful features, low operational cost | Migration away is expensive (lock-in) |
| In-region replication | Higher durability and availability | Storage cost and write latency increase |

---

## Common pitfalls

- **State in a single zone**: data confined to one availability zone shares that zone's failure. Durability requires replication across zones (see [Reliability](reliability.md)).
- **Residency overlooked**: discovering a jurisdiction requirement after choosing a region forces a costly migration.
- **Lock-in through proprietary formats**: data stored in a provider-specific service is the most expensive thing to migrate later. Weigh the feature benefit against exit cost.
- **No verified backups**: a backup that has never been restored is an assumption, not a recovery plan.

---
