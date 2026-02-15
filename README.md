<h1 align="center">A software architecture guide</h1>

<p align="center"><img src="head.webp" alt="Dog assembling building blocks" width="300" /></p>

---

# Introduction

This guide aims to provide a concise overview of various aspects of software architecture. It covers key concepts, architectural styles, and essential principles to help people understand the foundational elements of building software systems.

## What is Software Architecture?

There is no clear and universally accepted definition of software architecture, as it heavily depends on the context and objectives of a system. Generally, software architecture describes the entirety of decisions that shape the design, structure, and behavior of a software system. These decisions can be made consciously or unconsciously.
Regardless of whether these decisions are deliberate or accidental, every software project inevitably develops a software architecture. The quality and clarity of this architecture significantly impact the maintainability, extensibility, and scalability of the system.

---

## Key Concepts

### Coupling

Coupling describes the degree of dependency between two components in a system.
In general, we aim for a system with as **loose coupling** between components as possible.
A component can represent various entities, such as a module, a service, or a database.

Dependencies can manifest in many forms and affect the flexibility and maintainability of the system.

Types of coupling:

- Temporal coupling: Building blocks are dependent on the timing of their execution.
- Data coupling: Building blocks are connected by exchanging data.
- Content coupling: A building block directly accesses or manipulates internal data of another building block.
- Control coupling: A building block controls the behavior of another by passing control data.
- Contextual coupling: Building blocks depend on a shared context (for example configuration).
- Common coupling: Building blocks share the same global resources.
- Sequential coupling: The output of one building block serves as the input for another building block.
- External coupling: A building block depends on external systems, services, or interfaces.

Examples of Coupling:

- Component A imports another component B: This creates a direct dependency where A relies on the functionality or structure of B. If B changes, A might also need to be updated.
- A component depends on receiving data from a REST API: The component must wait for the API's response before it can process or proceed. This introduces a dependency on external communication and response times.

### Cohesion

Cohesion describes how strongly components within a module or system are related. In general, we aim to achieve high cohesion.
High cohesion ensures that components are focused on a single responsibility, making the system easier to understand, maintain, and extend.

## Quality Attributes

Quality attributes describe what “good” looks like for a system. They help to set priorities.
Not all are equally important. Pick the top 2–3 for your system.

- Maintainability: How easy is it to change the system?
- Scalability: Can the system handle more users or data?
- Reliability: Does it behave correctly and predictably?
- Availability: Is it up and running when people need it?
- Performance: Is it fast enough?
- Security: Are data and access protected?
- Operations: Is it easy to run and monitor?
- Cost: Is it affordable to build and run?

## Trade-offs

Architecture is about choices. Most choices have upsides and downsides.
You can’t optimize everything at the same time.

- Speed vs. consistency: Faster delivery can reduce consistency.
- Flexibility vs. simplicity: More options can add complexity.
- Cost vs. availability: High availability usually costs more.
- Isolation vs. reuse: Shared code is easy to reuse but harder to change safely.

## Decisions and Records

Important decisions should be written down. This helps new people and avoids repeated debates.
One simple way is an ADR (Architecture Decision Record).

An ADR is a short note that includes:

- The decision
- The reason
- Alternatives that were considered
- The date

## Diagrams

Diagrams help people understand a system quickly. Common types are:

- Context diagram: Shows the system and the outside world. Use it to explain scope.
- Container diagram: Shows the main parts and how they connect. Use it for the big picture.
- Component diagram: Shows parts inside a container. Use it for deeper detail.
- Sequence diagram: Shows steps over time. Use it for a single flow.
- Deployment diagram: Shows where software runs. Use it to explain infrastructure.
- Data flow diagram: Shows how data moves. Use it for data pipelines.
- ER diagram: Shows data models and relations. Use it for database design.

---

## Architecture Styles

### Monolithic Architecture

A single app where everything runs together. UI, business logic, and data access are deployed as one unit.

Pros:
- Easy to start and test
- Fewer interfaces
- Simple deployment

Cons:
- Changes can affect the whole system
- Scaling is all or nothing
- Teams can block each other

Fits when:
- The system is small or still early

Example:
- A small internal admin service

### Microservices

Many small services, each with a clear responsibility. Services talk through APIs and are deployed separately.

Pros:
- Parts can be changed independently
- Teams can work in parallel
- Scaling per service

Cons:
- More ops and tooling needed
- More interfaces to manage
- Data management is more complex

Fits when:
- The system is large and teams need autonomy

Example:
- An online shop with separate services for orders, payment, shipping

### Event-Driven Architecture

Parts publish events. Other parts react. This keeps things loosely coupled.

