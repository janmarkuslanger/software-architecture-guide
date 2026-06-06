# Modularity

## Overview

Modularity is the degree to which a system is composed of discrete components such that a change in one component has minimal impact on other components. A modular system can be understood, changed, tested, and deployed in parts, which tends to reduce the cost and risk of individual changes.

Modularity in the ISO 25010 maintainability sense is directly addressed by the foundational concepts in this guide: see [Modularity](../../foundations/modularity.md) for cohesion, coupling, and connascence.

---

## Core concepts

### Module boundaries

A module boundary is the contract between two components. The boundary defines what is visible (the interface) and what is hidden (the implementation). Strong module boundaries allow either side to change independently, as long as the contract is preserved.

The cost of a change is influenced by how many module boundaries it crosses. A change contained within a single module tends to have lower cost and risk. A change that requires modifications to interfaces between modules typically has higher cost and risk of regression.

### Cohesion and coupling

The two primary levers for modularity:
- **High cohesion**: each module has one clear responsibility. A module that does many unrelated things tends to change for many unrelated reasons.
- **Low coupling**: modules depend on each other minimally and only through explicit interfaces. A highly coupled module cannot change without changing its dependencies.

See [Modularity](../../foundations/modularity.md) for the full taxonomy of cohesion types, coupling types, and connascence.

### Package and service boundaries

Modularity applies at multiple levels: within a service (packages, classes), between services (APIs, events), and between deployment units (shared libraries, shared databases). The same principles apply at each level, but violations at higher levels (e.g., shared databases between services) are more costly to fix.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Small, focused modules | High cohesion; changes are contained | More interfaces to manage; potential for over-fragmentation |
| Explicit public/private module interfaces | Changes to internals do not affect consumers | Requires discipline to maintain interface stability |
| Package-by-feature rather than package-by-layer | Related code changes together; low coupling between features | Cross-feature dependencies require deliberate design |

---

## Common pitfalls

- **Modules that change for multiple unrelated reasons**: a violation of the Single Responsibility Principle. The module's responsibility is not well-defined.
- **Implicit dependencies through shared global state**: modules that share a global variable or singleton are coupled even if they do not import each other directly.
- **Module boundaries that mirror the org chart, not the domain**: Conway's Law produces modules that reflect team structure. Team structure and domain structure should be aligned.
