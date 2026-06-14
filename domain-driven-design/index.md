# Domain-Driven Design

## Overview
Domain-Driven Design (DDD) is an approach to software design that places the business domain at the centre of the model. The structure of the code, its boundaries, and the language used to describe it are derived from the domain, not from technical layers or storage concerns.

DDD was introduced by Eric Evans in *Domain-Driven Design: Tackling Complexity in the Heart of Software* (2003) and later refined by Vaughn Vernon and others. It is most useful when domain complexity, rather than technical complexity, dominates the cost of change.

## Two levels: strategic and tactical
DDD operates at two levels that are usually applied together.

**Strategic design** deals with the system as a whole: which parts of the domain matter most, how the domain is divided into separable contexts, what language is used in each context, and how those contexts relate.

**Tactical design** deals with the inside of a single context: how to model entities, value objects, aggregates, domain events, and the services that orchestrate them.

Strategic decisions shape where boundaries lie. Tactical decisions shape what lives inside each boundary.

## Why DDD
- **Domain complexity is the dominant cost.** When business rules drive most change, modelling them explicitly reduces the cost of future change.
- **Language drift is expensive.** When developers, domain experts, and stakeholders use different words for the same concept, miscommunication compounds over time. A shared language reduces translation cost.
- **Bounded contexts make large systems tractable.** Splitting a system along domain lines, rather than technical lines, enables independent evolution of parts.
- **Architectural styles benefit from explicit boundaries.** Modular monoliths, microservices, and event-driven systems all rely on knowing where one part of the domain ends and another begins.

## When DDD fits
- The domain is non-trivial and evolves over time.
- Multiple teams or roles (domain experts, product, engineering) need a shared understanding.
- The system spans several distinct business capabilities that interact.
- Long-term maintainability and clarity matter more than initial speed.

## When DDD does not fit
- The domain is simple, well-understood, and unlikely to change (e.g., a CRUD admin tool).
- The system is small enough that the overhead of explicit modelling exceeds its benefit.
- There is no access to domain experts and no realistic way to refine a shared language.
- The bottleneck is technical (performance, integration) rather than domain complexity.


## Topics
- [Strategic Design](strategic-design.md): domains, subdomains, bounded contexts
- [Ubiquitous Language](ubiquitous-language.md): language as a design tool
- [Context Maps](context-maps.md): relationships between bounded contexts
- [Tactical Design](tactical-design.md): entities, value objects, aggregates, domain events, repositories
- [Event Storming](event-storming.md): a discovery method for domain models
