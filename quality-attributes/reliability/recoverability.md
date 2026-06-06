# Recoverability

## Overview

Recoverability is the degree to which a system can recover data and re-establish its desired state in the case of an interruption or failure. It covers the restoration of lost state, the correction of corrupted data, and the resumption of processing after an outage.

Availability (MTTR) measures how quickly a system returns to service. Recoverability addresses whether the system returns to the *correct* state, not just that it is reachable again.

---

## Core concepts

### Recoverability vs. availability

A system can recover to an available state (reachable, responding) without being in a correct state (consistent data, no lost transactions). Recoverability ensures that after a failure, the system's state accurately reflects what happened before the failure, with no lost writes, no corrupted records, and no inconsistent views across components.

### Recovery Point Objective (RPO) and Recovery Time Objective (RTO)

| Metric | Definition | Architectural implication |
|---|---|---|
| **RPO** | Maximum acceptable data loss (how far back can we go?) | Determines backup frequency and replication lag tolerance |
| **RTO** | Maximum acceptable time to restore service | Determines failover automation and standby infrastructure requirements |

RPO and RTO are generally defined before choosing a recovery strategy. A 0 RPO (no data loss) requires synchronous replication. A 24h RPO tolerates daily backups.

### Recovery mechanisms

| Mechanism | RPO | RTO | Complexity |
|---|---|---|---|
| Periodic backup + restore | Hours to days | Hours | Low |
| Continuous replication to standby | Seconds to minutes | Minutes | Medium |
| Synchronous replication (active-active) | ~0 | ~0 | High |
| Write-ahead log (WAL) replay | Seconds | Minutes | Medium |

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Synchronous replication | Near-zero RPO | Latency overhead on every write; replication partner becomes a performance dependency |
| Point-in-time recovery (WAL/binlog) | Fine-grained recovery to any moment | Requires log retention and replay infrastructure |
| Event sourcing | Full history; state can be rebuilt from events | High storage and replay complexity |
| Idempotent reprocessing | Safe to re-run failed operations | Requires stable deduplication keys throughout the pipeline |

---

## When to prioritize

- Systems where data loss has direct financial, legal, or user-trust consequences.
- Systems with explicit RPO/RTO SLAs (e.g., regulated environments, paid tiers).
- Systems that process irreversible operations (financial transactions, sent communications).

## When not to prioritize

- Systems with fully reproducible data (caches, derived views) where recovery means re-computation rather than restoration.
- Development and staging environments.

---

## Common pitfalls

- **Untested recovery procedures**: backup and restore processes that are never exercised tend to fail at the worst moment. Regular testing of recovery procedures reduces this risk.
- **RPO/RTO undefined until after an incident**: without defined targets, recovery architecture lacks a concrete basis. Defining targets before designing the system allows recovery mechanisms to be matched to actual requirements.
- **Conflating availability with recoverability**: a system that comes back online after a failure but with 2 hours of lost transactions has poor recoverability, regardless of how fast it recovered availability.
- **No data integrity checks post-recovery**: returning to service without verifying state consistency can propagate corruption silently.
