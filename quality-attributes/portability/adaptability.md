# Adaptability

## Overview

Adaptability is the degree to which a system can effectively and efficiently be adapted for different or evolving hardware, software, or other operational or usage environments. A highly adaptable system can be moved to a new environment, reconfigured for different operational conditions, or adjusted for new platform requirements — with minimal changes to the system itself.

---

## Core concepts

### Environment dimensions

Adaptability covers multiple types of environmental change:

| Dimension | Examples |
|---|---|
| Infrastructure | Cloud provider, on-premises, hybrid; VM to container migration |
| Operating system | Linux distributions, Windows, macOS |
| Runtime | JVM version, Node.js version, Python version |
| Database | PostgreSQL to Aurora, MySQL to Cloud SQL |
| Configuration | Different parameter sets per environment (dev, staging, production) |

### Externalised configuration

The most fundamental adaptability mechanism is externalising all environment-specific configuration: connection strings, URLs, feature flags, timeouts, credentials. Configuration embedded in code or baked into build artefacts cannot be adapted without rebuilding.

The 12-factor app principle of config states: everything that varies between deployments must be stored in the environment, not in the code.

### Abstraction layers

Adaptability is achieved by abstracting over environment-specific services behind interfaces. Code that calls a database through an interface can be adapted to a different database by providing a new implementation — without changing the calling code. Code that calls AWS S3 directly is coupled to S3.

The cost of the abstraction must be weighed against the probability and cost of adaptation. Abstracting over a service that will never change is wasted complexity.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Externalised configuration | Adapts to different environments without rebuild | Configuration sprawl; secrets management required |
| Infrastructure-agnostic interfaces | Swap providers without changing application code | Abstractions leak when provider features differ fundamentally |
| Containerisation | Consistent runtime regardless of host | Container-level adaptability; underlying infrastructure may still vary |
| Feature detection over version checks | Adapts to capability differences without hardcoded version logic | More complex logic |

---

## Common pitfalls

- **Provider-specific SDK calls throughout the application**: calls to AWS, Azure, or GCP SDKs scattered across business logic cannot be adapted without touching every call site. Centralise infrastructure access.
- **Environment-specific logic in application code**: `if (env == "production") { ... }` blocks work against adaptability. Environment differences are generally better expressed in configuration than in code.
- **No environment parity**: significant differences between development and production environments mean the system is adapted for production but tested in a different context. Reduce environment differences as much as practical.
