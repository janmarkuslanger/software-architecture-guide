# Factory Method

The Factory Method pattern defines an interface for creating an object but lets subclasses decide which concrete class to instantiate. The creator class contains the logic that uses the object; the decision of which object to create is delegated to subclasses.

## How it works

A base class declares an abstract factory method that returns a product object. Each subclass overrides this method and returns a specific product type. The rest of the base class works against the product interface and never needs to know the concrete type.

## Example

A logistics platform needs to plan deliveries. The planning logic (route validation, cost estimation) is the same regardless of transport type — but which transport gets created differs per subclass.

```python
from abc import ABC, abstractmethod


# Product interface
class Transport(ABC):
    @abstractmethod
    def deliver(self, destination: str) -> None: ...


# Concrete products
class Truck(Transport):
    def deliver(self, destination: str) -> None:
        print(f"Truck driving to {destination}")


class Ship(Transport):
    def deliver(self, destination: str) -> None:
        print(f"Ship sailing to {destination}")


# Creator — contains the business logic, delegates object creation
class Logistics(ABC):
    @abstractmethod
    def create_transport(self) -> Transport: ...

    def plan_delivery(self, destination: str) -> None:
        # Business logic lives here, independent of transport type
        print(f"Planning delivery to {destination}...")
        transport = self.create_transport()  # <-- subclass decides what gets created
        transport.deliver(destination)


# Concrete creators — only differ in which Transport they instantiate
class RoadLogistics(Logistics):
    def create_transport(self) -> Transport:
        return Truck()


class SeaLogistics(Logistics):
    def create_transport(self) -> Transport:
        return Ship()


# Client code works against the Logistics interface
logistics: Logistics = SeaLogistics()
logistics.plan_delivery("Rotterdam")
# Planning delivery to Rotterdam...
# Ship sailing to Rotterdam
```

The key insight: `plan_delivery` is written once in the base class and never changes. Adding a new transport type (e.g. `AirLogistics`) only requires a new subclass — no existing code is touched.

## Simple Factory vs. Factory Method

If you only need to choose between types based on a parameter, a plain factory function is enough:

```python
def create_transport(mode: str) -> Transport:
    if mode == "road":
        return Truck()
    elif mode == "sea":
        return Ship()
    raise ValueError(f"Unknown mode: {mode}")
```

This is simpler and more direct — but adding a new type requires changing this function. Factory Method is worth the indirection only when real extensibility is needed: when subclasses or external consumers should be able to add new types without touching existing code.

## When to use

- The type of object to create is not known at design time and should be determined by subclasses or configuration.
- You want to centralize the creation logic for a product type so it can be changed in one place.

## When not to use

- There is only ever one concrete product and no extension point is needed. A simple constructor call is clearer.
- The variation between products is minor enough that a single class with a parameter handles it just as well.