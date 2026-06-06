# Confidentiality

## Overview

Confidentiality is the degree to which a system ensures that data is accessible only to those authorised to access it. Confidentiality failures expose data to unauthorised parties, whether through direct access, interception, or inference.

---

## Core concepts

### Data classification

Not all data requires the same level of confidentiality protection. A classification scheme defines protection requirements by data type:

| Class | Examples | Typical controls |
|---|---|---|
| Public | Marketing content, public documentation | No special controls |
| Internal | Business processes, non-sensitive operational data | Access controls, audit logging |
| Confidential | Personal data (PII), financial records, health data | Encryption at rest and in transit, strict access control, audit logging |
| Restricted | Credentials, cryptographic keys, regulated data | Secrets management, hardware security modules, minimal access |

### Encryption

Encryption is a primary technical control for confidentiality:
- **In transit**: TLS for all network communication, including internal service-to-service traffic
- **At rest**: encrypted storage for sensitive data; field-level encryption for the most sensitive fields
- **Key management**: encryption is only as strong as key management, so keys must be rotated, protected, and revocable

### Access control

Confidentiality requires enforcing the principle of least privilege: each principal (user, service, process) has access only to the data required for its function. Access control must be enforced at the data layer, not just at the API layer.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Encryption at rest | Protects against storage-level compromise | Key management complexity; performance overhead |
| Field-level encryption | Limits exposure even if the database is compromised | Complex query patterns; key rotation affects all encrypted records |
| Zero-trust network model | No implicit trust between services | Authentication overhead on every call; latency impact |
| Data minimisation | Less data collected means less to protect | May reduce functionality or analytics capability |

---

## Common pitfalls

- **Internal traffic not encrypted**: traffic between services inside a private network is not inherently secure. TLS must be enforced for all traffic carrying sensitive data.
- **Broad access grants**: service accounts and database users with access to more data than they need create a large blast radius for any compromise.
- **Keys stored alongside encrypted data**: encryption where the key is in the same store as the data provides minimal protection against a database compromise.
