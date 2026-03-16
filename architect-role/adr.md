# Architecture Decision Records (ADRs)

## Overview
An Architecture Decision Record (ADR) is a short document that captures a significant architectural decision, the context that led to it, the alternatives considered, and the consequences of the choice.

ADRs make the reasoning behind decisions visible and prevent repeated debates about settled questions.

## When to write an ADR
Write an ADR when a decision:
- Is hard to reverse or costly to change later.
- Affects multiple teams or system boundaries.
- Involves meaningful trade-offs between alternatives.
- Is likely to be questioned or revisited in the future.

Not every decision needs an ADR. Trivial or easily reversible choices do not warrant one.

## Structure of an ADR

### Title
Short, descriptive, and numbered for easy reference.
Example: `ADR-004: Use PostgreSQL as the primary data store`

### Status
One of: `Proposed` | `Accepted` | `Deprecated` | `Superseded by ADR-XXX`

### Context
Describe the situation and forces at play: business goals, technical constraints, team capabilities, or non-functional requirements that made a decision necessary.

### Decision
State what was decided, clearly and directly.

### Alternatives considered
List the options that were evaluated and briefly explain why they were not chosen.

### Consequences
Describe the expected outcomes — both positive and negative. Include risks, trade-offs, and follow-up actions if applicable.

## Example

```
# ADR-012: Use an event bus for inter-service communication

## Status
Accepted

## Context
Multiple services need to react to state changes in the order domain.
Direct HTTP calls would create tight coupling and complicate failure handling.

## Decision
Use an event bus (Kafka) for asynchronous communication between services.

## Alternatives considered
- Direct REST calls: simpler but creates tight coupling and cascading failures.
- Database polling: avoids a broker but is inefficient and hard to scale.

## Consequences
+ Services are decoupled and can evolve independently.
+ Consumers can be added without changing the producer.
- Introduces operational complexity (Kafka cluster, schema management).
- Debugging requires distributed tracing tooling.
```

## Storage and tooling
- Store ADRs as markdown files in the repository, close to the code they affect.
- Common convention: `docs/decisions/ADR-NNN-title.md`
- Tools like [adr-tools](https://github.com/npryce/adr-tools) can automate file creation.

## Decision considerations / trade-offs
- ADRs add upfront writing effort but reduce long-term confusion and re-discussion.
- Storing ADRs in the repo keeps them versioned; a wiki is more discoverable but can drift.
- Teams that skip ADRs often repeat the same architectural debates months or years later.
