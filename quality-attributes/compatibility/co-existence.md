# Co-existence

## Overview

Co-existence is the degree to which a system can perform its required functions efficiently while sharing an environment and resources with other systems, without detrimental impact on any other system.

Co-existence failures manifest as interference between systems that occupy the same environment: port conflicts, resource exhaustion caused by one system affecting another, dependency version conflicts, or shared configuration that one system modifies and another depends on.

---

## Core concepts

### Shared environment types

| Environment | Co-existence risks |
|---|---|
| Same host / VM | CPU and memory contention; port conflicts; filesystem collisions |
| Same container network | Service name collisions; DNS conflicts; subnet exhaustion |
| Same dependency ecosystem | Library version conflicts (dependency hell); shared global state |
| Same database | Schema ownership conflicts; connection pool contention; lock contention |
| Same message broker | Topic or queue name collisions; consumer group interference |

### Isolation as the primary mechanism

Co-existence is achieved through isolation: the degree to which a system's resource consumption, configuration, and side effects are bounded and do not leak into the shared environment. Containerisation, namespaces, and resource quotas are the primary technical mechanisms.

### Co-existence vs. interoperability

Co-existence concerns whether systems can run alongside each other without interfering. Interoperability concerns whether they can exchange information correctly. A system can co-exist without interoperating, and vice versa.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Containerisation | Strong process and filesystem isolation | Container-level isolation does not protect shared infrastructure (databases, brokers) |
| Resource quotas per service | Prevents one service from starving others | Requires capacity planning; misconfigured quotas cause premature failures |
| Separate database schemas or instances per service | Eliminates schema ownership conflicts | Higher operational overhead; cross-schema queries require coordination |
| Namespaced configuration | Configuration changes are scoped to the owning service | Requires discipline in configuration key naming |

---

## When to prioritize

- Multiple services deployed to the same infrastructure without strong isolation boundaries.
- Shared databases or brokers where multiple services consume the same resources.
- Environments with mixed deployment cadences where one service's deployment must not affect others.

## When not to prioritize

- Systems deployed in fully isolated environments where no shared resources exist.

---

## Common pitfalls

- **Shared mutable configuration**: a service that modifies a shared configuration file or environment variable can break co-located services silently.
- **No resource limits**: a service with unbounded resource consumption will eventually degrade or crash co-located services under load.
- **Shared database with implicit schema ownership**: multiple services writing to the same tables create co-existence conflicts whenever either service needs to change the schema.
