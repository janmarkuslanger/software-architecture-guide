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
