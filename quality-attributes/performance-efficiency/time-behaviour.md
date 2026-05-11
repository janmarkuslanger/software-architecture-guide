# Performance

## Overview

Performance describes how fast a system responds to requests (latency) and how much work it can process concurrently (throughput). It is measurable and load-dependent — a system may perform well under low load and degrade significantly under realistic conditions.

Performance becomes an architectural driver only when a naive implementation cannot meet the defined targets under the expected load profile. Optimizing before a baseline exists wastes design budget and often introduces accidental complexity.

---

## Core concepts

### Latency vs. throughput

Latency and throughput are related but often move in opposite directions.

- **Latency**: time from request to response, measured per request.
- **Throughput**: number of requests processed per unit of time across the system.

Batching is the canonical example of the tension: grouping multiple operations into a single I/O call increases throughput but increases the latency of any individual operation waiting for the batch to fill.

### Percentiles, not averages

Average latency hides the distribution. A p99 of 2s means 1% of users wait at least 2 seconds — which is often where SLA violations occur and where user experience degrades most visibly.

| Metric | What it tells you |
|---|---|
| p50 | Median experience — typical user |
| p95 | Near-tail — most users including slower cases |
| p99 | Tail — outliers that often define perceived reliability |
| p999 | Extreme tail — relevant for high-volume systems |

Always define performance targets using percentiles tied to a load profile: "p99 < 200ms at 1,000 concurrent users."

### Performance budget

A performance budget translates the system-level target into per-component allocations. If end-to-end p99 is 300ms and there are three sequential hops, each hop must be budgeted explicitly — leaving room for network overhead, serialization, and queueing.

---

## Trade-offs

| Lever | Effect | Consequence |
|---|---|---|
| Caching | Reduces latency for repeated reads | Introduces cache invalidation complexity; risk of stale data |
| Async processing | Decouples slow operations from the request path | Increases system complexity; results are not immediately available |
| Read replicas | Distributes read load; reduces latency for reads | Replication lag introduces eventual consistency |
| Connection pooling | Eliminates connection setup cost per request | Pool exhaustion under spike load; sizing requires tuning |
| Horizontal scaling | Increases throughput capacity | Requires stateless design; adds coordination overhead |
| Batching | Increases throughput per I/O | Increases per-request latency; adds buffering logic |

---

## When to prioritize

- The system has a defined SLA with latency or throughput targets.
- Load testing under a realistic profile shows the naive implementation does not meet targets.
- The system is user-facing and latency directly affects experience or conversion.
- The system is a dependency in a synchronous call chain where its latency contributes to the overall budget.

## When not to prioritize

- No baseline measurement exists. Optimize based on data, not assumptions.
- The system is not on a critical path and has no latency-sensitive consumers.
- The team is in an early phase where correctness and delivery speed matter more than efficiency.

---

## Common pitfalls

- **Optimizing without profiling**: performance work without measurement addresses the wrong bottleneck. Identify the actual hot path before changing the architecture.
- **Using average latency**: averages mask tail behavior. Define and monitor percentiles, especially p99.
- **Confusing latency and throughput**: they require different interventions. Reducing latency often means fewer sequential steps; increasing throughput often means parallelism and batching.
- **Caching without an invalidation strategy**: caching is only safe if the invalidation contract is explicit. Silent staleness is a correctness issue, not just a performance detail.
