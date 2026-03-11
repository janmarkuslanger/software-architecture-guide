# Prototype

The Prototype pattern creates new objects by cloning an existing one. Instead of constructing an object from scratch, you copy a configured prototype and then adjust only what needs to differ. This is useful when object creation is expensive or when you want to start from a known baseline.

## How it works

The prototype object exposes a `clone` method that returns a copy of itself. Depending on the object's structure, this is either a shallow copy (references are shared) or a deep copy (the entire object graph is duplicated). The caller receives a new, independent object that starts out identical to the prototype.

## Example

```python
import copy


class ReportTemplate:
    def __init__(
        self,
        title: str,
        sections: list[str],
        footer: str,
        confidential: bool = False,
    ) -> None:
        self.title = title
        self.sections = sections
        self.footer = footer
        self.confidential = confidential

    def clone(self) -> "ReportTemplate":
        return copy.deepcopy(self)

    def __repr__(self) -> str:
        return (
            f"ReportTemplate(title={self.title!r}, "
            f"sections={self.sections}, confidential={self.confidential})"
        )


base_template = ReportTemplate(
    title="Monthly Report",
    sections=["Executive Summary", "Financials"],
    footer="Company Confidential",
    confidential=True,
)

q1_report = base_template.clone()
q1_report.title = "Q1 2026 Report"
q1_report.sections.append("Forecast")

print(base_template)
# ReportTemplate(title='Monthly Report', sections=['Executive Summary', 'Financials'],
#                confidential=True)

print(q1_report)
# ReportTemplate(title='Q1 2026 Report',
#                sections=['Executive Summary', 'Financials', 'Forecast'],
#                confidential=True)
```

## When to use

- Creating an object from scratch is expensive (for example, it requires database calls or heavy computation) and you can amortize that cost by cloning a pre-built instance.
- You need many slightly different variations of an object and want to start each one from a known, validated baseline rather than configuring it from zero.
- The concrete class of the object should remain hidden from the client that creates it.

## When not to use

- The object graph is simple enough that a plain constructor call is cheaper and clearer than maintaining a prototype.
- Deep-copying the object is problematic because it holds resources such as file handles, database connections, or external references that cannot or should not be duplicated.
- The variations between instances are so large that starting from a shared prototype saves little effort compared to constructing each one directly.
