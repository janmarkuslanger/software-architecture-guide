# Foundations

## What is software architecture?
There is no single definition, but the most widely used comes from the Software Engineering Institute (SEI), and Bass, Clements, and Kazman in *Software Architecture in Practice*:

> The software architecture of a system is the set of structures needed to reason about the system, which comprise software **elements**, **relations** among them, and **properties** of both.

This is the working definition for this guide. It is precise, technology-neutral, and it hands you a vocabulary: whatever you are describing, you are describing elements, the relations between them, and their properties.

Other definitions sharpen different angles, and they agree more than they differ:

- **ISO/IEC/IEEE 42010**, the international standard, frames architecture as the "fundamental concepts or properties of a system in its environment, embodied in its elements, relationships, and in the principles of its design and evolution" — the same triad, with an explicit nod to *evolution*.
- **Grady Booch** focuses on significance: architecture represents "the significant design decisions that shape a system, where significant is measured by cost of change." Not every decision is architectural, only the ones that are expensive to reverse.
- **Martin Fowler / Ralph Johnson** reduce it to "the important stuff, whatever that is," and "the things people perceive as hard to change."

Taken together: architecture is the set of structures (elements, relations, properties) that matter because they are hard and costly to change.

## Elements, relations, and properties
The SEI triad is the core mental model; everything below is an elaboration of it.

**Elements**
The building blocks you can reason about and assign work to: modules, components, services, layers. They are abstractions, not necessarily source files.

**Relations**
How elements connect and interact: who calls whom, what depends on what, how data flows. Coupling lives here.

**Properties**
The externally visible characteristics of elements and relations: an interface contract, a latency budget, a security boundary, an allowed direction of dependency. Internal implementation is not a property; only what other elements can observe and rely on is.

A system has many structures, not one. How the code is organized, how things interact at runtime, and how software maps to hardware and teams are all valid views of the same architecture. You pick the structures that let you reason about the concerns you actually care about.

## Architecture is a set of trade-offs
A practical reading of the definition: architecture is the sum of architectural characteristics (what the system must do well), architectural decisions (the rules and constraints the system is built on), logical structure (how elements are organized), and design (how those elements interact).

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
- [Modularity](modularity.md): cohesion, coupling, connascence
- [Component Principles](component-principles.md): REP, CCP, CRP, ADP, SDP, SAP
- [Glossary](glossary.md)
