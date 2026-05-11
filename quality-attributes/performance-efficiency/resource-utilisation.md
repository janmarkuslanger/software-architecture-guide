# Resource Utilisation

## Overview

Resource utilisation is the degree to which the amounts and types of resources used by a system meet requirements. Resources include CPU, memory, disk I/O, network bandwidth, threads, and external service quotas. A system has poor resource utilisation when it uses more resources than necessary to accomplish its function, or when it exhausts resources under load in ways that degrade behaviour.

Resource utilisation matters architecturally because resource costs translate directly to infrastructure costs, and resource exhaustion produces availability and performance failures.

---

## Core concepts

### Resource types

| Resource | Failure mode when exhausted |
|---|---|
| CPU | Latency increases; requests queue; timeouts |
| Memory | Garbage collection pressure; OOM kills; swap thrashing |
| Disk I/O | Write latency spikes; read starvation |
| Network bandwidth | Packet loss; increased latency; connection drops |
| Thread pool / connection pool | Request queuing; timeouts; cascading failures |
| External service quotas | Rate limiting; 429 errors; throttling |

### Utilisation targets

Sustained utilisation above ~70–80% for CPU and memory leaves no headroom for traffic spikes. Systems designed to run at 95% utilisation have no buffer for load variations and are one spike away from failure.

### Efficiency vs. performance

Resource utilisation and time behaviour are related but distinct. A system can be fast (low latency) but wasteful (high CPU per request). At scale, inefficiency becomes a cost and reliability problem even if individual response times are acceptable.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Connection pooling | Reduces per-request resource cost | Pool sizing requires tuning; exhaustion under spikes |
| Lazy loading and on-demand initialisation | Reduces idle resource consumption | Higher latency on first access |
| Horizontal scaling over vertical | More efficient unit economics at scale | Requires stateless design; coordination overhead |
| Resource quotas and limits per container/process | Prevents one process from starving others | Requires capacity planning; misconfigured limits cause premature failures |

---

## When to prioritize

- Systems with significant infrastructure cost where efficiency improvements have measurable financial impact.
- Multi-tenant systems where one tenant's resource usage must not affect others.
- Systems approaching resource ceilings where headroom is insufficient for traffic variability.

## When not to prioritize

- Early-stage systems where correctness and delivery speed matter more than efficiency.
- Systems with stable, low load where resource costs are negligible.

---

## Common pitfalls

- **No resource limits on containers or processes**: an unconstrained process can exhaust host resources and take down co-located services.
- **Designing for average load with no headroom**: systems must handle burst traffic. Sustained utilisation targets should leave buffer.
- **Memory leaks treated as performance issues**: a process that grows in memory over time and requires periodic restarts has a resource utilisation defect, not a performance configuration issue.
- **Ignoring external service quotas**: rate limits on downstream APIs are resource constraints. Systems that do not account for them fail under load in ways that are difficult to diagnose.
