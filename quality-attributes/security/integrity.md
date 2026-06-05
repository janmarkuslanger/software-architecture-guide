# Integrity

## Overview

Integrity is the degree to which a system prevents unauthorised access to, or modification of, computer programs or data. It covers both deliberate tampering (malicious modification) and accidental corruption (storage errors, partial writes, transmission errors).

A system with high integrity ensures that data and software match their expected state — and that any modification is authorised, complete, and detectable.

---

## Core concepts

### Data integrity

Data integrity ensures that stored and transmitted data is accurate and unmodified. Mechanisms:
- **Checksums and hash verification**: detect accidental or malicious modification in transit or at rest
- **Database constraints**: primary keys, foreign keys, and check constraints prevent logically inconsistent states
- **Transactional writes**: atomic operations prevent partial writes that leave data in an inconsistent state

### Code integrity

Code integrity ensures that deployed software is what the organisation intended to deploy. Mechanisms:
- **Signed artefacts**: build artefacts signed at the CI level and signature verified at deployment
- **Immutable container images**: images are not modified after build; changes require a new build and deployment
- **Supply chain integrity**: dependencies verified against known-good hashes; software bill of materials (SBOM)

### Input integrity

Data entering the system from external sources (users, APIs, messages) must be validated before processing. Unvalidated input is both an integrity risk (corrupting internal state) and a security risk (injection attacks).

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Checksums on stored data | Detects silent corruption | Storage and compute overhead; does not prevent corruption, only detects it |
| Signed deployments | Verifies code provenance | Requires key management; CI/CD pipeline integration |
| Strict database constraints | Enforces invariants at the storage layer | Schema changes require careful migration |
| Immutable infrastructure | Prevents configuration drift; enforces known state | Requires robust deployment automation |

---

## Common pitfalls

- **No input validation**: data written to the database is whatever the client sent. Validation must occur before persistence.
- **Partial write states**: long write operations without transactions leave the system in an inconsistent state if they fail midway. Use atomic transactions or compensating operations.
- **Unverified dependencies**: a dependency that is silently replaced with a malicious version compromises code integrity. Pin dependency versions and verify hashes.
