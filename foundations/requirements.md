# Requirements

Architecture is driven by requirements, but not by all of them equally. To decide which requirements actually shape structure, you first need a vocabulary that is sharper than the historical "functional vs. non-functional" split.

That split is misleading. "Non-functional" sounds optional, and it lumps three very different kinds of requirement into one bucket. A more useful taxonomy (Bass/Clements/Kazman, Rozanski & Woods, iSAQB) breaks requirements into **four** kinds.

---

## A taxonomy of requirements

### 1. Functional requirements (FR) — *what* the system does
Features, behavior, reactions to input. "A user can start a workflow." "The system sends a confirmation email." Functional requirements describe observable behavior.

The surprise: most functional requirements are **not** architecturally significant. Adding another email template or another report does not change the structure. FRs matter to users, but they rarely force a structural decision.

### 2. Quality attributes — *how well* the system does it
The "-ilities": performance, availability, security, scalability, maintainability, and so on. This is the precise replacement for half of what "non-functional" used to mean.

A bank's need for strong security is a quality attribute, not a feature. A quality attribute is a **goal you choose a level for** and trade off against others. Qualities are made measurable through quality-attribute scenarios (source, stimulus, response, response measure); see [Quality Attributes](../quality-attributes/index.md).

### 3. Constraints — decisions already made for you
A constraint is a non-negotiable decision imposed from outside, before you start weighing options. "Must run on-prem." "Must be Java." "GDPR applies." "Budget X." "Deadline Y."

This is the other half of the "bank" intuition. "Security must be high *because the regulator mandates it*" — the *because mandated* part is a constraint, not a quality attribute. The difference is subtle but important:

- **Quality attribute** = a goal you weigh and pick a level for.
- **Constraint** = a decision taken out of your hands.

Constraints are sharply architecture-relevant because they **cut the solution space up front**, before any trade-off is made.

### 4. Guiding principles — rules you impose on yourself
Self-chosen rules that steer many later decisions in one direction: "buy over build", "API-first", "prefer managed services", "no shared database between services." A principle is effectively a trade-off decided once, in advance, so you do not have to re-litigate it on every decision.

**Constraint vs. principle:** a constraint is imposed from *outside*; a principle is *self-chosen*. Both cut the solution space, but from different sources.

| Kind | Question it answers | Architecturally significant? |
|---|---|---|
| Functional requirement | What does it do? | Usually no |
| Quality attribute | How well does it do it? | Often |
| Constraint | What was decided for us? | Almost always |
| Guiding principle | What did we decide once, in advance? | Often |

---

## Architecturally significant requirements (ASRs)

Not every requirement deserves architectural attention. An **ASR** is a requirement whose satisfaction forces or constrains a hard-to-reverse structural decision.

This connects directly to the definition of significance in [Design vs architecture](index.md#design-vs-architecture):

> **significance ≈ fan-in × effort to change**

An ASR is exactly a requirement with high **fan-in** (many parts of the structure hang off it) and/or high **reversal cost**. Three telltale signs:

- **High structural influence.** You would have to rebuild the architecture to satisfy it after the fact. You cannot bolt "99.99% availability" on at the end.
- **Tension with other requirements.** An ASR forces a trade-off — security ↔ usability, performance ↔ maintainability. Where requirements collide, significance concentrates.
- **Expensive to retrofit.** Adding it later costs far more than designing for it from the start.

The distribution is skewed: **ASRs come disproportionately from quality attributes and constraints, rarely from functional requirements.** That is why relabeling "non-functional" into "quality attribute + constraint" is not pedantry — it is precisely where the architecture-driving requirements live.

### From concern to ASR
The chain runs:

> **Concern → Requirement → (significance filter) → ASR → significant decision**

A **concern** is a stakeholder's worry or perspective (see [Stakeholder Management](../architect-role/stakeholder-management.md)). A **requirement** is its precise, measurable form. An **ASR** is a requirement that additionally passes the significance test (fan-in × effort to change).

### Eliciting ASRs
You cannot extract ASRs by asking "what are your requirements?" Stakeholders hand you features and solutions ("just use Temporal"), not the underlying qualities and constraints. You surface ASRs by deliberately:

1. asking for the **qualities and constraints** behind the features,
2. **prioritizing** them, and
3. filtering for the ones that are (a) hard to change, (b) in conflict with each other, or (c) raised by multiple stakeholders (high fan-in).

Treat the taxonomy as a filter: spend your architectural attention on the few requirements that pass the significance test, and weigh them as deliberate trade-offs (see [Architecture Decision Making](../decision-making/index.md)).
