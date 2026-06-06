# Functional Correctness

## Overview

Functional correctness is the degree to which a system produces the right results with the required degree of precision. A system is functionally correct when it faithfully implements the intended domain logic, not just when it avoids crashes or returns a response.

Correctness failures are often silent: the system runs, but the output is wrong. They can be more costly than availability failures because incorrect data propagates and may require manual correction.

---

## Core concepts

### Correctness vs. reliability

These are distinct properties. Functional correctness asks: does the system implement the right logic? Reliability asks: does it produce correct results under fault conditions? A system with correct logic can still fail reliably under a network partition. A system that is always available can consistently return wrong results.

### Domain logic as the source of truth

Correctness is determined by the domain model. Business rules (calculation formulas, state transition constraints, validation conditions) should be represented explicitly in code and should match the agreed specification. Ambiguous specifications tend to produce incorrect implementations regardless of code quality.

### Precision

Correctness includes precision: a tax calculation rounded to the wrong number of decimal places, a timestamp stored in the wrong timezone, or a currency conversion using an outdated rate are correctness failures even if the computation completes without error.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Domain model explicitly encoding business rules | Rules are testable and auditable | Requires upfront domain modeling investment |
| Property-based testing | Verifies correctness across a large input space | Higher test authoring complexity |
| Formal specification for critical calculations | Eliminates ambiguity as a source of incorrectness | Only practical for well-bounded, high-stakes logic |
| Contract tests between services | Verifies correctness of integration output | Requires coordination across teams |

---

## When to prioritize

- Financial calculations, tax logic, legal computations: any domain where incorrect output has direct monetary or legal consequences.
- Systems where downstream consumers depend on the correctness of the output.
- Regulated domains with audit requirements.

## When not to prioritize (relative to other attributes)

- Exploratory tools or dashboards where approximate results are acceptable and explicitly communicated.

---

## Common pitfalls

- **Business logic distributed across layers**: rules split between the domain layer, the API handler, and the database trigger tend to be inconsistent and untestable as a unit. Centralising domain logic reduces this risk.
- **Specification ambiguity accepted as normal**: deferring ambiguity to implementation tends to produce systems that are internally consistent but externally incorrect. Resolving ambiguity before implementation reduces this risk.
- **Testing only the happy path**: correctness tests should cover boundary conditions, negative cases, and precision edge cases, not just the common scenario.
