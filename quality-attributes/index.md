# Quality Attributes

Quality attributes, also called non-functional requirements, describe how a system behaves, not what it does. They are often primary drivers of architectural decisions: the same feature set can be implemented with fundamentally different architectures depending on the quality targets.

---

## What are quality attributes?

A quality attribute is a measurable property of a system's runtime behavior, structure, or development process. Unlike functional requirements, they apply across the entire system and cannot be addressed by a single component in isolation.

**Measurability matters.** A quality attribute target is most useful when it is concrete and verifiable:

- "The system must be fast" is not a requirement.
- "p99 latency < 300ms under 500 concurrent users" is a requirement.

Without a measurable target, there is no basis for architectural decisions and no way to verify whether the architecture delivers what was needed.

---

## Quality attributes covered in this guide

The attributes below are treated as independent architectural concerns. They align with ISO 25010 but are not structured to mirror its hierarchy. Some ISO sub-characteristics (availability, scalability) are elevated to standalone entries because they drive distinct architectural decisions.

| ISO 25010 Characteristic | Sub-characteristics |
|---|---|
| [Functional Suitability](functional-suitability/index.md) | Functional Completeness · Functional Correctness · Functional Appropriateness |
| [Performance Efficiency](performance-efficiency/index.md) | Time Behaviour · Resource Utilisation · Capacity |
| [Compatibility](compatibility/index.md) | Co-existence · Interoperability |
| [Usability](usability/index.md) | Appropriateness Recognisability · Learnability · Operability · User Error Protection · User Interface Aesthetics · Accessibility |
| [Reliability](reliability/index.md) | Maturity · Availability · Fault Tolerance · Recoverability |
| [Security](security/index.md) | Confidentiality · Integrity · Non-repudiation · Accountability · Authenticity |
| [Maintainability](maintainability/index.md) | Modularity · Reusability · Analysability · Modifiability · Testability |
| [Portability](portability/index.md) | Adaptability · Installability · Replaceability |

---

## Relationships and tensions between attributes

Quality attributes are not independent. Architectural decisions that improve one often affect others.

**Performance vs. Scalability**
A system optimized for single-instance performance (e.g., heavy in-process caching, local state) may not scale horizontally. Scaling adds synchronization points (distributed locks, cache invalidation, coordination overhead) which increase latency and reduce throughput under certain patterns.

**Scalability vs. Availability**
Statelessness is a prerequisite for horizontal scaling and simultaneously simplifies availability: stateless instances can be replaced or multiplied without session continuity concerns. Both attributes benefit from the same architectural decision.

**Availability vs. Reliability**
These are orthogonal. A system can have high availability (always reachable) but low reliability (returns wrong results). Redundancy increases availability by reducing downtime; it does not prevent a faulty component from being replicated. Reliability requires correctness guarantees, not just uptime.

**Security vs. Usability**
Security controls add friction. Authentication steps, session timeouts, and access restrictions reduce convenience. The appropriate balance is context-dependent: a banking application and a public content site have different thresholds.

**Maintainability vs. Performance**
Abstractions, layering, and modularity that improve maintainability can introduce overhead. Highly optimized code is often tightly coupled to its execution context and difficult to change. Performance optimizations should be introduced after measurable need is established, not speculatively.

**Portability vs. Functional Suitability**
Abstracting over provider-specific features to preserve portability sometimes means forgoing capabilities that would directly improve functional suitability (managed search, geospatial queries, stream processing). This trade-off is worth making explicitly.

---

## Quality attributes are requirements, not optimization targets

Quality attributes are sometimes treated as things to maximize after the system is built; they are better understood as constraints to be established before architectural decisions are made.

If the target is unknown, any architecture is defensible, and none can be evaluated. Defining targets early, making them measurable, and using them to drive trade-off decisions explicitly supports more grounded architectural choices.
