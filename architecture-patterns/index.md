# Architecture Styles & Patterns

## Overview
Architecture styles define system structure, boundaries, and communication. They are not just deployment choices; they shape ownership, data flow, and change cost.

Choosing the right style early reduces long-term coordination cost. The wrong style is expensive to fix — it touches team topology, deployment pipelines, and data ownership.

## Styles covered

| Style | Best for | Key trade-off |
|---|---|---|
| [Layered](layered.md) | Stable domains, small teams | Simple to start, harder to scale across teams |
| [Hexagonal / Ports & Adapters](hexagonal.md) | Systems with many external dependencies | Better isolation, more interfaces to maintain |
| [Modular Monolith](modular-monolith.md) | Growing teams, pre-microservices | Clean boundaries without deployment complexity |
| [Microservices](microservices.md) | Multiple teams, independent scaling | High autonomy, high operational cost |
| [Event-Driven](event-driven.md) | Fan-out, audit trails, async workflows | Great decoupling, harder to trace and debug |


## General trade-offs

- Deployment independence improves autonomy but adds operational overhead.
- Shared databases simplify queries but tightly couple teams and release cycles.
- Synchronous calls are easy to reason about; async messaging improves resilience.
- Eventual consistency reduces coordination but requires explicit reconciliation.
- Service boundaries should align with team boundaries and change frequency.
- Integration risk belongs behind adapters to protect core logic.
