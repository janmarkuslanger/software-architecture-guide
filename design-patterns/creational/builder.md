# Builder

The Builder pattern constructs a complex object step by step. Instead of a constructor that takes many parameters, you call a series of methods to configure the object and then request the final result. Each method returns the builder itself so the calls can be chained.

## How it works

A builder class holds the object under construction as internal state. Each configuration method updates one aspect of that state and returns `self`. A final `build` method assembles and returns the completed object. The builder can enforce rules, set defaults, and validate the configuration before handing over the result.

## Example

```python
class EmailMessage:
    def __init__(
        self,
        to: str,
        subject: str,
        body: str,
        cc: list[str],
        reply_to: str | None,
    ) -> None:
        self.to = to
        self.subject = subject
        self.body = body
        self.cc = cc
        self.reply_to = reply_to

    def __repr__(self) -> str:
        return (
            f"EmailMessage(to={self.to!r}, subject={self.subject!r}, "
            f"cc={self.cc}, reply_to={self.reply_to!r})"
        )


class EmailBuilder:
    def __init__(self, to: str, subject: str) -> None:
        self._to = to
        self._subject = subject
        self._body = ""
        self._cc: list[str] = []
        self._reply_to: str | None = None

    def body(self, text: str) -> "EmailBuilder":
        self._body = text
        return self

    def cc(self, address: str) -> "EmailBuilder":
        self._cc.append(address)
        return self

    def reply_to(self, address: str) -> "EmailBuilder":
        self._reply_to = address
        return self

    def build(self) -> EmailMessage:
        if not self._body:
            raise ValueError("Email body cannot be empty.")
        return EmailMessage(
            to=self._to,
            subject=self._subject,
            body=self._body,
            cc=list(self._cc),
            reply_to=self._reply_to,
        )


message = (
    EmailBuilder("alice@example.com", "Monthly Report")
    .body("Please find the report attached.")
    .cc("manager@example.com")
    .reply_to("noreply@example.com")
    .build()
)

print(message)
# EmailMessage(to='alice@example.com', subject='Monthly Report',
#              cc=['manager@example.com'], reply_to='noreply@example.com')
```

## When to use

- An object requires many parameters to be constructed and most of them are optional or have sensible defaults.
- The construction process involves validation or computation steps that should not live inside the constructor.
- You want a readable, fluent API that makes the intent of each configuration step clear at the call site.

## When not to use

- The object is simple and has only a few required parameters. A plain constructor is more direct and easier to understand.
- The builder grows to mirror a complex domain object one-to-one without adding clarity. In that case the duplication becomes a maintenance burden.
- Immutability is not important and the object can be configured after construction through simple property assignment.
