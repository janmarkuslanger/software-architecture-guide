# Functional Completeness

## Overview

Functional completeness is the degree to which the set of functions covers all specified tasks and user objectives. A system lacks functional completeness when users cannot accomplish their intended goals because required functionality is absent or only partially implemented.

Completeness failures are often invisible during development and surface at acceptance testing or after go-live, when users encounter missing paths or unhandled cases.

---

## Core concepts

### Specified vs. implied needs

Completeness applies to both explicitly documented requirements and to needs that are reasonably implied by context. A payment system that processes successful transactions but provides no refund mechanism is functionally incomplete, even if refunds were never explicitly written into a requirement.

### Completeness vs. scope creep

Completeness does not mean implementing everything. It means implementing everything within the agreed scope. Undocumented assumptions on either side (features the team assumed were out of scope that users assumed were included) are the primary source of completeness gaps.

### Acceptance criteria as completeness contracts

Each functional requirement should have testable acceptance criteria. Without them, completeness cannot be verified objectively. "Users can manage their orders" is not verifiable. "Users can view, cancel, and reorder any order placed in the last 12 months" is.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Explicit scope documentation | Completeness is verifiable; gaps are visible early | Overhead of maintaining scope artifacts |
| Iterative delivery with defined MVPs | Partial completeness shipped with explicit communication | Users may encounter missing functionality without warning |
| Feature flags for incomplete paths | Incomplete features can be deployed safely | Incomplete state in production requires discipline to manage |

---

## When to prioritize

- Replacing an existing system where functional parity is a migration requirement.
- Systems used in regulated workflows where missing steps have compliance implications.
- Public-facing launches where user expectation of completeness is high.

## When not to prioritize

- Early exploratory phases where scope is deliberately undefined.
- Internal tooling where missing functionality can be added on request.

---

## Common pitfalls

- **Implicit scope assumptions**: stakeholders assume features that were never documented. Validating scope explicitly with stakeholders before development reduces the risk of gaps surfacing late.
- **Happy-path-only implementation**: the main flow is complete but error paths, edge cases, and administrative functions (e.g., cancellation, correction) are missing.
- **Acceptance criteria written after implementation**: criteria written to match what was built rather than what was needed cannot catch completeness gaps.
