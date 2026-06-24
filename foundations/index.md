# Foundations

## What is software architecture?
There is no single definition. Three influential approaches each emphasize something the others leave implicit; together they give a fuller picture than any one alone.

### Structures — SEI (Bass, Clements, Kazman)
> The software architecture of a system is the set of structures needed to reason about the system, which comprise software **elements**, **relations** among them, and **properties** of both.

This approach hands you a vocabulary: whatever you describe, you describe elements, the relations between them, and their properties.

**Elements**
The building blocks you can reason about and assign work to: modules, components, services, layers. They are abstractions, not necessarily source files.

**Relations**
How elements connect and interact: who calls whom, what depends on what, how data flows. Coupling lives here.

**Properties**
The externally visible characteristics of elements and relations: an interface contract, a latency budget, a security boundary, an allowed direction of dependency. Internal implementation is not a property; only what other elements can observe and rely on is.

A system has many structures, not one. How the code is organized, how things interact at runtime, and how software maps to hardware and teams are all valid views of the same architecture.

### A system in its environment — ISO/IEC/IEEE 42010
> Fundamental concepts or properties of a system in its environment, embodied in its elements, relationships, and in the principles of its design and evolution.

The international standard shares the same triad, but adds two things the others leave implicit: the system's **environment** (its context and stakeholders) and the **principles** that guide its design and evolution over time.

### Significance — Booch / Fowler
> Architecture represents the significant design decisions that shape a system, where significant is measured by cost of change. — Grady Booch

Martin Fowler and Ralph Johnson reduce it further to "the important stuff, whatever that is," and "the things people perceive as hard to change." This approach does not tell you what architecture is made of, but which decisions are worth calling architectural: the ones that are expensive to reverse.

### Putting them together
The three are complementary, not competing:

- **SEI** tells you what architecture is made of: structures.
- **ISO 42010** reminds you it lives in an environment and evolves.
- **Booch / Fowler** tell you which parts matter: the costly-to-change ones.

A working synthesis: architecture is the set of structures (elements, relations, properties) that matter because they are hard and costly to change.

## Design vs architecture
Architecture and design are not two separate activities; they are the same activity at different levels of significance. Every architectural decision is also a design decision, but not every design decision is an architectural one. Choosing how to name a variable, how to structure a single function, or which loop to use is design. Choosing how services communicate, where a transactional boundary sits, or whether the system is a monolith or a set of services is architecture *and* design.

The hard question is where the line falls. "Significant" (Booch) and "costly to change" (Fowler) are the right idea but hard to apply in the moment. A more operational rule: a decision is architectural when its **significance is high**, and significance can be estimated as

> **significance ≈ fan-in × effort to change**

- **Fan-in** is how many things depend on the decision: how many components, modules, or teams would have to change with it. (See [fan-in / fan-out](glossary.md) in the glossary.)
- **Effort to change** is what reversing or replacing the decision costs once the system is built: the work, risk, and coordination involved.

Either factor alone is not enough. A decision with high effort but fan-in of one — a gnarly but self-contained algorithm — is local design: painful to rewrite, but nothing else depends on it. A decision with high fan-in but trivial effort — a constant that is easy to change everywhere — is also just design. It is the **product** that makes something architectural: many dependents *and* expensive to change.

| | low effort to change | high effort to change |
|---|---|---|
| **low fan-in** | trivial design | local design |
| **high fan-in** | convention / cheap to fix | **architecture** |

This is also why the same kind of decision can be architectural in one system and not in another. Picking a database is architectural when dozens of services depend on its query model and migrating means coordinated downtime; it is a mere design detail in a small app hidden behind one repository class. The decision did not change — its fan-in and the effort to change it did.

The practical takeaway: when a decision has high fan-in and is expensive to reverse, treat it as architecture. Slow down, write it down, and weigh the trade-offs deliberately, because you will likely live with it for a long time.

## Architecture is a set of trade-offs
A practical reading: architecture is the sum of architectural characteristics (what the system must do well), architectural decisions (the rules and constraints the system is built on), logical structure (how elements are organized), and design (how those elements interact).

Every architectural decision involves trade-offs. There is no option without a downside, only choices where the benefits outweigh the costs in a given context. The recurring concerns:

**Boundaries**
Each module or service owns a clear capability and its data. Users, external systems, regulations, and operational limits all influence where you draw those boundaries.

**Contracts**
Interfaces define stable inputs, outputs, and error behavior, the property other elements depend on. Less coupling (temporal, spatial, or data) means less coordination between teams and services. Functional cohesion is strongest; avoid grouping by technical layer alone.

**Quality & constraints**
Quality attributes (scalability, reliability, availability, security, maintainability, performance) describe what the system must do well. Constraints like budget, team size, compliance, and technology limit what is feasible. You cannot maximize everything at once; document the chosen balance.

## Architecture evolves
Architecture is not a one-time decision. It evolves iteratively as requirements, team, and context change. Avoid locking in irreversible decisions early, measure actual behavior, and refactor safely. The goal is to keep technical debt low, but no decision is ever perfect. You work with incomplete information and real constraints, so the right call is to make a reasonable choice, document the trade-offs, and revisit when you know more.

## Topics
- [Requirements](requirements.md): the four-kind taxonomy and architecturally significant requirements (ASRs)
- [Modularity](modularity.md): cohesion, coupling, connascence
- [Component Principles](component-principles.md): REP, CCP, CRP, ADP, SDP, SAP
- [Glossary](glossary.md)
