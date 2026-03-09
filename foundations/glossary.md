# Glossary

Common terms used throughout this guide.

---

**Indirection**
A layer in between two components so they don't depend on each other directly. Makes the system more flexible, but adds complexity.

Without indirection — the caller depends directly on the concrete class:
```python
report = PDFReport()
report.generate()
```

With indirection — the caller depends on an interface, the concrete class is resolved elsewhere:
```python
def generate_report(report: Report) -> None:
    report.generate()
```

Adding a new report format (`HTMLReport`, `CSVReport`) requires no change to `generate_report`.