Pros:
- Good decoupling
- Fast reactions
- Easy to extend

Cons:
- Flow is harder to follow
- Debugging is harder
- Event order can be tricky

Fits when:
- Many parts need to react to changes

Example:
- Payment happens -> event triggers shipping and invoice

### Service-Oriented Architecture (SOA)

Large services with clear interfaces, often aligned to business areas. Services are broader than microservices.

Pros:
- Good reuse
- Clear contracts between services
- Works well for large organizations

Cons:
- Services can become large and heavy
- Changes can take longer
- More governance needed

Fits when:
- Many products share the same business capabilities

Example:
- A central customer service used by many products

### Serverless Architecture

Code runs as functions. The cloud provider runs the servers and scales automatically.

Pros:
- No server management
- Auto-scaling
- Pay per use

Cons:
- Vendor lock-in
- Cold starts are possible
- Limits on runtime and resources

Fits when:
- Workloads are event-based or bursty

Example:
- Image processing after an upload to cloud storage

### Cloud Architecture

Systems are built on cloud services like managed databases, storage, and queues.

Pros:
- Fast provisioning
- Many managed services
- Good scaling

Cons:
- Cost control is important
- Vendor lock-in
- Security and compliance topics

Fits when:
- You want fast delivery with managed services

Example:
- A web app with a managed DB, storage, and CDN

---

## Design Patterns

Design patterns are common solutions to recurring design problems. They give teams a shared language and speed up decisions.

### Creational Patterns

These patterns create objects in a flexible way.

#### Abstract Factory
**Overview:**
Creates families of related objects without naming the concrete classes.
**Benefits:**
- Keeps products consistent
- Easy to switch a whole family
**Trade-offs:**
- Harder to add new product types
- More classes to manage
**When to use:**
- You need matching UI or service families
**Example:**
Create Windows or macOS UI widgets

```python
class WinFactory:
    def button(self): return "WinButton"
    def checkbox(self): return "WinCheckbox"

class MacFactory:
    def button(self): return "MacButton"
    def checkbox(self): return "MacCheckbox"

def render_ui(factory):
    return factory.button(), factory.checkbox()
```

#### Builder
**Overview:**
Builds complex objects step by step with the same construction process.
**Benefits:**
- Clear step-by-step build
- Optional parts are easy
**Trade-offs:**
- More code
- Extra classes
**When to use:**
- An object has many optional parts
**Example:**
Build a complex HTTP request

```python
class RequestBuilder:
    def __init__(self): self.req = {"headers": {}, "params": {}}
    def header(self, k, v): self.req["headers"][k] = v; return self
    def param(self, k, v): self.req["params"][k] = v; return self
    def build(self): return self.req

req = RequestBuilder().header("Auth", "token").param("q", "books").build()
```

#### Factory Method
**Overview:**
Creates objects through a method, so subclasses decide the concrete type.
**Benefits:**
- Decouples creation from use
- Easy to extend with new types
**Trade-offs:**
- Many small subclasses
**When to use:**
- Subclasses should choose which object to create
**Example:**
A document app creates PDF or Word documents

```python
class DocCreator:
    def create(self): raise NotImplementedError
    def open(self): return f"Open {self.create()}"

class PdfCreator(DocCreator):
    def create(self): return "PDF"

class WordCreator(DocCreator):
    def create(self): return "Word"
```

#### Prototype
**Overview:**
Creates new objects by cloning existing ones.
**Benefits:**
- Fast creation
- Easy to customize
**Trade-offs:**
- Deep copies can be tricky
**When to use:**
- You need copies of preconfigured objects
**Example:**
Duplicate a preconfigured form

```python
import copy

class Form:
    def __init__(self, fields): self.fields = fields
    def clone(self): return copy.deepcopy(self)

base = Form(["name", "email"])
custom = base.clone()
custom.fields.append("company")
```

#### Singleton
**Overview:**
Ensures one instance and provides a global access point.
**Benefits:**
- One shared instance
- Easy access
**Trade-offs:**
- Global state is hard to test
- Hidden dependencies
**When to use:**
- You truly need one instance
**Example:**
A single configuration object

```python
class Config:
    _instance = None
    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance

c1, c2 = Config(), Config()
assert c1 is c2
```

### Structural Patterns

These patterns organize classes and objects.

#### Adapter
**Overview:**
Converts one interface into another expected by the client.
**Benefits:**
- Lets incompatible components work together
- Keeps old code stable
**Trade-offs:**
- Adds extra layers
- Can hide integration issues
**When to use:**
- You need to integrate a new interface
**Example:**
Use a new payment provider with an old interface

