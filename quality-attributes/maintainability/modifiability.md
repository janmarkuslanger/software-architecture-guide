# Modifiability

## Overview

Modifiability is the degree to which a system can be effectively and efficiently modified without introducing defects or degrading existing product quality. A highly modifiable system allows developers to make changes confidently, with predictable effort and low risk of regression.

Modifiability tends to follow from structural decisions such as low coupling, high cohesion, clear dependencies, and comprehensive automated tests. It can degrade when shortcuts accumulate over time.

---

## Core concepts

### Cost of change

The cost of a change is determined by:
1. How many places in the codebase must be modified
2. How risky each modification is (how likely to introduce a defect)
3. How long it takes to verify the change is correct

A modifiable system minimises all three. A system with poor modifiability requires many modifications for each change, each carrying significant regression risk, with a slow feedback loop.

### Open/Closed Principle

A system is modifiable to the extent that it can be extended without being modified. New behaviour should be addable through extension points (new implementations of interfaces, new event handlers, new plugins) rather than through modification of existing code. Each modification to existing code risks introducing defects.

### Dependency direction

Dependencies that point in the wrong direction make the system hard to modify. In a layered architecture, domain logic that depends on infrastructure details (database schemas, HTTP frameworks) is coupled to those details — a change in the infrastructure requires changes in the domain. Inverting these dependencies (the domain defines interfaces; infrastructure implements them) isolates change.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Dependency inversion | Domain and infrastructure change independently | More interfaces; more indirection |
| Feature flags | New behaviour can be enabled/disabled without redeployment | Flag management overhead; flags that are never cleaned up become technical debt |
| Event-driven decoupling | Publishers and subscribers change independently | Increased complexity; harder to trace causality |
| Comprehensive test suite | Regressions are caught; refactoring is safe | Tests require maintenance; poorly designed tests resist change |

---

## Common pitfalls

- **Magic constants and hardcoded values**: values embedded in code rather than configuration require code changes for what should be operational adjustments. Hardcoded values can become a modifiability concern when they need to change.
- **Tests that test implementation rather than behaviour**: tests tied to internal structure break when the structure changes, precisely when tests should be providing safety. Test behaviour and outcomes, not implementation details.
- **No automated tests for changed code**: modifying code without a test suite makes every change a risk. Test coverage is the prerequisite for safe modifiability.
- **Accumulating workarounds instead of fixes**: each workaround increases the complexity of the affected area and makes future modifications harder. Technical debt compounds.
