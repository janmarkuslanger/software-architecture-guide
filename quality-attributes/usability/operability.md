# Operability

## Overview

Operability is the degree to which a system has attributes that make it easy to operate and control. For end-user systems, this means users can direct the system effectively toward their goals. For developer-facing systems and infrastructure, operability means the system can be observed, configured, started, stopped, and maintained without excessive effort.

---

## Core concepts

### User-facing operability

At the user interface level, operability concerns: clear feedback about system state, predictable and reversible actions, consistent controls, and the ability to interrupt or undo operations in progress. Users should be able to understand what the system is doing and intervene when needed.

### Operational operability (infrastructure perspective)

For deployed systems, operability means:
- **Observability**: the system's internal state is visible through logs, metrics, and traces
- **Configuration without redeployment**: operational parameters (timeouts, feature flags, rate limits) can be adjusted without rebuilding or restarting
- **Graceful shutdown**: the system can be stopped cleanly without data loss or incomplete transactions
- **Health endpoints**: the system exposes its own health status for infrastructure tooling

### Operator as user

Operability treats system operators (SREs, DevOps engineers) as users with specific goals: deploy reliably, diagnose problems quickly, respond to incidents, and maintain configuration. Poor operability increases MTTR and incident frequency.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Structured logging | Machine-parseable; queryable at scale | More verbose; higher storage cost |
| Externalized configuration | Operational changes without redeployment | Configuration sprawl; requires a configuration management strategy |
| Health and readiness endpoints | Enables automation and load balancer integration | Requires accurate health logic: false positives cause unnecessary restarts |
| Admin API for runtime control | Operators can adjust behaviour without deployment | Security surface; requires authentication and authorisation |

---

## Common pitfalls

- **No operational visibility**: a system that runs but cannot be observed is difficult to operate safely. Logs, metrics, and health endpoints are generally considered essential.
- **Configuration baked into the binary**: parameters that require redeployment to change (timeouts, feature flags, rate limits) slow operational response to problems.
- **No graceful shutdown**: a service that terminates abruptly drops in-flight requests and may leave data in an inconsistent state. Graceful shutdown is widely regarded as a basic operational requirement.
