# Testability

## Overview

Testability is the degree to which test criteria can be established for a system or component, and tests can be performed to determine whether those criteria have been met. A testable system makes it possible to verify its behaviour in isolation, under controlled conditions, with fast and reliable feedback.

Testability is a property of the design, not of the test suite. A component that is difficult to test in isolation has a design problem — hidden dependencies, unclear responsibilities, or tightly coupled infrastructure.

---

## Core concepts

### Testability as a design signal

If a component is hard to test, the design needs to change. Common testability problems and their design causes:

| Test difficulty | Design cause |
|---|---|
| Cannot instantiate without starting the whole system | Tight coupling to infrastructure; no dependency injection |
| Tests are slow | Business logic mixed with I/O; no abstraction over external calls |
| Tests are non-deterministic | Shared mutable state; dependencies on time, randomness, or external services |
| Cannot test a specific case | No interface for injecting test conditions; logic buried in private methods |

### Test seams

A test seam is a point in the design where the behaviour of the system can be changed for testing without modifying the code under test. Dependency injection is the primary mechanism: the component declares its dependencies as interfaces; tests inject test doubles.

Without seams, tests must exercise the real implementation of every dependency — making tests slow, fragile, and coupled to external systems.

### Test pyramid

| Level | Scope | Speed | Feedback |
|---|---|---|---|
| Unit | Single component in isolation | Fast (milliseconds) | Immediate; pinpoints the defect |
| Integration | Component interactions | Medium (seconds) | Confirms integration contracts |
| End-to-end | Full system | Slow (minutes) | Confirms user-visible behaviour |

A testable architecture supports all three levels. Over-reliance on end-to-end tests indicates poor unit and integration testability.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Dependency injection | Dependencies are injectable; behaviour is controllable in tests | More boilerplate; wiring complexity |
| Hexagonal architecture | Domain logic is isolated from I/O; testable without infrastructure | Additional adapter layer |
| Pure functions for business logic | Deterministic; no test setup required | Requires separating logic from state management |
| Test containers for integration tests | Real infrastructure in tests; high confidence | Slower than mocks; requires Docker in CI |

---

## Common pitfalls

- **Static method calls and global state**: static dependencies and singletons cannot be replaced in tests. They make every test that exercises the affected code path depend on the real implementation.
- **Business logic in controllers or handlers**: logic that lives in HTTP handlers, message consumers, or database triggers can only be tested through the full integration path. Extract logic into plain objects.
- **Tests that start the whole application**: if every test requires a full application context, the test suite is slow and fragile. Components must be testable in isolation.
- **No tests for the hard cases**: the most important tests cover error conditions, boundary values, and concurrent scenarios. If these are not testable, the design is deficient.
