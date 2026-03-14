# State

The State pattern allows an object to change its behavior when its internal state changes. Rather than scattering `if/elif` checks for each possible state throughout the code, the behavior for each state is encapsulated in a separate class. The object delegates work to its current state object and swaps it when transitioning.

## How it works

A context object holds a reference to the current state object. All state-dependent operations are forwarded to that object. Each concrete state class implements the operations for its state and is responsible for triggering transitions to other states by replacing the context's state reference.

## Example

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class OrderState(ABC):
    @abstractmethod
    def pay(self, order: Order) -> None: ...

    @abstractmethod
    def ship(self, order: Order) -> None: ...

    @abstractmethod
    def cancel(self, order: Order) -> None: ...

    def __str__(self) -> str:
        return type(self).__name__


class PendingState(OrderState):
    def pay(self, order: Order) -> None:
        print("Payment received.")
        order.state = ConfirmedState()

    def ship(self, order: Order) -> None:
        print("Cannot ship: payment required first.")

    def cancel(self, order: Order) -> None:
        print("Order cancelled.")
        order.state = CancelledState()


class ConfirmedState(OrderState):
    def pay(self, order: Order) -> None:
        print("Order is already paid.")

    def ship(self, order: Order) -> None:
        print("Order shipped.")
        order.state = ShippedState()

    def cancel(self, order: Order) -> None:
        print("Order cancelled after payment. Refund initiated.")
        order.state = CancelledState()


class ShippedState(OrderState):
    def pay(self, order: Order) -> None:
        print("Order is already paid.")

    def ship(self, order: Order) -> None:
        print("Order is already shipped.")

    def cancel(self, order: Order) -> None:
        print("Cannot cancel: order already shipped.")


class CancelledState(OrderState):
    def pay(self, order: Order) -> None:
        print("Order is cancelled.")

    def ship(self, order: Order) -> None:
        print("Order is cancelled.")

    def cancel(self, order: Order) -> None:
        print("Order is already cancelled.")


class Order:
    def __init__(self, order_id: str) -> None:
        self.order_id = order_id
        self.state: OrderState = PendingState()

    def pay(self) -> None:
        self.state.pay(self)

    def ship(self) -> None:
        self.state.ship(self)

    def cancel(self) -> None:
        self.state.cancel(self)

    def status(self) -> str:
        return str(self.state)


order = Order("ORD-001")
print(order.status())  # PendingState

order.ship()    # Cannot ship: payment required first.
order.pay()     # Payment received.
order.ship()    # Order shipped.
order.cancel()  # Cannot cancel: order already shipped.
print(order.status())  # ShippedState
```

## When to use

- An object's behavior depends on its state and must change at runtime, and the number of states is more than two or three.
- Operations contain large conditional blocks that switch on the object's state. Replacing those blocks with polymorphism makes each state's logic self-contained and easier to change.
- State transitions follow explicit rules and you want those rules to live close to the state definitions, not scattered across the codebase.

## When not to use

- The object only has two states and the transitions are simple. A boolean flag and two branches are more direct than a full class hierarchy.
- The states share very little behavior and each state class ends up nearly empty. Thin state classes add overhead without organizing anything meaningful.
- State is determined entirely by external data at the point of use. In that case a strategy passed in from the outside is a cleaner fit.
