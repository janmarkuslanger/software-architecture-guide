# Replaceability

## Overview

Replaceability is the degree to which a system or component can be replaced by another product for the same purpose in the same environment. A replaceable system has well-defined interfaces and externally accessible data, allowing migration to a different implementation without requiring changes across all consumers.

Replaceability is relevant both at the system level (replacing one service with another) and at the component level (replacing a library, database, or infrastructure service).

---

## Core concepts

### Interface contracts as replaceability enablers

A system is replaceable to the degree that its interface is well-defined and its consumers depend only on that interface — not on implementation details. If consumers use only the documented API, a replacement that honours the same contract can substitute the original without requiring consumer changes.

Tight coupling to implementation details (database schemas, internal endpoints, binary wire formats, undocumented behaviours) makes replacement difficult even when the functional contract is preserved.

### Data migration

System replacement almost always involves data migration. Replaceability requires that:
- Data can be exported from the current system in a standard format
- The new system can import that data
- There is a migration path that does not require extended downtime

Systems with opaque or proprietary data formats are difficult to replace regardless of interface design.

### The Strangler Fig pattern

When replacing a large system, complete replacement in a single step is high-risk. The Strangler Fig pattern incrementally replaces functionality: a façade routes requests to the new system for replaced functionality and to the old system for functionality not yet replaced. This limits blast radius and allows incremental validation.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Standard data export formats | Reduces migration effort; consumers can extract data independently | May expose internal model in ways that constrain future evolution |
| Façade or anti-corruption layer | Consumers are decoupled from the replaced system | Additional indirection; façade maintenance cost |
| Versioned, stable public APIs | Consumers can migrate to a replacement at their own pace | Old versions must be supported for the migration window |

---

## When to prioritize

- Systems that may be replaced due to vendor lock-in risk, cost, or capability gaps.
- Systems where regulatory requirements mandate data portability.
- Shared infrastructure components with many consumers, where replacement affects a large surface area.

## Common pitfalls

- **No data export capability**: a system whose data cannot be extracted makes replacement effectively impossible. Data portability must be designed in, not added at migration time.
- **Consumers depending on implementation details**: consumers that bypass the documented API and call internal endpoints, or read directly from the database, cannot migrate to a replacement without changes.
- **Big-bang replacement**: replacing a system in a single step is high-risk. Incremental replacement with a façade reduces risk and allows validation at each step.
