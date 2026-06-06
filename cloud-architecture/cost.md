# Cost

## Overview
In the cloud, cost is an architectural property. Pay-per-use pricing means design decisions — compute model, redundancy level, data placement, traffic flow — translate directly into a recurring bill. Cost is therefore something to design for, not a number to discover at the end of the month.

---

## The cloud cost model

- **Pay-per-use**: you are billed for what you consume — compute time, storage, requests, and data transfer — rather than for owned hardware.
- **Scale to zero**: some models (serverless) cost nothing when idle; others (instances) bill continuously whether used or not.
- **Committed vs on-demand**: reserved or committed capacity is cheaper per unit but trades flexibility for a longer commitment.
- **Egress**: data leaving a region or the provider's network is billed, often more than people expect. Inbound transfer is usually free.
- **Storage tiers**: frequent, infrequent, and archival storage trade retrieval latency against price.

---

## Controlling cost

- **Right-sizing**: match instance and database sizes to actual use; idle over-provisioned capacity is the most common waste.
- **Autoscaling**: add capacity under load and remove it when demand falls, so you pay for the current need.
- **Cost visibility**: tag resources by team, service, and environment so spend can be attributed and questioned. Untagged spend is unmanageable.
- **FinOps**: the practice of making cost a shared engineering responsibility, with visibility and accountability close to the teams that create it.

---

## Trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| Committed capacity | Lower unit price | Reduced flexibility; commitment risk |
| Scale-to-zero compute | No cost when idle | Cold starts; less predictable per-request cost |
| Multi-region redundancy | Higher availability | Duplicated capacity and cross-region transfer cost |
| Premium managed services | Less operational work | Higher direct cost than a self-managed equivalent |

---

## Common pitfalls

- **Unbudgeted egress**: cross-region replication and internet downloads can dominate a bill that was planned around compute alone.
- **Idle over-provisioning**: capacity sized for peak and never scaled down wastes the difference continuously.
- **No cost attribution**: without tagging, no team owns the bill and overspend has no clear cause.
- **Optimising too early**: spending engineering time shaving cost on a system without meaningful traffic can cost more than it saves.

---
