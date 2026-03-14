# Strategy

The Strategy pattern defines a family of algorithms, encapsulates each one in its own class, and makes them interchangeable. The object that uses the algorithm holds a reference to a strategy interface and delegates the work to it. The strategy can be swapped at runtime without changing the object that uses it.

## How it works

A strategy interface declares the operation that all concrete strategies must implement. Each concrete strategy provides a different implementation of that operation. A context object holds a reference to the current strategy and calls it when needed. Changing the strategy changes the behavior of the context without touching its own code.

## Example

```python
from abc import ABC, abstractmethod


class PricingStrategy(ABC):
    @abstractmethod
    def calculate(self, base_price: float, quantity: int) -> float: ...


class StandardPricing(PricingStrategy):
    def calculate(self, base_price: float, quantity: int) -> float:
        return base_price * quantity


class BulkDiscountPricing(PricingStrategy):
    def __init__(self, threshold: int, discount: float) -> None:
        self._threshold = threshold
        self._discount = discount

    def calculate(self, base_price: float, quantity: int) -> float:
        if quantity >= self._threshold:
            return base_price * quantity * (1 - self._discount)
        return base_price * quantity


class SubscriberPricing(PricingStrategy):
    def __init__(self, monthly_fee: float) -> None:
        self._monthly_fee = monthly_fee

    def calculate(self, base_price: float, quantity: int) -> float:
        unit_price = base_price * 0.8
        return self._monthly_fee + unit_price * quantity


class OrderPricer:
    def __init__(self, strategy: PricingStrategy) -> None:
        self._strategy = strategy

    def set_strategy(self, strategy: PricingStrategy) -> None:
        self._strategy = strategy

    def total(self, base_price: float, quantity: int) -> float:
        return self._strategy.calculate(base_price, quantity)


pricer = OrderPricer(StandardPricing())
print(f"Standard: {pricer.total(10.0, 5):.2f} EUR")
# Standard: 50.00 EUR

pricer.set_strategy(BulkDiscountPricing(threshold=10, discount=0.15))
print(f"Bulk (4 units):  {pricer.total(10.0, 4):.2f} EUR")
# Bulk (4 units):  40.00 EUR

print(f"Bulk (12 units): {pricer.total(10.0, 12):.2f} EUR")
# Bulk (12 units): 102.00 EUR

pricer.set_strategy(SubscriberPricing(monthly_fee=9.99))
print(f"Subscriber (5 units): {pricer.total(10.0, 5):.2f} EUR")
# Subscriber (5 units): 49.99 EUR
```

## When to use

- You have multiple variants of an algorithm and want to switch between them at runtime without conditional branching in the caller.
- You want to isolate the implementation details of an algorithm from the code that uses it so each can change independently.
- A class has a large conditional block that selects different behavior based on a type or mode. Replacing it with a strategy for each variant makes the code easier to extend.

## When not to use

- There is only one algorithm and no realistic prospect of adding others. Introducing a strategy interface for a single implementation adds abstraction without benefit.
- The algorithms are trivial one-liners. Wrapping them in classes adds ceremony that outweighs the value of the pattern.
- Clients must be aware of all available strategies to choose the right one. If the selection logic is complex, it belongs in a factory or configuration layer, not scattered at each call site.
