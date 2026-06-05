# Reusability

## Overview

Reusability is the degree to which an asset can be used in more than one system, or in building other assets. A reusable component solves a problem once and can be applied across multiple contexts without modification.

Reusability reduces duplication and the associated cost of maintaining the same logic in multiple places. However, premature generalisation in pursuit of reusability adds complexity without delivering value.

---

## Core concepts

### Reuse vs. duplication

Not all duplication is a problem. The cost of duplication (maintaining the same logic twice) should be weighed against the cost of the abstraction that would eliminate it. Three instances of similar logic are a signal to consider abstraction. A single instance is generally not sufficient justification to abstract.

A poorly fitting abstraction — a reusable component that does not quite fit any of its use cases — can be more costly than the duplication it was meant to remove.

### What makes a component reusable

A reusable component:
- Has a well-defined, stable interface
- Depends only on what it needs — no unnecessary dependencies that callers must also acquire
- Has a focused responsibility — a component that solves a general problem is often more reusable than one that solves a specific problem
- Is documented — callers can understand its contract without reading its implementation

### Library design vs. application design

Reusable components (libraries, shared modules) have different design constraints than application code. Libraries must have stable interfaces, clear versioning, and minimal dependencies. Application code that is never reused can be less stable and more specific. Treating all code as a library produces over-engineered applications; treating all code as application code produces duplicated logic.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Shared library for common logic | Single source of truth; changes propagate to all consumers | Shared dependency creates coupling; breaking changes require coordinated updates |
| Copy-paste with domain-specific adaptation | Each copy is independently evolvable | Fixes must be applied in multiple places |
| Generic, parameterised components | High reuse potential | Complexity grows with generality; harder to understand and use correctly |
| Versioned interfaces for shared components | Consumers upgrade on their own schedule | Old versions must be maintained; deprecation requires active management |

---

## Common pitfalls

- **Reusability pursued before the abstraction is understood**: generalising from one use case can produce a poorly fitting abstraction. Consider waiting for at least two or three concrete use cases before abstracting.
- **Shared components with too many dependencies**: a "reusable" utility that requires 10 transitive dependencies is rarely reused because the cost of adoption is too high.
- **No ownership of shared components**: components shared across teams without a clear owner degrade over time as each team makes local patches rather than contributing to the shared version.
