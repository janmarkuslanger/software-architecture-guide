# Observer

The Observer pattern defines a one-to-many relationship between objects: when one object changes state, all its dependents are notified automatically. The object being watched is called the subject or publisher; the objects that react are called observers or subscribers.

## How it works

The subject maintains a list of observers and provides methods to subscribe and unsubscribe. When the subject's state changes, it calls a notification method on each registered observer. Observers implement a common interface so the subject does not need to know their concrete types. The subject and observers are loosely coupled. They only share the observer interface.

## Example

```python
from abc import ABC, abstractmethod


class StockObserver(ABC):
    @abstractmethod
    def on_price_change(self, symbol: str, price: float) -> None: ...


class StockTicker:
    def __init__(self, symbol: str) -> None:
        self._symbol = symbol
        self._price = 0.0
        self._observers: list[StockObserver] = []

    def subscribe(self, observer: StockObserver) -> None:
        self._observers.append(observer)

    def unsubscribe(self, observer: StockObserver) -> None:
        self._observers.remove(observer)

    def set_price(self, price: float) -> None:
        self._price = price
        for observer in self._observers:
            observer.on_price_change(self._symbol, price)


class PriceAlertObserver(StockObserver):
    def __init__(self, threshold: float) -> None:
        self._threshold = threshold

    def on_price_change(self, symbol: str, price: float) -> None:
        if price > self._threshold:
            print(f"[Alert] {symbol} exceeded {self._threshold:.2f}: now {price:.2f}")


class PortfolioTracker(StockObserver):
    def __init__(self, shares: int) -> None:
        self._shares = shares
        self._value = 0.0

    def on_price_change(self, symbol: str, price: float) -> None:
        self._value = self._shares * price
        print(f"[Portfolio] {symbol} portfolio value: {self._value:.2f} EUR")


ticker = StockTicker("ACME")
alert = PriceAlertObserver(threshold=100.0)
portfolio = PortfolioTracker(shares=50)

ticker.subscribe(alert)
ticker.subscribe(portfolio)

ticker.set_price(95.0)
# [Portfolio] ACME portfolio value: 4750.00 EUR

ticker.set_price(105.0)
# [Alert] ACME exceeded 100.00: now 105.00
# [Portfolio] ACME portfolio value: 5250.00 EUR

ticker.unsubscribe(alert)
ticker.set_price(110.0)
# [Portfolio] ACME portfolio value: 5500.00 EUR
```

## When to use

- A change in one object requires notifying an unknown number of other objects, and you do not want the subject to be coupled to them.
- You are building event systems, reactive UI bindings, or pub/sub mechanisms where the set of subscribers changes at runtime.
- You need a clean separation between the source of changes and the reactions to those changes.

## When not to use

- The update chain becomes long and recursive: an observer changes the subject, which notifies more observers, causing cascading updates that are hard to trace.
- The notification order matters for correctness. Observer lists do not guarantee a stable order, and relying on it creates fragile behavior.
