# Non-repudiation

## Overview

Non-repudiation is the degree to which actions or events can be proven to have taken place, such that the events or actions cannot be repudiated later. It ensures that a party that performed an action cannot later credibly deny having performed it.

Non-repudiation is architecturally relevant in any system where the origin or occurrence of actions must be provable — financial transactions, legal document signing, audit-sensitive operations, and regulated workflows.

---

## Core concepts

### Technical mechanisms

| Mechanism | What it proves |
|---|---|
| Digital signatures | A specific key holder produced or approved a document or message |
| Timestamping (trusted timestamp authority) | An event occurred at a specific point in time |
| Audit logs with cryptographic chaining | A sequence of events occurred in a specific order and has not been modified |
| Multi-factor authentication records | A specific human identity was authenticated at a specific time |

### Audit logs as non-repudiation evidence

Audit logs that record who performed what action and when are the primary non-repudiation mechanism for most systems. For logs to serve as non-repudiation evidence, they must be:
- **Tamper-evident**: modification of log entries must be detectable (cryptographic chaining, append-only stores)
- **Complete**: all relevant actions must be logged, including failed attempts
- **Attributable**: each log entry must be linked to an authenticated identity, not just a session token

### Non-repudiation vs. accountability

These are related but distinct. Accountability means actions can be traced to an identity. Non-repudiation means the identity cannot deny the action. Accountability requires attribution; non-repudiation requires proof that the attributed party cannot credibly contest.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Append-only audit log store | Tamper-evident; modification is detectable | Requires separate storage with different access controls |
| Digital signatures on critical operations | Cryptographic proof of authorship | Key management complexity; signing infrastructure |
| Trusted timestamping | Proves when an event occurred | Requires a trusted external authority |

---

## When to prioritize

- Financial transactions, contracts, and regulatory filings where legal disputes may require proof of action.
- Audit-sensitive systems in regulated industries (healthcare, finance, legal).
- Systems where high-value operations are performed and disputes about whether they occurred are foreseeable.

## Common pitfalls

- **Audit logs stored in the same system they audit**: a compromised system can modify its own audit log. Logs must be written to a separate, protected store.
- **Session-level attribution without identity verification**: logging a session ID provides traceability but not non-repudiation if the session could have been accessed by multiple individuals.
