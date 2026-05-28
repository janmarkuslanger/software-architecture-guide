# Availability

## Overview

Availability is the fraction of time a system is operational and reachable by its users. It is typically expressed as a service level agreement (SLA) and has direct business consequences: downtime means users cannot complete their goals, which translates into lost revenue, broken trust, or contractual penalties.

Availability is a system-level property. It is affected by every component in the request path — a chain of components is only as available as its least available link.

---

## Core concepts

### Availability formulas

```
Availability = MTBF / (MTBF + MTTR)
Availability = Uptime / (Uptime + Downtime)
```

- **MTBF** (Mean Time Between Failures): average time between incidents. Higher is better.
- **MTTR** (Mean Time To Recovery): average time to restore service after an incident. Lower is better.

### The "nines"

| SLA | Downtime per year | Downtime per month |
|---|---|---|
| 99% | ~3.65 days | ~7.2 hours |
| 99.9% | ~8.76 hours | ~43.8 minutes |
| 99.99% | ~52.6 minutes | ~4.4 minutes |
| 99.999% | ~5.3 minutes | ~26 seconds |

Each additional nine is an order-of-magnitude harder to achieve. Moving from 99.9% to 99.99% requires a fundamentally different operational posture, not just better hardware.

### Two levers: MTBF and MTTR

Most availability strategies target one of two levers:

**Increasing MTBF** — reduce how often failures occur:
- Redundant hardware and network paths
- Rigorous testing and gradual rollouts
- Higher-quality components

**Reducing MTTR** — reduce how long recovery takes:
- Monitoring and alerting with low detection latency
- Automated failover
- Runbooks and incident response procedures
- Feature flags for rapid rollback

**MTTR is often the more effective lever.** Failures cannot be eliminated in complex distributed systems — hardware fails, networks partition, deployments go wrong. Recovery, however, can be automated and made predictable. A system that recovers in 30 seconds consistently is more available than one that fails rarely but takes 2 hours to restore.

---

## Trade-offs

### Redundancy strategies

| Strategy | MTTR | Complexity | Notes |
|---|---|---|---|
| Active-Passive | Failover time (~10–60 seconds) | Lower | Standby instance is idle; switchover requires detection and promotion |
| Active-Active | Near zero (no failover needed) | Higher | All instances serve traffic; requires stateless design and consistency management |

### Availability vs. other attributes

Increasing availability through redundancy adds operational complexity and cost. Active-active setups require careful consistency management — decisions about what happens when two active nodes disagree.

Availability targets above 99.99% typically require eliminating all single points of failure, including the load balancer, DNS, and the monitoring system itself.

---

## When to prioritize

- The system serves users or downstream systems that depend on it continuously.
- Downtime has measurable financial or contractual consequences.
- The system is part of a critical path with an SLA commitment.

## When not to prioritize

- Internal tooling or batch systems where short outages are acceptable.
- Development or staging environments.
- Early-stage systems where correctness and delivery speed outweigh uptime guarantees.

---

## Common pitfalls

- **Redundancy placed at the wrong layer**: adding a second application server provides no benefit if both share a single database with no failover capability. Identify and eliminate actual single points of failure.
- **No automated failover**: manual failover under pressure is slow and error-prone. Automation reduces MTTR from minutes or hours to seconds.
- **No runbooks**: even with alerts, teams without documented recovery procedures take longer to restore service. Runbooks reduce MTTR and lower the skill threshold required during incidents.
- **Defining SLA after the architecture is built**: availability targets determine which redundancy patterns are necessary. A 99.9% target and a 99.999% target require fundamentally different designs. Define the target first.
