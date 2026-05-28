# Chain of Responsibility

The Chain of Responsibility pattern passes a request along a chain of handlers. Each handler decides whether to process the request itself or forward it to the next handler in the chain. The sender does not know which handler will ultimately handle the request.

## How it works

Each handler holds a reference to the next handler in the chain. When a request arrives, the handler checks whether it can process it. If it can, it handles it and optionally stops the chain. If it cannot, it forwards the request to the next handler. The chain can be assembled at runtime by linking handlers together.

## Example

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class SupportHandler(ABC):
    def __init__(self) -> None:
        self._next: SupportHandler | None = None

    def set_next(self, handler: SupportHandler) -> SupportHandler:
        self._next = handler
        return handler

    def handle(self, severity: int, message: str) -> str | None:
        if self._next:
            return self._next.handle(severity, message)
        return None


class FirstLineSupport(SupportHandler):
    def handle(self, severity: int, message: str) -> str | None:
        if severity <= 1:
            return f"First line resolved: {message}"
        return super().handle(severity, message)


class SecondLineSupport(SupportHandler):
    def handle(self, severity: int, message: str) -> str | None:
        if severity <= 3:
            return f"Second line resolved: {message}"
        return super().handle(severity, message)


class EngineeringTeam(SupportHandler):
    def handle(self, severity: int, message: str) -> str | None:
        return f"Engineering resolved: {message}"


first = FirstLineSupport()
second = SecondLineSupport()
engineering = EngineeringTeam()

first.set_next(second).set_next(engineering)

print(first.handle(1, "Password reset"))
# First line resolved: Password reset

print(first.handle(2, "API timeout"))
# Second line resolved: API timeout

print(first.handle(5, "Data corruption detected"))
# Engineering resolved: Data corruption detected
```

## When to use

- More than one object might handle a request and the handler is not known in advance.
- You want to issue a request without coupling it to a specific receiver. The chain can change independently of the sender.
- You are building a pipeline such as HTTP middleware, event bubbling, or an approval workflow where each step decides whether to act or pass on.

## When not to use

- Every request must be guaranteed to be handled. A chain with no default handler can silently drop requests.
- The chain grows so long that tracing which handler acted becomes difficult. In that case a registry or rule engine communicates intent more clearly.
- The handling decision depends on global context rather than the request itself. Chain of Responsibility assumes local, request-based decisions.
