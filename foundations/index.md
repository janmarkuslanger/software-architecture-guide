# Foundations

## Overview
There is no single definition of architecture, but one has become canonical. The Software Engineering Institute (SEI), and Bass, Clements, and Kazman in *Software Architecture in Practice*, define it as:

> The software architecture of a system is the set of structures needed to reason about the system, which comprise software **elements**, **relations** among them, and **properties** of both.

Three ideas carry the definition:

- **Elements** are the building blocks you can reason about and assign work to: modules, components, services, layers. They are abstractions, not necessarily source files.
- **Relations** describe how elements connect and interact: who calls whom, what depends on what, how data flows. Coupling lives here.
- **Properties** are the externally visible characteristics of elements and relations: an interface contract, a latency budget, a security boundary, an allowed direction of dependency. This is where quality attributes and constraints attach.

A practical reading: architecture is the sum of architectural characteristics (what the system must do well), architectural decisions (the rules and constraints the system is built on), logical structure (how components are organized), and design (how those components interact). Every architectural decision involves trade-offs. There is no option without a downside, only choices where the benefits outweigh the costs in a given context.

## Core concepts

**Structure** *(elements)*
Each module or service owns a clear capability and its data. Users, external systems, regulations, and operational limits all influence where you draw those boundaries.

**Contracts** *(relations)*
Interfaces define stable inputs, outputs, and error behavior. Less coupling (temporal, spatial, or data) means less coordination between teams and services. Functional cohesion is strongest; avoid grouping by technical layer alone.

**Quality & Constraints** *(properties)*
Quality attributes: scalability, reliability, availability, security, maintainability and performance describe what the system must do well. Constraints like budget, team size, compliance and technology limit what is actually feasible. You cannot maximize all attributes at once; document the chosen balance.

**Evolution**
Architecture is not a one-time decision. It evolves iteratively as requirements, team, and context change. Avoid locking in irreversible decisions early, measure actual behavior, and refactor safely. The goal is to keep technical debt low, but no decision is ever perfect. You work with incomplete information and real constraints, so the right call is to make a reasonable choice, document the trade-offs, and revisit when you know more.

## Topics
- [Modularity](modularity.md): cohesion, coupling, connascence
- [Component Principles](component-principles.md): REP, CCP, CRP, ADP, SDP, SAP
- [Glossary](glossary.md)