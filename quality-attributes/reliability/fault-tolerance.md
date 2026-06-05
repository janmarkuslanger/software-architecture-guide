# Reliability

## Overview

Reliability describes whether a system produces correct results, even in the presence of faults. It is independent of availability: a system can be highly available (always reachable) while being unreliable (returning wrong or inconsistent results).

In distributed systems, some degree of failure is inevitable. Reliability engineering is therefore not about preventing all faults — it is about building systems that tolerate faults without producing incorrect behavior.

---

## Core concepts

### Fault → Error → Failure chain

| Term | Definition | Example |
|---|---|---|
| **Fault** | A latent defect in the system | A bug in payment processing logic that is never triggered |
| **Error** | A fault that has been activated | The buggy code path executes during a specific transaction |
| **Failure** | An observable deviation from correct behavior | The user is charged twice |

Understanding this chain matters for strategy: fault avoidance tries to prevent faults from existing; fault tolerance accepts that faults will activate and designs the system to contain the resulting errors before they become failures.

### Fault avoidance vs. fault tolerance

| Strategy | Mechanism | Limitation |
|---|---|---|
| Fault avoidance | Testing, code review, type systems, input validation | Cannot prevent all faults; especially insufficient for distributed failure modes |
| Fault tolerance | Idempotency, checksums, transactions, redundancy with consistency guarantees | Adds complexity; requires explicit design |

In distributed systems, fault tolerance is often a primary strategy. Networks partition, processes crash, messages arrive out of order or are delivered more than once. These are operational realities that testing alone cannot eliminate.

Fault avoidance remains important for correctness at the business logic level, but is generally not sufficient on its own.

### Idempotency

An operation is idempotent if executing it multiple times produces the same result as executing it once. Idempotency is critical in any system with at-least-once delivery semantics — which includes most message queues, retry mechanisms, and distributed RPC frameworks.

Without idempotency, a retried message can cause duplicate side effects: double charges, duplicate records, repeated notifications.

Idempotency requires a stable deduplication key — an identifier that is consistent across retries and unique per logical operation.

---

## Trade-offs

| Strategy | What it protects against | Consequence |
|---|---|---|
| Idempotency + deduplication key | Duplicate message delivery and retries | Requires storage for seen keys; key design is non-trivial |
| Checksums and data integrity checks | Silent data corruption | Compute overhead; does not recover corrupted data, only detects it |
| Transactionality | Partial writes leaving inconsistent state | Locking overhead; limited to a single transactional resource in most implementations |
| Redundancy with consistency guarantees | Single-node failures causing incorrect state | Requires consensus protocol; adds latency to writes |
| Circuit breaker | Cascading failures from a degraded dependency | Requires fallback logic or graceful degradation; adds complexity |

---

## Reliability vs. availability

This distinction is frequently misunderstood and worth stating explicitly:

- **Availability** answers: "Is the system reachable?"
- **Reliability** answers: "Does the system produce correct results?"

A load-balanced cluster with two nodes — one returning correct results, one returning corrupted data — can have 100% availability with 50% reliability. Redundancy does not fix correctness issues; it may replicate them.

Both properties are typically addressed independently and with separate architectural decisions.

---

## When to prioritize

- The system processes financial transactions, medical data, or any domain where incorrect results have high-severity consequences.
- The system consumes messages from a queue or event stream with at-least-once delivery guarantees.
- The system coordinates writes across multiple services or storage systems.

## When not to prioritize

- Read-only systems where no state is modified and incorrect output is detectable and retryable by the caller.
- Systems where eventual consistency and occasional duplicates are explicitly acceptable to stakeholders.

---

## Common pitfalls

- **No idempotency for message consumers**: any consumer that processes messages from a queue without handling duplicates may cause incorrect behavior when messages are redelivered — which is expected under normal operating conditions in at-least-once delivery systems.
- **No stable deduplication key**: idempotency requires a key that survives retries. Using a timestamp or generated UUID per attempt defeats the purpose.
- **Fault avoidance as the only strategy in distributed systems**: comprehensive test coverage does not address network partitions, message redelivery, or partial failures across service boundaries.
- **Conflating reliability with availability**: adding more replicas increases availability; it does not increase reliability if the fault being replicated is a logic error or data corruption.
