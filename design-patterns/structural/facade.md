# Facade

The Facade pattern provides a simple, unified interface to a complex subsystem. Clients talk to the facade instead of coordinating multiple subsystem classes directly. This reduces the surface area that consumers need to understand and hides internal complexity behind a clean boundary.

## How it works

The facade class holds references to the subsystem objects it covers. Its methods orchestrate calls to those objects in the right sequence and return a result. The subsystem objects still exist and can be accessed directly when needed, but the facade covers the common cases so most callers never need to go deeper.

## Example

```python
class InventoryService:
    def reserve(self, product_id: str, quantity: int) -> bool:
        print(f"Reserved {quantity}x {product_id}")
        return True


class PaymentService:
    def charge(self, customer_id: str, amount: float) -> str:
        print(f"Charged {amount:.2f} EUR to customer {customer_id}")
        return "txn-001"


class ShippingService:
    def schedule(self, product_id: str, address: str) -> str:
        print(f"Scheduled shipment of {product_id} to {address}")
        return "ship-001"


class NotificationService:
    def send_confirmation(self, customer_id: str, transaction_id: str) -> None:
        print(f"Confirmation sent to {customer_id} for {transaction_id}")


class OrderFacade:
    def __init__(self) -> None:
        self._inventory = InventoryService()
        self._payment = PaymentService()
        self._shipping = ShippingService()
        self._notifications = NotificationService()

    def place_order(
        self,
        customer_id: str,
        product_id: str,
        quantity: int,
        amount: float,
        address: str,
    ) -> bool:
        if not self._inventory.reserve(product_id, quantity):
            print("Order failed: product not available.")
            return False
        transaction_id = self._payment.charge(customer_id, amount)
        self._shipping.schedule(product_id, address)
        self._notifications.send_confirmation(customer_id, transaction_id)
        return True


facade = OrderFacade()
facade.place_order("cust-42", "prod-7", 2, 59.98, "Berlin, Germany")
# Reserved 2x prod-7
# Charged 59.98 EUR to customer cust-42
# Scheduled shipment of prod-7 to Berlin, Germany
# Confirmation sent to cust-42 for txn-001
```

## When to use

- You want to provide a simple entry point into a complex or layered subsystem for the most common use cases.
- You are designing a public API or SDK and want to hide internal implementation details so you can change them later without breaking callers.
- Multiple clients repeat the same sequence of subsystem calls. Centralizing that sequence in a facade reduces duplication and keeps the orchestration in one place.

## When not to use

- The subsystem is already simple. Adding a facade layer over a small number of methods introduces indirection without clarity.
- Clients need fine-grained control over subsystem internals that the facade does not expose. In that case, the facade becomes a bottleneck rather than a convenience.
- The facade starts accumulating business logic. A facade should orchestrate, not decide. If it grows decision-making responsibilities, it needs to be split up.
