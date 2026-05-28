# System Design Core Topics

## Overview
System design is about balancing correctness, performance, and operability under real constraints. These topics shape most architecture decisions.

## Core concepts
- API design: clear contracts, error models, pagination, versioning, idempotency.
- Data architecture: SQL vs NoSQL, schema evolution, indexing, replication.
- Consistency models: strong, eventual, and causal consistency.
- Messaging: queues vs streams, ordering, delivery guarantees, dead-letter queues.
- Caching: cache-aside, write-through, TTLs, invalidation, stampede protection.
- Scalability: stateless services, horizontal scaling, sharding, load balancing.
- Distributed transactions: 2PC, sagas, outbox, compensating actions.
