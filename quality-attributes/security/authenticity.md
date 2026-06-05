# Authenticity

## Overview

Authenticity is the degree to which the identity of a subject or resource can be proved to be the one claimed. It covers the verification of user identities (authentication), the verification of service identities (mutual TLS, service tokens), and the verification of data origin (signed payloads, certificate chains).

Without authenticity, a system cannot make access decisions or trust the data it receives.

---

## Core concepts

### Authentication factors

| Factor type | Example | Strength |
|---|---|---|
| Something you know | Password, PIN | Low — can be guessed, stolen, or phished |
| Something you have | TOTP token, hardware key, SMS code | Medium-High — requires physical access or SIM swap |
| Something you are | Biometric | High — difficult to replicate; privacy implications |

Multi-factor authentication (MFA) combines factors. Systems handling sensitive data or privileged access commonly require at minimum two factors.

### Service authenticity

In distributed systems, services must authenticate to each other — not just users to services. Mechanisms:
- **Mutual TLS (mTLS)**: both parties present certificates; identity is verified on both sides
- **Short-lived service tokens (JWT, SPIFFE/SPIRE)**: services obtain tokens with a bounded lifetime; compromise has limited blast radius
- **API keys**: simpler but long-lived; rotation is operationally complex

### Data authenticity

Data authenticity verifies that a message or document originates from the claimed source and has not been modified. Mechanisms: digital signatures, HMAC, certificate chains (for TLS certificates, package signing).

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Centralised identity provider (OIDC/OAuth2) | Single source of truth for identities; consistent auth across services | Central dependency; availability impact if IdP is down |
| Short-lived tokens | Limited blast radius for stolen tokens | Requires refresh infrastructure; more frequent auth round-trips |
| mTLS between services | Strong service-to-service authenticity | Certificate management overhead; rotation complexity |
| Passwordless authentication | Eliminates phishing of passwords | Requires device/hardware factor; recovery flows are complex |

---

## Common pitfalls

- **Long-lived credentials without rotation**: API keys, service passwords, and certificates with multi-year lifespans create a large window of exposure if compromised. Define and enforce rotation schedules.
- **Authentication at the gateway only**: downstream services that trust any request from inside the network implicitly trust every compromised or misconfigured upstream service.
- **No MFA on privileged accounts**: administrative accounts with single-factor authentication represent a frequently exploited entry point for account compromise. MFA is strongly recommended for privileged access.
- **Self-signed certificates in production**: self-signed certificates prevent verification of authenticity. Use certificates from a trusted certificate authority.
