# Interoperability

## Overview

Interoperability is the degree to which two or more systems can exchange information and use the information that has been exchanged. It requires agreement on data formats, protocols, semantics, and error handling — not just technical connectivity.

Systems can be technically connected (messages are transmitted) but still lack interoperability if the receiving system cannot correctly interpret or act on the exchanged data.

---

## Core concepts

### Levels of interoperability

| Level | What it requires |
|---|---|
| Transport | Shared protocol (HTTP, gRPC, AMQP) and connectivity |
| Syntactic | Shared data format (JSON, XML, Protobuf, Avro) |
| Semantic | Shared meaning of fields, values, and identifiers |
| Behavioural | Shared understanding of sequence, state transitions, and error handling |

Most integration problems occur at the semantic and behavioural levels, not the transport level.

### Interface contracts

Interoperability depends on explicit contracts that define the format, semantics, and versioning of exchanged data. Implicit contracts — undocumented but relied-upon behaviours — are a frequent source of integration failures during evolution.

Contract formats: OpenAPI for REST, Protobuf/gRPC for binary RPC, Avro/JSON Schema for event streams, AsyncAPI for message-based systems.

### Versioning and evolution

Interoperability needs to be maintained as systems evolve independently. Breaking changes — removed fields, changed types, reordered enum values, altered error codes — break consumers without notice unless versioning is explicit and backward compatibility is a design constraint.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Contract-first design | Interoperability is verifiable before integration | Upfront contract design cost; requires tooling |
| Additive-only schema evolution | Zero migration cost for existing consumers | Schema grows over time; deprecated fields remain |
| Consumer-driven contract tests | Catches breaking changes before deployment | Requires cross-team coordination |
| Tolerant reader pattern | Consumers are robust to new fields from producers | Consumers may silently ignore data they should process |
| Semantic versioning with deprecation windows | Breaking changes are predictable and communicated | Requires enforcement and sunset management |

---

## When to prioritize

- The system exchanges data with external consumers or providers it does not control.
- Multiple teams integrate against the same interface on independent release cycles.
- The integration landscape includes regulated data exchanges with formal interface agreements.

## When not to prioritize

- Fully internal integrations within a single team and codebase, where coordinated changes are low-cost.

---

## Common pitfalls

- **Undocumented implicit contracts**: consumers build on observed behaviour that was never specified. The producer changes it; consumers break. Interfaces with external consumers generally benefit from an explicit contract.
- **Semantic mismatch**: field names match but meanings diverge — a `status` field that means different things to producer and consumer. Shared vocabulary and canonical data models prevent this.
- **No backward compatibility policy**: teams evolve their APIs without defining what constitutes a breaking change. Consumers discover breakage at runtime.
