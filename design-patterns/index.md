# Design Patterns

Design patterns are proven, reusable solutions to recurring software design problems. They are not code — they are templates for solving a class of problem in a given context.

The canonical catalog comes from the Gang of Four (GoF) book *Design Patterns* (1994) and divides patterns into three categories based on their purpose:

| Category | Concern |
|---|---|
| **Creational** | How objects are created, decoupling creation from usage |
| **Structural** | How objects and classes are composed into larger structures |
| **Behavioral** | How objects communicate and distribute responsibility |

---

## Creational Patterns

Control object instantiation. The goal is to decouple the client from the concrete classes it depends on.

| Pattern | Intent | Typical use |
|---|---|---|
| [**Singleton**](creational/singleton.md) | Ensure only one instance exists and provide a global access point | Config registry, logger, connection pool |
| [**Factory Method**](creational/factory-method.md) | Let subclasses decide which class to instantiate | Framework extension points, plug-in systems |
| [**Abstract Factory**](creational/abstract-factory.md) | Create families of related objects without specifying concrete classes | UI toolkit (light/dark theme), cross-platform widgets |
| [**Builder**](creational/builder.md) | Construct complex objects step by step, separating construction from representation | Query builders, HTTP request builders, test data builders |
| [**Prototype**](creational/prototype.md) | Clone an existing object instead of creating from scratch | Copying configured objects, template-based creation |

---

## Structural Patterns

Describe how classes and objects are composed to form larger structures. Focus on simplifying relationships between parts.

| Pattern | Intent | Typical use |
|---|---|---|
| [**Adapter**](structural/adapter.md) | Convert one interface into another the client expects | Integrating a legacy API or third-party library |
| [**Bridge**](structural/bridge.md) | Separate abstraction from its implementation so both can vary independently | Platform-specific rendering, notification channels |
| [**Composite**](structural/composite.md) | Treat individual objects and compositions of objects uniformly | File system trees, UI component hierarchies, menus |
| [**Decorator**](structural/decorator.md) | Attach additional responsibilities to an object dynamically | HTTP middleware, logging wrappers, caching layers |
| [**Facade**](structural/facade.md) | Provide a simplified interface to a complex subsystem | SDK surface over a complex internal library |
| [**Flyweight**](structural/flyweight.md) | Share common state across many fine-grained objects to reduce memory | Character glyphs in a text editor, game tile maps |
| [**Proxy**](structural/proxy.md) | Provide a surrogate that controls access to another object | Lazy loading, access control, caching |

---

## Behavioral Patterns

Define how objects interact and distribute responsibility. Focus on algorithms and communication between objects.

| Pattern | Intent | Typical use |
|---|---|---|
| [**Chain of Responsibility**](behavioral/chain-of-responsibility.md) | Pass a request along a chain of handlers until one handles it | Middleware pipelines, event bubbling, approval workflows |
| [**Command**](behavioral/command.md) | Encapsulate a request as an object, enabling queuing, logging, and undo | Undo/redo, task queues, macro recording |
| [**Interpreter**](behavioral/interpreter.md) | Define a grammar and an interpreter for a language | Expression parsers, rule engines |
| [**Iterator**](behavioral/iterator.md) | Access elements of a collection sequentially without exposing its structure | Custom data structures, cursor-based pagination |
| [**Mediator**](behavioral/mediator.md) | Define an object that encapsulates how a set of objects interact | Event buses, UI form coordination |
| [**Memento**](behavioral/memento.md) | Capture and restore an object's internal state | Undo history, state snapshots, game saves |
| [**Observer**](behavioral/observer.md) | Notify dependents automatically when an object changes state | Event systems, pub/sub, UI data binding |
| [**State**](behavioral/state.md) | Let an object alter its behavior when its internal state changes | Order lifecycle, connection state machines |
| [**Strategy**](behavioral/strategy.md) | Define a family of algorithms, encapsulate each, and make them interchangeable | Sorting, pricing rules, payment methods |
| [**Template Method**](behavioral/template-method.md) | Define the skeleton of an algorithm; let subclasses fill in specific steps | Data pipelines, report generators |
| [**Visitor**](behavioral/visitor.md) | Add operations to objects without modifying them | AST traversal, document export, analytics |

---

## Decision considerations / trade-offs

| | Pro | Con |
|---|---|---|
| Shared vocabulary | Patterns give teams a common language ("use a Strategy here") | Overuse leads to pattern-for-pattern's-sake code that obscures intent |
| Reuse | Tested structure reduces bugs in recurring problems | Applying the wrong pattern creates unnecessary indirection |
| Extensibility | Open/closed principle — extend without modifying existing code | Additional abstractions increase cognitive load for new team members |
| Testability | Interfaces and injection points make units easier to test | Too many patterns can make it hard to trace execution flow |

---

## When to use / when not to use

Use patterns when the same structural or behavioral problem appears in multiple places, when the team needs a shared vocabulary, or when extensibility of a specific component is a real requirement.

Avoid patterns when a direct and simple solution is clearer. Patterns are tools, not goals. A misapplied pattern causes more harm than no pattern at all.

---

## Common pitfalls

Patterns are most harmful when applied for the wrong reasons. The most common mistakes are using a pattern to signal expertise rather than to solve a real problem, applying a pattern before the problem it solves actually exists, and choosing a pattern based on its name rather than its intent. Each individual pattern page covers the pitfalls specific to that pattern.
