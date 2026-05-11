# Scalability

## Overview

Scalability describes whether a system's performance characteristics remain stable as load increases. It is not the same as performance: a system can be fast at low load but fail to maintain that performance as users or data volume grows.

A scalable system can handle more work by adding resources — without requiring fundamental redesign.

---

## Core concepts

### Vertical vs. horizontal scaling

| Dimension | Vertical scaling | Horizontal scaling |
|---|---|---|
| Mechanism | Larger machines (more CPU, RAM) | More machines of the same size |
| Limit | Physical ceiling of available hardware | Theoretically unbounded |
| Complexity | Simple — no application changes needed | Requires stateless design and load distribution |
| Cost | Expensive at the high end | More predictable unit economics |

Vertical scaling is a valid short-term lever, but it has a hard ceiling. Horizontal scaling is the sustainable path for systems with growing load — and it requires the application to be designed for it from the start.

### Statelessness

Statelessness is the prerequisite for horizontal scaling. A stateless instance holds no request-specific state between calls: no local session data, no in-memory user context, no local filesystem state that another instance cannot access.

When state is required, it must be externalized to a shared store (database, distributed cache, object storage). This makes every instance interchangeable and allows the load balancer to route requests to any available instance.

### Elasticity

Elasticity is automatic, reactive scaling: the system adds or removes instances based on observed load, without manual intervention. It is a capability built on top of horizontal scaling and requires infrastructure support (autoscaling groups, container orchestration).

Elasticity is distinct from scalability. A system is scalable if it *can* handle more load with more resources. It is elastic if it *automatically* acquires and releases those resources.

### Bottleneck hierarchy

When a system fails to scale, the bottleneck is typically one of the following, in order of how often they are encountered:

1. **Compute** — CPU or memory saturation on a single instance. Solved by horizontal scaling.
2. **Shared mutable state** — a distributed lock, a shared counter, or a serialized queue. Solved by partitioning or redesigning the data model.
3. **Synchronous call chains** — a request must wait for N sequential downstream calls. Solved by async patterns or parallelism.
4. **Database** — connection limits, single-writer bottleneck, or query performance at scale. The most common and most costly bottleneck to address late.

Identifying the bottleneck before choosing a scaling strategy prevents applying the wrong lever.

---

## Trade-offs

| Strategy | Use case | Consequence |
|---|---|---|
| Read replicas | Read-heavy workloads with a single write primary | Replication lag; read-after-write consistency requires care |
| Connection pooling | High request concurrency hitting a relational database | Pool size becomes a tuning parameter; exhaustion under spikes |
| Caching | Repeated reads of slow or expensive data | Invalidation complexity; potential for stale reads |
| Sharding | Write-heavy workloads exceeding single-node database capacity | Cross-shard queries are expensive; resharding is operationally complex |
| CQRS | Read and write models have different scaling needs | Two models to maintain; eventual consistency between them |

---

## When to prioritize

- Load projections show the current architecture will reach a ceiling within the planning horizon.
- The system has variable or spiky load where elasticity provides cost or reliability benefits.
- The team is designing a new system where horizontal scalability is a stated requirement.

## When not to prioritize

- Load is low and stable. Scalability work on a system with 10 users is premature optimization.
- The team does not yet understand the actual load profile. Design for the current reality; add scalability when the bottleneck is known.

---

## Common pitfalls

- **Local state in horizontally scaled instances**: session data stored in memory on a single instance breaks when the load balancer routes the next request elsewhere. Externalize all shared state.
- **Early sharding**: sharding before a database becomes the actual bottleneck introduces significant operational complexity with no benefit. Use it as a last resort for write scalability.
- **Ignoring the database as a bottleneck**: compute scales easily; databases often do not. A system designed with no database scaling strategy will hit a wall that is expensive to fix retroactively.
- **Elasticity without a stateless design**: autoscaling adds instances, but if instances hold local state, new instances cannot serve requests that were in progress elsewhere.
