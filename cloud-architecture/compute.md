# Compute

## Overview
The compute model determines how a workload runs and, with it, how much you control versus how much the platform manages. It is one of the most consequential early decisions in cloud architecture, because it shapes scaling behaviour, cost structure, and operational burden.

---

## Compute models

| Model | You manage | Platform manages | Common fit |
|---|---|---|---|
| **Virtual machines** | OS, runtime, scaling | Hardware, hypervisor | Full control, legacy workloads |
| **Containers** | Application, configuration | OS (partial), scheduling | Consistent environments, microservices |
| **Serverless** | Business logic only | Everything else | Event-driven, unpredictable traffic |
| **Managed services** | Configuration only | Compute, scaling, patching | Databases, queues, caches |

No single model is correct. Most systems use a mix: containers for long-running services, serverless for event processing, managed services for data.

### Virtual machines
Full control over the operating system and runtime; the platform manages only the physical hardware and hypervisor. Suited to workloads that need specific OS configuration or that were not designed for the cloud. The cost is operational: patching, scaling, and capacity planning remain the team's responsibility.

### Containers
Package an application with its dependencies into a portable, reproducible unit. An orchestrator (e.g. Kubernetes) schedules containers across hosts, restarts failed ones, and scales replicas. Containers give consistent environments from local development to production, at the cost of operating the orchestration layer.

### Serverless
The platform runs business logic in response to events and manages everything else: provisioning, scaling, and capacity. Billing follows actual use and can scale to zero. The trade-offs are cold starts on infrequently used functions, execution limits, and tighter coupling to platform-specific triggers.

### Managed services
The platform operates a capability (a database, queue, or cache) and the team only configures it. This removes most operational work in exchange for less control and a dependency on the provider's implementation.

---

## Scaling

Compute scales in two directions: **vertically** (a larger instance) and **horizontally** (more instances). Horizontal scaling depends on workloads being [stateless](../system-design/scalability.md#statelessness); state held inside an instance prevents that instance from being replaced or replicated freely. Serverless and container platforms scale horizontally by default; virtual machines require explicit autoscaling configuration.

---

## Trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| Serverless compute | No infrastructure management, scales to zero | Cold starts, execution limits, platform coupling |
| Container orchestration | Portability, fine-grained scaling | Operating the orchestration layer requires expertise |
| Virtual machines | Full control over the environment | Patching, scaling, and capacity are the team's burden |
| Managed services | Minimal operational work | Less control, dependency on the provider |

---

## Common pitfalls

- **Serverless for steady high-throughput workloads**: scale-to-zero and per-invocation billing favour spiky or unpredictable traffic. For constant load, containers or instances are often cheaper and avoid cold starts.
- **Kubernetes without the expertise to operate it**: an orchestrator adds significant operational surface. A managed container platform or serverless can deliver the same result with less risk.
- **Lift-and-shift to oversized virtual machines**: moving a workload onto a VM without revisiting its sizing inherits old capacity assumptions and wastes budget.

---
