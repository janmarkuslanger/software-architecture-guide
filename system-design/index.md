# System Design Core Topics

## Overview

System design concerns the decisions that shape how a system behaves under real load and failure: how it exposes its boundary, stores its state, stays consistent across copies, moves data between its parts, and grows with demand. These concerns cut across architecture styles — a layered monolith and a microservices system both decide how to store data, when to cache, and what consistency a read guarantees.

The pages below treat each concern on its own, but the decisions are linked: a choice in one — replicating data for scale, say — constrains the options in another, such as the consistency a read can offer.

## Topics

| Topic | Concern |
|---|---|
| [API Design](api-design.md) | The contract between a system and its consumers — versioning, error models, pagination, idempotency |
| [Data Architecture](data-architecture.md) | How persistent state is stored and structured — relational vs non-relational, schema evolution, indexing, replication |
| [Consistency Models](consistency-models.md) | What a read guarantees when data is replicated or accessed concurrently — strong, causal, eventual; CAP and PACELC |
| [Messaging](messaging.md) | Asynchronous transport between components — queues vs streams, ordering, delivery guarantees |
| [Caching](caching.md) | Copies kept closer to use for faster reads — strategies, expiry, invalidation, stampede |
| [Scalability](scalability.md) | Handling increased load by adding resources — vertical and horizontal scaling, statelessness, sharding |
| [Distributed Transactions](distributed-transactions.md) | Consistency across service and database boundaries — 2PC, sagas, outbox, compensation |

## How these topics connect

- **Copies of data raise consistency questions.** Replication ([Data Architecture](data-architecture.md#replication)), caching ([Caching](caching.md)), and sagas ([Distributed Transactions](distributed-transactions.md#saga)) each hold more than one copy of the same data, which is why [Consistency Models](consistency-models.md) cuts across all of them.
- **Statelessness moves the bottleneck to data.** Keeping the service tier stateless ([Scalability](scalability.md#statelessness)) concentrates state in the data tier, so scaling compute shifts the limit onto [Data Architecture](data-architecture.md).
- **Splitting data ownership forces distributed coordination.** Once data is partitioned across services or shards, an operation that spans them relies on [Messaging](messaging.md) and [Distributed Transactions](distributed-transactions.md) instead of a single local transaction.
- **Idempotency recurs at every boundary.** The same duplicate-collapsing problem appears at the [API boundary](api-design.md#idempotency), in [message delivery](messaging.md#delivery-guarantees), and in the outbox relay: a retry may deliver the same intent more than once, and the receiver discards the duplicates.
