# Infrastructure as Code

## Overview
Infrastructure as Code (IaC) defines infrastructure — networks, compute, databases, permissions — as versioned, machine-readable files rather than manual console actions. The same files provision every environment, so infrastructure becomes repeatable, reviewable, and auditable.

---

## Declarative vs imperative

| Approach | You specify | Example tools |
|---|---|---|
| **Declarative** | The desired end state; the tool computes the steps | Terraform, CloudFormation |
| **Imperative** | The sequence of steps to reach the state | Scripts, provisioning SDKs |

Declarative IaC is the common default: it converges the real infrastructure toward the described state and can detect **drift** — manual changes that diverge from the definition.

---

## Core concepts

- **Desired state and plan/apply**: the tool compares the definition against reality, shows a plan of changes, then applies them.
- **Idempotency**: applying the same definition repeatedly yields the same result; re-running is safe.
- **Immutability**: replace infrastructure rather than modifying it in place, so deployed state always matches a known definition.
- **State management**: declarative tools track what they manage; this state must be stored securely and locked against concurrent changes.
- **Modules**: reusable, parameterised units keep large definitions consistent and avoid duplication.

---

## Trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| Infrastructure as code | Repeatable, auditable, reviewable deployments | Upfront investment in tooling and discipline |
| Immutable infrastructure | Eliminates drift and configuration surprises | Requires robust deployment automation |
| Shared modules | Consistency across environments and teams | A versioning and maintenance commitment |

---

## Common pitfalls

- **Manual changes alongside IaC**: editing infrastructure by hand causes drift; the definition no longer matches reality and the next apply may revert or break it.
- **Secrets in definitions**: credentials committed into IaC files leak into version control. Reference a secrets manager instead.
- **Unprotected state**: shared state without locking lets concurrent applies corrupt it.
- **One monolithic stack**: a single definition for the entire system makes every change high-risk. Split by lifecycle and ownership.

---
