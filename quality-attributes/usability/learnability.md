# Learnability

## Overview

Learnability is the degree to which a system enables users to learn how to use it effectively with a specified degree of effort. A learnable system allows users to become proficient within a reasonable time and without requiring external support.

Learnability is distinct from usability in general: an expert may find a system highly usable, while a novice finds it impossible to learn without assistance.

---

## Core concepts

### Learning curve

The learning curve describes the effort required to reach a defined level of proficiency. A steep learning curve (high initial investment, slow ramp-up) is acceptable for systems used intensively by professionals. It is not acceptable for systems with casual or infrequent users.

Architectural decisions that affect the learning curve:
- Consistency of interaction patterns across the system
- Quality and discoverability of documentation
- Availability and quality of in-system guidance (tooltips, inline help, examples)
- Error messages that teach rather than just report

### Progressive disclosure

Progressive disclosure is the practice of presenting only the information and options necessary for the current task, revealing complexity incrementally as users advance. It reduces the initial learning investment while preserving access to full functionality.

Architecturally, progressive disclosure requires layered interface design: a simple default path plus an explicit path to advanced options.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Consistent interaction patterns | Users transfer knowledge across the system | Requires design discipline; consistency must be enforced |
| In-system tutorials and guided flows | Reduces external documentation dependency | Maintenance overhead; tutorials become outdated |
| Progressive disclosure | Low initial learning cost | Expert users may find the layering frustrating |
| Convention over configuration | Reduces the number of things users must learn | Inflexible for users whose needs deviate from conventions |

---

## Common pitfalls

- **Inconsistent patterns**: buttons that behave differently in different parts of the system, different terminology for the same concept, and inconsistent navigation patterns each require additional learning. Inconsistency multiplies learning cost.
- **No getting-started path**: a system that exposes its full feature set without a guided path to the first successful outcome has poor learnability for new users.
- **Documentation that describes the interface but not the task**: users learn better from task-oriented documentation ("how do I achieve X?") than from reference documentation ("here is what each field does").
