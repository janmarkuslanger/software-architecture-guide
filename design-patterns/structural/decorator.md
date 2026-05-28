# Decorator

The Decorator pattern attaches new behavior to an object by wrapping it in another object that shares the same interface. Decorators can be stacked: each one adds its behavior and passes the call through to the next layer. This is a flexible alternative to subclassing when you need to add responsibilities dynamically.

## How it works

The decorator implements the same interface as the object it wraps and holds a reference to it. When a method is called on the decorator, it can execute additional logic before or after forwarding the call to the wrapped object. Multiple decorators can be layered around the same object in any order.

## Example

```python
from abc import ABC, abstractmethod


class DataExporter(ABC):
    @abstractmethod
    def export(self, data: str) -> str: ...


class JSONExporter(DataExporter):
    def export(self, data: str) -> str:
        return f'{{"payload": "{data}"}}'


class CompressionDecorator(DataExporter):
    def __init__(self, exporter: DataExporter) -> None:
        self._exporter = exporter

    def export(self, data: str) -> str:
        result = self._exporter.export(data)
        return f"GZIP({result})"


class EncryptionDecorator(DataExporter):
    def __init__(self, exporter: DataExporter) -> None:
        self._exporter = exporter

    def export(self, data: str) -> str:
        result = self._exporter.export(data)
        return f"AES({result})"


class LoggingDecorator(DataExporter):
    def __init__(self, exporter: DataExporter) -> None:
        self._exporter = exporter

    def export(self, data: str) -> str:
        print(f"Exporting data of length {len(data)}...")
        result = self._exporter.export(data)
        print("Export complete.")
        return result


exporter = LoggingDecorator(
    EncryptionDecorator(
        CompressionDecorator(
            JSONExporter()
        )
    )
)

output = exporter.export("order-data")
print(output)
# Exporting data of length 10...
# Export complete.
# AES(GZIP({"payload": "order-data"}))
```

## When to use

- You need to add behavior to individual objects without affecting others of the same class.
- The behavior combinations are too numerous to express through subclasses. Stacking two or three decorators is more manageable than creating a subclass for every combination.
- You want to add and remove responsibilities at runtime rather than at compile time.

## When not to use

- The number of stacked decorators grows so large that the call chain becomes difficult to trace and debug.
- The order in which decorators are applied matters significantly and is easy to get wrong. A dedicated pipeline or middleware framework communicates order more explicitly.
- The wrapped object is a value object or a simple data structure. Wrapping simple things in multiple layers of decorators is usually over-engineering.
