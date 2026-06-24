# Requirements

Requirements shape architecture, but not equally. The historical "functional vs. non-functional" split is too coarse: "non-functional" suggests "optional" and groups several distinct kinds of requirement together. A finer taxonomy (Bass/Clements/Kazman, Rozanski & Woods, iSAQB) distinguishes **four** kinds.

---

## A taxonomy of requirements

### 1. Functional requirements (FR) — *what* the system does
Features, behavior, and reactions to input: "a user can start a workflow", "the system sends a confirmation email."

Most functional requirements are not architecturally significant. Adding another email template or report does not change the structure. FRs matter to users but rarely force a structural decision.

### 2. Quality attributes — *how well* the system does it
The "-ilities": performance, availability, security, scalability, maintainability. A quality attribute is a goal you choose a level for and trade off against others — a bank's strong security is a quality attribute, not a feature.

Qualities are made measurable through quality-attribute scenarios (source, stimulus, response, response measure); see [Quality Attributes](../quality-attributes/index.md).

### 3. Constraints — decisions already made for you
A non-negotiable decision imposed from outside, before any options are weighed: "must run on-prem", "must be Java", "GDPR applies", "budget X", "deadline Y."

A constraint differs from a quality attribute by source: "security must be high *because the regulator mandates it*" — the *because mandated* is a constraint.

- **Quality attribute** — a goal you weigh and pick a level for.
- **Constraint** — a decision taken out of your hands.

Constraints are architecture-relevant because they cut the solution space up front, before any trade-off is made.

### 4. Guiding principles — rules you impose on yourself
Self-chosen rules that steer later decisions in one direction: "buy over build", "API-first", "prefer managed services", "no shared database between services." A principle is a trade-off decided once, in advance, so it need not be re-litigated on every decision.

A constraint is imposed from outside; a principle is self-chosen. Both cut the solution space, but from different sources.

| Kind | Question it answers | Architecturally significant? |
|---|---|---|
| Functional requirement | What does it do? | Usually no |
| Quality attribute | How well does it do it? | Often |
| Constraint | What was decided for us? | Almost always |
| Guiding principle | What did we decide once, in advance? | Often |

---

## Architecturally significant requirements (ASRs)

An **ASR** is a requirement whose satisfaction forces or constrains a hard-to-reverse structural decision. This maps onto the definition of significance in [Design vs architecture](index.md#design-vs-architecture):

> **significance ≈ fan-in × effort to change**

An ASR is a requirement with high fan-in (many parts of the structure depend on it) and/or high cost to reverse. Three markers:

- **High structural influence** — satisfying it later means rebuilding the architecture. "99.99% availability" cannot be bolted on at the end.
- **Tension with other requirements** — it forces a trade-off: security ↔ usability, performance ↔ maintainability.
- **Expensive to retrofit** — adding it later costs more than designing for it from the start.

ASRs come disproportionately from quality attributes and constraints, rarely from functional requirements. Relabeling "non-functional" into "quality attribute + constraint" matters because that is where the architecture-driving requirements concentrate.

### From concern to ASR

> **Concern → Requirement → (significance filter) → ASR → significant decision**

A **concern** is a stakeholder's perspective or worry (see [Stakeholder Management](../architect-role/stakeholder-management.md)). A **requirement** is its precise, measurable form. An **ASR** is a requirement that also passes the significance test (fan-in × effort to change).

### Eliciting ASRs
Asking "what are your requirements?" yields features and proposed solutions, not the underlying qualities and constraints. Surface ASRs by:

1. asking for the qualities and constraints behind the features,
2. prioritizing them, and
3. filtering for those that are hard to change, in conflict with each other, or raised by multiple stakeholders (high fan-in).

The taxonomy is a filter: spend architectural attention on the few requirements that pass the significance test, and weigh them as deliberate trade-offs (see [Architecture Decision Making](../decision-making/index.md)).
