# Design Patterns

## Overview
Design patterns are proven, reusable solutions to recurring software design problems. They are not code, they are templates for solving a class of problem in a given context.

The canonical catalog comes from the Gang of Four (GoF) book *Design Patterns* (1994) and divides patterns into three categories based on their purpose:

| Category | Concern |
|---|---|
| **Creational** | How objects are created which decouples creation from usage |
| **Structural** | How objects and classes are composed into larger structures |
| **Behavioral** | How objects communicate and distribute responsibility |

---

## Creational Patterns

Control object instantiation. The goal is to decouple the client from the concrete classes it depends on.

| Pattern | Intent | Typical use |
|---|---|---|
| **Singleton** | Ensure only one instance exists; provide a global access point | Config registry, logger, connection pool |
| **Factory Method** | Let subclasses decide which class to instantiate | Framework extension points, plug-in systems |
| **Abstract Factory** | Create families of related objects without specifying concrete classes | UI toolkit (light/dark theme), cross-platform widgets |
| **Builder** | Construct complex objects step by step, separating construction from representation | Query builders, HTTP request builders, test data builders |
| **Prototype** | Clone an existing object instead of creating from scratch | Copying configured objects, template-based creation |


---

## Structural Patterns

Describe how classes and objects are composed to form larger structures. Focus on simplifying relationships between parts.

| Pattern | Intent | Typical use |
|---|---|---|
| **Adapter** | Convert one interface into another the client expects | Integrating a legacy API or third-party library |
| **Bridge** | Separate abstraction from its implementation so both can vary independently | Platform-specific rendering (e.g., Draw API across Windows/Linux) |
| **Composite** | Treat individual objects and compositions of objects uniformly | File system trees, UI component hierarchies, menus |
| **Decorator** | Attach additional responsibilities to an object dynamically, as an alternative to subclassing | HTTP middleware, logging wrappers, caching layers |
| **Facade** | Provide a simplified interface to a complex subsystem | SDK surface over a complex internal library, service client hiding retry logic |
| **Flyweight** | Share common state across many fine-grained objects to reduce memory | Character glyphs in a text editor, game tile maps |
| **Proxy** | Provide a surrogate that controls access to another object | Lazy loading, access control, remote service calls |


---

## Behavioral Patterns

Define how objects interact and distribute responsibility. Focus on algorithms and communication between objects.

| Pattern | Intent | Typical use |
|---|---|---|
| **Chain of Responsibility** | Pass a request along a chain of handlers until one handles it | Middleware pipelines, event bubbling, approval workflows |
| **Command** | Encapsulate a request as an object, enabling queuing, logging, and undo | Undo/redo, task queues, macro recording |
| **Interpreter** | Define a grammar and an interpreter for a language | Expression parsers, rule engines, SQL query builders |
| **Iterator** | Provide a way to access elements of a collection sequentially without exposing its representation | Looping over custom data structures, cursor-based pagination |
| **Mediator** | Define an object that encapsulates how a set of objects interact | Chat rooms, air traffic control, UI form coordination |
| **Memento** | Capture and restore an object's internal state without violating encapsulation | Undo history, state snapshots, game save states |
| **Observer** | Define a one-to-many dependency so dependents are notified automatically on state change | Event systems, pub/sub, UI data binding |
| **State** | Allow an object to alter its behavior when its internal state changes | Order lifecycle (pending → processing → shipped), connection state machines |
| **Strategy** | Define a family of algorithms, encapsulate each, and make them interchangeable | Sorting algorithms, pricing rules, payment methods |
| **Template Method** | Define the skeleton of an algorithm in a base class; let subclasses fill in specific steps | Report generators, data import pipelines |
| **Visitor** | Let you add further operations to objects without modifying them | AST traversal, document export (PDF, HTML), analytics collection |

---

## Decision considerations / trade-offs

| | Pro | Con |
|---|---|---|
| Shared vocabulary | Patterns give teams a common language ("use a Strategy here") | Overuse leads to pattern-for-pattern's-sake code that obscures intent |
| Reuse | Tested structure reduces bugs in recurring problems | Applying the wrong pattern creates unnecessary indirection |
| Extensibility | Open/closed principle — extend without modifying existing code | Additional abstractions increase cognitive load for new team members |
| Testability | Interfaces and injection points (Strategy, Command) make units easier to test | Too many patterns can make it hard to trace execution flow |

---

## When to use / when not to use

- **Use when**: the same structural or behavioral problem appears in multiple parts of the system.
- **Use when**: the team needs a shared vocabulary to communicate design decisions efficiently.
- **Use when**: extensibility or replaceability of a specific component is a real requirement.
- **Avoid when**: a direct, simple solution is clearer — patterns are not goals, they are tools.
- **Avoid when**: the problem is genuinely one-off; a bespoke solution is often better than a pattern misfit.
- **Avoid when**: the team is unfamiliar with the pattern — misapplied patterns cause more harm than no pattern.

---

## Common pitfalls

- **Pattern overuse**: forcing a pattern onto a problem it doesn't fit to signal expertise.
- **Singleton abuse**: using Singleton as a global variable substitute, introducing hidden coupling.
- **Decorator chains**: stacking too many decorators makes call order and debugging opaque.
- **Observer memory leaks**: observers not unregistered keep the subject alive longer than intended.
