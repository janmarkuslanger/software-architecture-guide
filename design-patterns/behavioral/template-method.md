# Template Method

The Template Method pattern defines the skeleton of an algorithm in a base class and lets subclasses fill in specific steps without changing the overall structure. The base class calls the steps in the right order; subclasses override only the parts that need to vary.

## How it works

A base class implements the template method, which calls a series of steps in a fixed sequence. Some steps are fully implemented in the base class (shared behavior). Others are declared abstract and must be implemented by subclasses (variable behavior). Optionally, some steps are given a default implementation that subclasses can override if needed.

## Example

```python
from abc import ABC, abstractmethod


class DataPipeline(ABC):
    def run(self) -> None:
        raw = self.extract()
        transformed = self.transform(raw)
        validated = self.validate(transformed)
        if validated:
            self.load(transformed)
            self.on_success()
        else:
            self.on_failure()

    @abstractmethod
    def extract(self) -> list[dict]: ...

    @abstractmethod
    def transform(self, records: list[dict]) -> list[dict]: ...

    def validate(self, records: list[dict]) -> bool:
        return all("id" in r for r in records)

    @abstractmethod
    def load(self, records: list[dict]) -> None: ...

    def on_success(self) -> None:
        print("Pipeline completed successfully.")

    def on_failure(self) -> None:
        print("Pipeline failed: validation error.")


class OrderPipeline(DataPipeline):
    def extract(self) -> list[dict]:
        print("Extracting orders from API...")
        return [{"id": 1, "amount": 99.0}, {"id": 2, "amount": 149.0}]

    def transform(self, records: list[dict]) -> list[dict]:
        return [
            {**r, "amount_cents": int(r["amount"] * 100)}
            for r in records
        ]

    def load(self, records: list[dict]) -> None:
        print(f"Inserting {len(records)} orders into the database.")


class UserPipeline(DataPipeline):
    def extract(self) -> list[dict]:
        print("Extracting users from CSV...")
        return [{"id": 10, "name": "Alice"}, {"name": "Bob"}]

    def transform(self, records: list[dict]) -> list[dict]:
        return [r for r in records if "id" in r]

    def load(self, records: list[dict]) -> None:
        print(f"Saving {len(records)} users.")

    def on_failure(self) -> None:
        print("User pipeline failed. Check source file.")


OrderPipeline().run()
# Extracting orders from API...
# Inserting 2 orders into the database.
# Pipeline completed successfully.

UserPipeline().run()
# Extracting users from CSV...
# User pipeline failed. Check source file.
```

## When to use

- Multiple classes share the same overall algorithm but differ in specific steps. Centralizing the structure in a base class removes duplication and ensures all variants follow the same sequence.
- You are building a framework where you want to define an invariant process and let consumers plug in their own implementations for specific steps.
- You want to control which steps can be overridden and which must stay fixed, by marking some methods final and others abstract.

## When not to use

- The algorithm varies so much between subclasses that the base class ends up nearly empty or the shared steps become meaningless. A strategy is more appropriate when the entire algorithm needs to be replaceable.
- You need to combine behaviors from multiple independent sources. Inheritance is single-chain; for mixable behaviors, composition or decorators work better.
- The template method grows so many optional hook methods that subclasses have to override empty stubs to do nothing. That signals the algorithm has too many variation points to be expressed cleanly through inheritance.
