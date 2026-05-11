# Installability

## Overview

Installability is the degree to which a system can be successfully installed or uninstalled in a specified environment. A system with high installability can be deployed to its target environment reliably, reproducibly, and with minimal manual intervention — and can be removed cleanly without leaving residual state.

---

## Core concepts

### Reproducible deployments

Installability requires that a deployment produces a consistent, known state regardless of how many times it is executed or what the prior state of the environment was. An installation that works the first time but fails on reinstall, or that behaves differently depending on what was installed before, has poor installability.

Mechanisms for reproducible deployments:
- **Idempotent installation scripts**: running the script multiple times produces the same result
- **Immutable artefacts**: container images and build artefacts do not change after build; deployment always installs a known version
- **Infrastructure as Code**: environment state is declared and enforced, not accumulated through manual steps

### Installation prerequisites

Installation dependencies (runtime versions, OS packages, network access, credentials) must be documented and verifiable before installation begins. A deployment that fails midway because of a missing prerequisite is a reliability and installability failure.

### Clean uninstall

A system with high installability can be removed completely — no orphaned processes, no residual files, no database entries that prevent reinstallation, no configuration that persists across uninstall/reinstall cycles.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Containerisation | Consistent environment; installation is image pull + run | Container runtime must be available; image distribution requires registry |
| Infrastructure as Code | Reproducible; version-controlled; auditable | Requires IaC tooling and expertise |
| Package managers with pinned versions | Deterministic dependency resolution | Requires active maintenance of pinned versions |
| Zero-downtime deployment (blue/green, rolling) | Installation without service interruption | More complex deployment infrastructure |

---

## When to prioritize

- Systems deployed to multiple environments or customer sites.
- Systems with complex dependency chains where manual installation is error-prone.
- Systems with high deployment frequency where installation reliability directly affects delivery velocity.

## Common pitfalls

- **Manual installation steps not documented**: steps that live only in the installer's head are not reproducible. Every non-automated step is a reliability risk.
- **Installation state stored in the installer's local environment**: configuration, credentials, or state that exists only on the machine where installation was performed makes the system difficult to reinstall or migrate.
- **No rollback path**: a failed partial installation leaves the environment in an unknown state. Rollback must be possible and tested.