```python
class LegacyPay:
    def pay_legacy(self, cents): return f"paid {cents}"

class PayAdapter:
    def __init__(self, legacy): self.legacy = legacy
    def pay(self, dollars): return self.legacy.pay_legacy(dollars * 100)
```

#### Bridge
**Overview:**
Separates abstraction from implementation so both can change independently.
**Benefits:**
- Avoids subclass explosion
- Two dimensions can evolve independently
**Trade-offs:**
- More setup
- More indirection
**When to use:**
- You have two dimensions of change
**Example:**
Remote controls that work with many devices

```python
class Device:
    def on(self): pass

class TV(Device):
    def on(self): return "TV on"

class Remote:
    def __init__(self, device): self.device = device
    def power(self): return self.device.on()
```

#### Composite
**Overview:**
Builds tree structures so single items and groups look the same.
**Benefits:**
- Simple handling of trees
- Same API for leaf and group
**Trade-offs:**
- Harder to restrict component types
- Some operations get complex
**When to use:**
- You model part-whole hierarchies
**Example:**
A file system with folders and files

```python
class File:
    def __init__(self, name): self.name = name
    def show(self): return [self.name]

class Folder:
    def __init__(self, name): self.name, self.items = name, []
    def add(self, item): self.items.append(item)
    def show(self):
        out = [self.name]
        for i in self.items: out += i.show()
        return out
```

#### Decorator
**Overview:**
Adds behavior to objects without changing their class.
**Benefits:**
- Flexible and composable features
- Add behavior at runtime
**Trade-offs:**
- Many wrapper classes
- Debugging is harder
**When to use:**
- You need optional features
**Example:**
Add compression and encryption to a stream

```python
class Stream:
    def read(self): return "data"

class Encrypted(Stream):
    def __init__(self, stream): self.stream = stream
    def read(self): return f"enc({self.stream.read()})"
```

#### Facade
**Overview:**
Provides a simple interface to a complex subsystem.
**Benefits:**
- Easier to use
- Fewer dependencies
**Trade-offs:**
- Can hide useful features
- Can grow into a god interface
**When to use:**
- You want a simple entry point to a complex system
**Example:**
A single API for a complex payment system

```python
class Auth:
    def ok(self): return True
class Charge:
    def run(self): return "charged"
class Receipt:
    def send(self): return "sent"

class PaymentFacade:
    def pay(self):
        if Auth().ok():
            Charge().run(); return Receipt().send()
```

#### Flyweight
**Overview:**
Shares common data to save memory when many objects are similar.
**Benefits:**
- Lower memory usage
- Faster creation for repeated objects
**Trade-offs:**
- Shared state is harder to manage
- Code is more complex
**When to use:**
- Many small objects share the same data
**Example:**
Characters in a text editor

```python
class Glyph:
    def __init__(self, char): self.char = char

class GlyphFactory:
    _cache = {}
    def get(self, char):
        if char not in self._cache:
            self._cache[char] = Glyph(char)
        return self._cache[char]
```

#### Proxy
**Overview:**
Controls access to another object.
**Benefits:**
- Lazy loading, caching, or security
- Extra control without changing the real object
**Trade-offs:**
- Extra latency
- More complexity
**When to use:**
- You need access control or lazy loading
**Example:**
Lazy-loading large images

```python
class RealImage:
    def load(self): return "image loaded"

class ImageProxy:
    def __init__(self): self.real = None
    def load(self):
        if not self.real: self.real = RealImage()
        return self.real.load()
```

### Behavioral Patterns

These patterns define how objects communicate.

#### Chain of Responsibility
**Overview:**
Passes a request through a chain until one handler processes it.
**Benefits:**
- Loose coupling between sender and handler
- Easy to reorder handlers
**Trade-offs:**
- Not clear who handles the request
- Requests can go unhandled
**When to use:**
- Multiple handlers could process the same request
**Example:**
Support tickets routed by category

```python
class Handler:
    def __init__(self, nxt=None): self.nxt = nxt
    def handle(self, req): return self.nxt.handle(req) if self.nxt else "none"

class Auth(Handler):
    def handle(self, req): return "auth" if req == "auth" else super().handle(req)
```

#### Command
**Overview:**
Wraps a request as an object.
**Benefits:**
- Supports undo, logging, and queues
- Decouples sender and receiver
**Trade-offs:**
- Many small command classes
- Extra boilerplate
**When to use:**
- You need undo or queued actions
**Example:**
UI actions with undo

```python
class Command:
    def execute(self): pass

class LightOn(Command):
    def execute(self): return "on"

class Remote:
    def __init__(self, cmd): self.cmd = cmd
    def press(self): return self.cmd.execute()
```

