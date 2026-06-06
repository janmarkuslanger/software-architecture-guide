# System Design Core Topics

## Overview

System design concerns the decisions that shape how a system behaves under real load and failure: how it exposes its boundary, stores its state, stays consistent across copies, moves data between its parts, and grows with demand. These concerns cut across architecture styles — a layered monolith and a microservices system both decide how to store data, when to cache, and what consistency a read guarantees.

The pages below treat each concern on its own, but the decisions are linked: a choice in one — replicating data for scale, say — constrains the options in another, such as the consistency a read can offer.

## Topics

| Topic | Concern |
|---|---|
| [API Design](api-design.md) | The contract between a system and its consumers — versioning, error models, pagination, idempotency |
| [API Styles](api-styles.md) | The model and protocol a system exposes — REST, GraphQL, gRPC, WebSocket, SSE |
| [Data Architecture](data-architecture.md) | How persistent state is stored and structured — relational vs non-relational, schema evolution, indexing, replication |
| [Consistency Models](consistency-models.md) | What a read guarantees when data is replicated or accessed concurrently — strong, causal, eventual; CAP and PACELC |
| [CQRS and Event Sourcing](cqrs-event-sourcing.md) | Separating read and write models, and storing state as a sequence of events |
| [Messaging](messaging.md) | Asynchronous transport between components — queues vs streams, ordering, delivery guarantees |
| [Caching](caching.md) | Copies kept closer to use for faster reads — strategies, expiry, invalidation, stampede |
| [Scalability](scalability.md) | Handling increased load by adding resources — vertical and horizontal scaling, statelessness, sharding |
| [Distributed Transactions](distributed-transactions.md) | Consistency across service and database boundaries — 2PC, sagas, outbox, compensation |
| [Resilience](resilience.md) | Containing dependency failures so they do not cascade — timeout, retry, circuit breaker, bulkhead, backpressure |

## How these topics connect

- **Copies of data raise consistency questions.** Replication ([Data Architecture](data-architecture.md#replication)), caching ([Caching](caching.md)), and sagas ([Distributed Transactions](distributed-transactions.md#saga)) each hold more than one copy of the same data, which is why [Consistency Models](consistency-models.md) cuts across all of them.
- **Statelessness moves the bottleneck to data.** Keeping the service tier stateless ([Scalability](scalability.md#statelessness)) concentrates state in the data tier, so scaling compute shifts the limit onto [Data Architecture](data-architecture.md).
- **Splitting data ownership forces distributed coordination.** Once data is partitioned across services or shards, an operation that spans them relies on [Messaging](messaging.md) and [Distributed Transactions](distributed-transactions.md) instead of a single local transaction.
- **Idempotency recurs at every boundary.** The same duplicate-collapsing problem appears at the [API boundary](api-design.md#idempotency), in [message delivery](messaging.md#delivery-guarantees), in the outbox relay, and wherever a [retry](resilience.md#timeouts-and-retries) re-attempts a failed call: a repeated request may carry the same intent more than once, and the receiver collapses the duplicates.
- **Style and contract are separate API decisions.** Which model and protocol a system exposes ([API Styles](api-styles.md)) is decided independently of the contract concerns — versioning, errors, pagination, idempotency — that apply to any style ([API Design](api-design.md)). Where a call needs no immediate response, [Messaging](messaging.md) is an alternative to a request/response style altogether.
- **Event logs are a source for read models.** [Event Sourcing](cqrs-event-sourcing.md) stores state as an append-only log; projecting that log builds the read side that [CQRS](cqrs-event-sourcing.md) serves queries from — the same projection mechanism [Event-Driven Architecture](../architecture-patterns/event-driven.md#patterns-commonly-used-with-eda) uses. Because the read model trails the log, the two are linked by [eventual consistency](consistency-models.md).
- **Scaling and resilience approach load from two sides.** Rate limiting and backpressure ([Resilience](resilience.md)) bound the load a tier accepts, while horizontal scaling ([Scalability](scalability.md)) adds capacity to serve it — one limits demand, the other increases supply.
