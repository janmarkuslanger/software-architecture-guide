# Analysability

## Overview

Analysability is the degree to which it is possible to assess the impact of an intended change on one or more parts of a system, to diagnose deficiencies or causes of failures, or to identify parts to be modified. A highly analysable system makes it possible to understand what is happening, why something is failing, and what will be affected by a change.

Analysability directly reduces MTTR for incidents and reduces the cost and risk of every change.

---

## Core concepts

### Observability

A system is observable to the degree that its internal state can be inferred from its external outputs. Observability has three primary signals:

| Signal | Purpose |
|---|---|
| **Logs** | Discrete events with context; used for debugging and audit |
| **Metrics** | Aggregated measurements over time; used for alerting and trending |
| **Traces** | Correlated request paths across services; used for latency analysis and dependency mapping |

A system that emits all three signals can be analysed effectively in production without requiring code changes or redeployment to investigate an issue.

### Impact analysis

Before making a change, a developer must be able to assess what the change will affect. This requires:
- Module boundaries that make dependencies explicit
- Dependency graphs that can be navigated without deep code reading
- Test coverage that provides confidence about what breaks when a change is made

Systems with hidden dependencies, implicit coupling, or tangled module graphs make impact analysis difficult and expensive — leading to regressions and conservative "safe" changes that never clean up technical debt.

### Static analysis

Static analysis tools (linters, dependency analysers, complexity metrics) provide automated analysability: they surface structural issues, identify high-risk areas, and enforce rules that keep the codebase analysable as it grows.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Structured logging with correlation IDs | Requests can be traced across components from a single ID | Higher log verbosity; storage cost |
| Distributed tracing | End-to-end request visibility across services | Instrumentation overhead; requires tracing infrastructure |
| Explicit dependency injection | Dependencies are visible at the call site | More boilerplate |
| Architecture fitness functions | Structural rules are enforced automatically | Requires investment in tooling |

---

## Common pitfalls

- **Logs without context**: log entries that say "error occurred" without the request ID, user, or relevant state are useless for incident analysis. Every log entry must carry enough context to be actionable in isolation.
- **No correlation IDs**: a request that crosses three services with no shared identifier cannot be traced end-to-end without correlating by timestamp, which is unreliable at scale.
- **High cyclomatic complexity**: functions with many branches are hard to analyse for impact. Complexity metrics should be tracked and thresholds enforced.
- **Implicit coupling**: if a change to module A unexpectedly breaks module B, and the dependency between them was not visible, analysability has failed. Dependencies must be explicit.
