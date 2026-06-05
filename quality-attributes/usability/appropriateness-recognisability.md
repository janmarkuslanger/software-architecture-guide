# Appropriateness Recognisability

## Overview

Appropriateness recognisability is the degree to which users can recognise whether a system or component is appropriate for their needs. Users should be able to determine — before investing significant time — whether the system will help them accomplish their goal.

Recognisability failures cause high abandonment rates, misuse, and support load from users who engaged with the wrong tool for their task.

---

## Core concepts

### First contact evaluation

Recognisability is evaluated at first contact: the landing page, the API documentation overview, the CLI help text, or the first screen. Users make fit decisions quickly. If the system's purpose, audience, and capabilities are not communicated clearly at this point, they cannot evaluate appropriateness.

### Signals of appropriateness

Users assess appropriateness through: explicit purpose statements, visible examples of typical use cases, scope boundaries (what the system does *not* do), and trust signals (credentials, user base, reliability indicators). All of these are architectural decisions about what to surface at the entry point.

### Developer-facing recognisability

For APIs and SDKs, recognisability means developers can determine from documentation, naming, and examples whether the interface solves their problem — before writing any code. An API that requires deep exploration to understand its scope has poor recognisability.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Clear scope statement at entry point | Users self-select correctly; reduces misuse | Narrow framing may exclude adjacent use cases |
| Prominent examples for common use cases | Immediate recognisability for the target audience | Examples that don't match the user's scenario reduce confidence |
| Explicit non-goals documentation | Prevents misuse; manages expectations | Requires discipline to keep current as the system evolves |

---

## Common pitfalls

- **Generic naming**: a system called "DataProcessor" or "ServiceManager" communicates little about its purpose. Names and entry-point descriptions tend to be more effective when they are specific.
- **No visible scope boundaries**: users only discover the system cannot meet their need after investing significant time. Communicate limitations early.
- **Internal terminology at entry points**: using system-internal concepts in the first description forces users to learn the system's model before they can evaluate fit.
