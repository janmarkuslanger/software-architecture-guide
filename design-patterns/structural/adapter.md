# Adapter

The Adapter pattern converts the interface of one class into the interface that a client expects. It acts as a translator between two incompatible interfaces without changing the code of either side. This is particularly useful when integrating third-party libraries or legacy systems.

## How it works

The adapter wraps the incompatible object and implements the interface the client works with. When the client calls a method on the adapter, the adapter translates that call into whatever the wrapped object actually provides. The client never interacts with the wrapped object directly.

## Example

```python
class LegacyPaymentGateway:
    def make_payment(self, amount_cents: int, currency_code: str) -> dict:
        return {
            "status": "OK",
            "paid": amount_cents,
            "currency": currency_code,
        }


class PaymentProcessor:
    def charge(self, amount: float, currency: str) -> bool:
        raise NotImplementedError


class LegacyGatewayAdapter(PaymentProcessor):
    def __init__(self, gateway: LegacyPaymentGateway) -> None:
        self._gateway = gateway

    def charge(self, amount: float, currency: str) -> bool:
        amount_cents = int(amount * 100)
        result = self._gateway.make_payment(amount_cents, currency)
        return result["status"] == "OK"


def process_order(processor: PaymentProcessor, total: float) -> None:
    success = processor.charge(total, "EUR")
    print(f"Payment {'succeeded' if success else 'failed'}")


legacy = LegacyPaymentGateway()
adapter = LegacyGatewayAdapter(legacy)
process_order(adapter, 49.99)
# Payment succeeded
```

## When to use

- You want to use an existing class but its interface does not match what your code expects, and you cannot or do not want to change the existing class.
- You are integrating a third-party library and want to shield your application from its specific interface so you can replace it later.
- You are migrating from a legacy system gradually and need both old and new implementations to coexist behind the same interface.

## When not to use

- The incompatibility is small enough that a direct wrapper function or a one-line translation handles it without a full class.
- You control both sides of the interface. In that case, changing one of them directly is simpler than adding an adapter layer.
- The adapter starts accumulating business logic. An adapter should only translate. If logic creeps in, the design needs revisiting.
