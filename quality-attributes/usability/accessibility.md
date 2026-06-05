# Accessibility

## Overview

Accessibility is the degree to which a system can be used by people with the widest range of characteristics and capabilities. This includes users with visual, auditory, motor, or cognitive impairments — as well as users on assistive technologies such as screen readers, switch access devices, or voice control.

Accessibility is an important design consideration and, in many contexts, a legal requirement (WCAG 2.1, EN 301 549, ADA, EAA). Architecturally, treating it as a first-class design constraint rather than a post-launch checklist reduces remediation cost.

---

## Core concepts

### WCAG principles

The Web Content Accessibility Guidelines (WCAG) define four principles, often abbreviated as POUR:

| Principle | Meaning |
|---|---|
| **Perceivable** | Information is presentable in ways users can perceive (text alternatives, captions, sufficient contrast) |
| **Operable** | Interface components are operable by all users (keyboard navigation, no seizure-triggering content, enough time) |
| **Understandable** | Information and operation is understandable (readable, predictable, input assistance) |
| **Robust** | Content can be interpreted by assistive technologies (valid HTML, ARIA, name/role/value) |

WCAG 2.1 AA is the most widely required conformance level in legislation and procurement requirements.

### Structural accessibility

Accessibility is primarily a structural concern:
- Semantic HTML communicates meaning to assistive technologies that visual design cannot
- ARIA attributes provide role and state information where native semantics are insufficient
- Keyboard navigation requires explicit focus management and logical tab order
- Color contrast must meet minimum ratios (4.5:1 for normal text at AA level)

These are structural concerns, not cosmetic changes. They require architectural decisions about how components are built and what markup they produce.

### Accessibility in APIs and developer tools

Accessibility extends to developer-facing interfaces: CLIs must work with screen readers, documentation must have sufficient color contrast, and error messages must be parseable without color as the only signal.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Semantic HTML over unsemantic `<div>` nesting | Native accessibility; correct ARIA roles | Requires discipline; frameworks may generate non-semantic markup |
| Design system with built-in accessibility | Consistent, tested accessibility across the product | Design system becomes a dependency; bespoke components bypass it |
| Automated accessibility testing in CI | Catches regressions; enforces baseline | Automated tools catch ~30–40% of issues; manual testing is still required |

---

## When to prioritize

- Public-facing systems subject to accessibility legislation.
- Systems used in contexts with diverse user populations.
- Government, education, healthcare, and financial services — where accessibility requirements are typically mandated.

## Common pitfalls

- **Accessibility retrofitted post-launch**: structural accessibility is expensive to add to an existing component library. Including it in the initial design avoids this cost.
- **Color as the only differentiator**: error states, required fields, and status indicators communicated only through color are invisible to users with color blindness or monochrome displays.
- **No keyboard navigation**: interactive elements that are only reachable by mouse exclude keyboard-only and assistive technology users.
- **Automated tools as the only test**: automated scanners catch missing alt text and contrast failures but cannot verify screen reader experience, focus management, or cognitive load. Manual testing with assistive technologies is generally needed to cover the remaining issues.
