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
Contention occurs when multiple threads, processes, or services compete for the same shared resource at the same time — such as a CPU, a lock, or a database connection. The resource can only serve one at a time, so others have to wait. This waiting is a common cause of latency and poor performance under load.

A typical example is lock contention: two threads both want to write to the same data structure. One acquires the lock and proceeds, the other blocks until the lock is released.
```python
# Both threads call this function concurrently.
# Only one can hold the lock at a time — the other waits.
def transfer(amount):
    with account_lock:
        account.balance -= amount
```

---

**Saga**
A way to manage a multi-step operation that spans several services. Instead of one big transaction, a saga breaks the operation into smaller steps. Each step is handled by a different service. If a step fails, the saga runs compensating actions to undo the steps that already completed.

There are two ways to coordinate a saga: choreography (each service reacts to events independently) and orchestration (a central coordinator directs each step). See [Event-Driven Architecture](../architecture-patterns/event-driven.md#saga-managing-multi-step-workflows).

---

**Compensating action**
An action that undoes a previous step when something goes wrong in a multi-step workflow. For example: if "charge the customer" succeeded but "schedule delivery" failed, the compensating action is "refund the customer". Compensating actions are the main way to handle failure in a saga.

---

**Strangler Fig**
A pattern for replacing parts of an existing system gradually, without a full rewrite. Named after the strangler fig tree, which grows around another tree and eventually replaces it. A routing layer is placed in front of the old system; new parts are built alongside the old ones; traffic is moved piece by piece until the old system is no longer needed. See [Breaking a System Apart](../architecture-patterns/decomposition.md#strangler-fig).

---

**Choreography**
A way of coordinating work across services where each service reacts to events on its own. No central coordinator tells services what to do — they listen for events and decide independently. Works well for simple flows; harder to follow when flows become complex.

---

**Orchestration**
A way of coordinating work across services where a central coordinator sends instructions and tracks progress. The coordinator knows all the steps and handles failures. Easier to follow than choreography; the coordinator becomes a central point of coupling.

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
