# Mediator

The Mediator pattern defines a central object that encapsulates how a set of other objects communicate. Instead of objects referencing each other directly, they all talk through the mediator. This reduces the dependencies between objects from a many-to-many mesh to a many-to-one relationship with the mediator.

## How it works

Each participating object holds a reference to the mediator instead of holding references to its peers. When something happens that other objects should know about, the object notifies the mediator. The mediator then decides which other objects to inform and what to do with the information. The participants are decoupled from each other — they only know the mediator.

## Example

A booking form with three controls: a "collect at store" checkbox, an address field, and a submit button. The controls don't reference each other — they all notify the mediator, which holds the coordination logic.

```python
from __future__ import annotations


class BookingForm:
    """Mediator: owns the coordination rules."""

    def __init__(self) -> None:
        self.collect_at_store = Checkbox(self)
        self.address = TextInput(self)
        self.submit = Button(self)

    def notify(self, sender: object) -> None:
        if sender is self.collect_at_store:
            self.address.enabled = not self.collect_at_store.checked
        address_ok = self.collect_at_store.checked or bool(self.address.value)
        self.submit.enabled = address_ok


class Checkbox:
    def __init__(self, form: BookingForm) -> None:
        self._form = form
        self.checked = False

    def toggle(self) -> None:
        self.checked = not self.checked
        self._form.notify(self)


class TextInput:
    def __init__(self, form: BookingForm) -> None:
        self._form = form
        self.value = ""
        self.enabled = True

    def type(self, text: str) -> None:
        self.value = text
        self._form.notify(self)


class Button:
    def __init__(self, form: "BookingForm") -> None:
        self._form = form
        self.enabled = False


form = BookingForm()

form.address.type("Baker Street 1")
assert form.submit.enabled        # address filled → submit active

form.collect_at_store.toggle()
assert not form.address.enabled   # pickup selected → address hidden
assert form.submit.enabled        # store pickup needs no address

form.collect_at_store.toggle()
assert form.address.enabled       # address visible again
assert form.submit.enabled        # value still set → submit stays active
```


## When to use

- A set of objects communicate in complex ways that produce hard-to-maintain dependencies between them. Centralizing the communication in a mediator makes the relationships explicit.
- You want to reuse components in different contexts without carrying all their peer dependencies along.

## When not to use

- Only two objects interact. A direct reference is simpler and communicates the relationship more clearly.
- The mediator grows to know everything about every participant and becomes a "god object". When that happens, the mediator itself becomes the coupling problem.
- The communication patterns are simple and stable. Introducing a mediator for straightforward interactions adds indirection without value.