#### Interpreter
**Overview:**
Defines a grammar and interprets expressions.
**Benefits:**
- Simple to add small rules
- Easy to extend the grammar
**Trade-offs:**
- Slow for large grammars
- Hard to maintain at scale
**When to use:**
- You have a small, simple language
**Example:**
A simple rule engine

```python
class Number:
    def __init__(self, v): self.v = v
    def eval(self): return self.v

class Add:
    def __init__(self, a, b): self.a, self.b = a, b
    def eval(self): return self.a.eval() + self.b.eval()
```

#### Iterator
**Overview:**
Accesses elements of a collection without exposing its structure.
**Benefits:**
- Consistent traversal API
- Hides internal structure
**Trade-offs:**
- Can hide performance costs
- Adds extra objects
**When to use:**
- You need to traverse different collections the same way
**Example:**
Iterating over a tree

```python
class Bag:
    def __init__(self, items): self.items = items
    def __iter__(self):
        for i in self.items: yield i
```

#### Mediator
**Overview:**
Centralizes communication between objects.
**Benefits:**
- Reduces direct dependencies
- Keeps objects simpler
**Trade-offs:**
- Mediator can become complex
- Hard to test if it grows too large
**When to use:**
- Many objects talk to each other
**Example:**
A chat room managing messages

```python
class ChatRoom:
    def __init__(self): self.users = []
    def join(self, u): self.users.append(u)
    def send(self, msg): return [u.recv(msg) for u in self.users]
```

#### Memento
**Overview:**
Captures and restores an object's state.
**Benefits:**
- Enables undo
- Keeps encapsulation
**Trade-offs:**
- Can use lots of memory
- State management can get complex
**When to use:**
- You need restore points
**Example:**
Undo in a text editor

```python
class Editor:
    def __init__(self): self.text = ""
    def save(self): return self.text
    def restore(self, m): self.text = m
```

#### Observer
**Overview:**
Notifies many dependents when a subject changes.
**Benefits:**
- Loose coupling
- Easy to add new observers
**Trade-offs:**
- Update cascades can be hard to debug
- Update order is not obvious
**When to use:**
- Many parts depend on the same change
**Example:**
UI updates when data changes

```python
class Subject:
    def __init__(self): self.obs = []
    def attach(self, o): self.obs.append(o)
    def notify(self, msg):
        for o in self.obs: o.update(msg)
```

#### State
**Overview:**
Changes behavior when internal state changes.
**Benefits:**
- Removes large conditional logic
- Clear state transitions
**Trade-offs:**
- More classes
- More moving parts
**When to use:**
- Behavior depends on state
**Example:**
Order states like New, Paid, Shipped

```python
class New:
    def next(self): return Paid()
class Paid:
    def next(self): return Shipped()
class Order:
    def __init__(self): self.state = New()
    def advance(self): self.state = self.state.next()
```

#### Strategy
**Overview:**
Encapsulates algorithms and makes them interchangeable.
**Benefits:**
- Swap behavior at runtime
- Keeps algorithms isolated
**Trade-offs:**
- Many small classes
- Extra wiring
**When to use:**
- You have multiple algorithms for one task
**Example:**
Different pricing rules

```python
class Fixed:
    def price(self, base): return base
class Discount:
    def price(self, base): return base * 0.9
class Checkout:
    def __init__(self, strategy): self.strategy = strategy
    def total(self, base): return self.strategy.price(base)
```

#### Template Method
**Overview:**
Defines a skeleton algorithm with steps overridden in subclasses.
**Benefits:**
- Consistent flow
- Custom steps where needed
**Trade-offs:**
- Inheritance can be rigid
- Hard to change the base flow
**When to use:**
- Steps are the same but details differ
**Example:**
Report generation with shared steps

```python
class Exporter:
    def export(self):
        data = self.load(); return self.format(data)
    def load(self): raise NotImplementedError
    def format(self, data): raise NotImplementedError
```

#### Visitor
**Overview:**
Adds new operations to object structures without changing the objects.
**Benefits:**
- Easy to add new operations
- Keeps element classes stable
**Trade-offs:**
- Hard to add new element types
- Can be verbose
**When to use:**
- The object structure is stable
**Example:**
Running analytics on an AST

```python
class Node:
    def accept(self, v): return v.visit(self)

class Visitor:
    def visit(self, node): return "visited"
```

---
---

## Glossary

- Building block: A part of a system (for example a module, service, or database).
- Dependency: When one part needs another part to work or to deliver a result.

---

## Contribution

If you'd like to contribute, feel free to create a pull request.
If you think a topic is missing, please open an issue.
