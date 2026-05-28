# Bridge

The Bridge pattern separates an abstraction from its implementation so that the two can evolve independently. Instead of building all combinations into a class hierarchy, you compose them at runtime by combining an abstraction object with an implementation object.

## How it works

The abstraction holds a reference to an implementation object through an interface. The abstraction defines the high-level behavior and delegates the implementation-specific work to that reference. You can swap implementations without touching the abstraction, and extend the abstraction without touching the implementations.

## Example

```python
from abc import ABC, abstractmethod


class MessageSender(ABC):
    @abstractmethod
    def send(self, recipient: str, content: str) -> None: ...


class EmailSender(MessageSender):
    def send(self, recipient: str, content: str) -> None:
        print(f"Email to {recipient}: {content}")


class SMSSender(MessageSender):
    def send(self, recipient: str, content: str) -> None:
        print(f"SMS to {recipient}: {content}")


class Notification(ABC):
    def __init__(self, sender: MessageSender) -> None:
        self._sender = sender

    @abstractmethod
    def notify(self, recipient: str) -> None: ...


class AlertNotification(Notification):
    def __init__(self, sender: MessageSender, alert_text: str) -> None:
        super().__init__(sender)
        self._alert_text = alert_text

    def notify(self, recipient: str) -> None:
        self._sender.send(recipient, f"ALERT: {self._alert_text}")


class ReminderNotification(Notification):
    def __init__(self, sender: MessageSender, reminder_text: str) -> None:
        super().__init__(sender)
        self._reminder_text = reminder_text

    def notify(self, recipient: str) -> None:
        self._sender.send(recipient, f"Reminder: {self._reminder_text}")


alert_via_email = AlertNotification(EmailSender(), "Server CPU above 90%")
alert_via_email.notify("ops-team@example.com")

reminder_via_sms = ReminderNotification(SMSSender(), "Meeting in 10 minutes")
reminder_via_sms.notify("+49123456789")
```

## When to use

- You have two independent dimensions of variation (for example, abstraction type and implementation type) and combining them via inheritance would produce an exponentially growing number of subclasses.
- You want to switch implementations at runtime rather than at compile time.
- You are extending a framework and need to provide implementation-specific behavior without modifying the core abstraction.

## When not to use

- There is only one implementation and no realistic prospect of adding others. The extra indirection adds complexity without payoff.
- The abstraction and implementation genuinely cannot vary independently. Separating them would produce an artificial split that makes the code harder to follow.
- A simpler solution such as dependency injection of a strategy object solves the problem with less ceremony.
