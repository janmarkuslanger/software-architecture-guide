# Glossary

Common terms used throughout this guide.

---

**Indirection**
A layer between two components so they don't depend on each other directly. Makes the system more flexible, but adds complexity. Indirection can take many forms: interfaces, abstract base classes, message brokers, API gateways, or configuration. Anything that decouples a caller from a concrete implementation can be an indirection.

Without indirection the caller depends directly on a concrete implementation:
```python
report = PDFReport()
report.generate()
```

With indirection the caller depends on an abstraction, the concrete implementation is resolved elsewhere:
```python
def generate_report(report: Report) -> None:
    report.generate()
```

Adding a new report format (`HTMLReport`, `CSVReport`) requires no change to `generate_report`.

---

**Architectural Quantum**
The smallest independently deployable unit of an architecture that includes all the structural elements required for the system to function. An architectural quantum has high functional cohesion, high structural coupling, and is deployable in isolation.

In a monolith, the entire application is a single quantum. In a microservices architecture, each service is typically its own quantum. It can be deployed, scaled, and updated independently of other services.

The concept helps evaluate how granular deployment and change management can be within a system.

---

**Fitness Function**
An objective, measurable criterion used to evaluate whether an architecture meets a desired quality attribute. Fitness functions make architectural goals explicit and verifiable. They are similar to how automated tests verify functional correctness.

Examples:
- A fitness function for performance: "95% of API responses must complete within 200ms."
- A fitness function for modularity: "No component in module A may import directly from module C."
- A fitness function for availability: "The service must have an uptime of ≥ 99.9% per month."

Fitness functions can be automated (e.g. run in CI) or manual (e.g. periodic architecture reviews). The term comes from evolutionary architecture, where fitness functions guide incremental architectural change.

---

**Contention**
Contention occurs when multiple threads, processes, or services compete for the same shared resource at the same time, such as a CPU, a lock, or a database connection. The resource can only serve one at a time, so others have to wait. This waiting is a common cause of latency and poor performance under load.

A typical example is lock contention: two threads both want to write to the same data structure. One acquires the lock and proceeds, the other blocks until the lock is released.
```python
# Both threads call this function concurrently.
# Only one can hold the lock at a time — the other waits.
def transfer(amount):
    with account_lock:
        account.balance -= amount
```

---

**Seam**
A seam is a place in the code where behaviour can be changed without modifying the code at that point. Seams are the basis for making existing code testable: instead of calling a concrete dependency directly, the code is structured so a different implementation can be substituted at the seam, typically through dependency injection, an interface, or a configuration point.

```python
# No seam — the dependency is created internally; cannot be replaced in tests.
class OrderService:
    def place_order(self, order):
        mailer = SmtpMailer()  # hard-coded; no way to substitute
        mailer.send(order.confirmation_email())

# Object seam — the dependency is injected; a test double can be passed in.
class OrderService:
    def __init__(self, mailer: Mailer):
        self.mailer = mailer

    def place_order(self, order):
        self.mailer.send(order.confirmation_email())
```

A seam always has an *enabling point*: the place where you choose which behaviour to use. In the example above, the enabling point is the constructor parameter.

---

**Fan-in / Fan-out**
Two measures of how an element is connected to the rest of a system.

**Fan-in** counts how many other elements depend on a given element — how many callers, importers, or consumers point *at* it. High fan-in means the element is widely relied upon: a change to its contract ripples out to everything that depends on it. Shared libraries, core domain types, and central services tend to have high fan-in.

**Fan-out** counts how many other elements a given element depends on — how many things it calls, imports, or consumes. High fan-out means the element is sensitive to change *elsewhere*: it can break when any of its many dependencies changes, and it is harder to test and reason about in isolation.

```
        A   B   C          A depends on X        (X has fan-in 3)
         \  |  /
          \ | /
            X               X depends on P, Q     (X has fan-out 2)
           / \
          P   Q
```

As a rule of thumb, high fan-in elements should be kept stable, because many things depend on them; high fan-out elements should be kept small, because they depend on many things. Fan-in also feeds the notion of architectural significance: a decision with high fan-in affects a large part of the system, so it is more likely to be architectural than local (see [Design vs architecture](index.md#design-vs-architecture)).

---

**Composition Root**
The single place in an application where all dependencies are wired together. Instead of creating objects throughout the codebase, everything is assembled once at startup. This keeps dependency creation out of business logic and makes the structure of the application easy to see and change in one place.

```python
# Composition Root — runs once at startup
def main():
    db = PostgresDatabase(url=config.DB_URL)
    repo = UserRepository(db)
    service = UserService(repo)
    app.register(service)
```
