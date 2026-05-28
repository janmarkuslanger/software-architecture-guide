# Accountability

## Overview

Accountability is the degree to which the actions of an entity can be traced uniquely to that entity. A system with high accountability makes it possible to determine who performed what action, when, and on which resource — for any action of significance.

Accountability is a foundation for security incident response, compliance auditing, and detection of misuse. Without it, breaches and policy violations cannot be investigated effectively.

---

## Core concepts

### Identity binding

Accountability requires that every significant action is bound to an authenticated identity — not just a session, IP address, or service name. This means:
- Human actions are attributed to individual user accounts, not shared credentials
- Service-to-service calls are attributed to specific service identities (not a single shared API key)
- Actions taken on behalf of a user (by a background job or service) retain the original user identity in the audit trail

### What to log

Not all events need to be logged for accountability. Audit-relevant events include:
- Authentication: successful logins, failed attempts, MFA events, session creation and termination
- Authorisation: access grants, denials, permission changes
- Data access: reads of sensitive data (not just writes)
- Mutations: create, update, delete operations on significant resources
- Administrative actions: configuration changes, user management, privilege escalation

### Log attributes

Each accountability log entry must capture: authenticated identity, action performed, resource affected, timestamp, outcome (success/failure), and correlation ID linking to related events.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Centralised audit log pipeline | Consistent format; single query surface | Single point of failure for audit data; requires high availability |
| Logging reads of sensitive data | Full accountability for data access | High log volume; storage cost; performance impact for read-heavy systems |
| Immutable log storage | Tamper-evident; modification is detectable | Cannot correct erroneous log entries |
| Service-level identities (not shared keys) | Fine-grained attribution per service | More complex credential management |

---

## Common pitfalls

- **Shared credentials**: a shared API key used by multiple services means accountability is at the service group level, not the individual service. Actions cannot be attributed precisely.
- **Logging only mutations, not reads**: access to sensitive data without modification leaves no trace. For regulated data (PII, financial records), read access must be logged.
- **No correlation IDs**: without a correlation ID linking an API request to downstream service calls and database writes, accountability is fragmented across disconnected log entries.
- **Logs purged too aggressively**: regulatory requirements and incident investigation timelines may require log retention of months or years. Define retention policy before building the logging pipeline.
