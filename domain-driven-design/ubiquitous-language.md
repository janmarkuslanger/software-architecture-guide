# Ubiquitous Language

## Overview
The ubiquitous language is the shared vocabulary used inside a [bounded context](strategic-design.md) by everyone working on it: domain experts, product, engineering, and the code itself. The same term carries the same meaning across conversations, documents, tests, and source code.

It is *ubiquitous* in the sense that it pervades every artefact of the context. It is not jargon imposed on the business by developers, nor is it business language that engineers translate away. It is one language, used everywhere within the context.

## Why language matters in design
Software encodes business rules. When the words used to describe those rules differ between the people who define them and the people who implement them, translation happens at every handover. Translation is lossy. Over time, translation cost accumulates as defects, misunderstandings, and refactors.

Three concrete effects:

- **Defects.** A developer implements *Order* as everything from cart to delivery. Sales meant *confirmed Order* only. The bug is not in the code; it is in the language.
- **Refactor cost.** A class named *Manager* or *Processor* tells a future reader nothing. A class named *ShippingAddressValidator* tells them exactly what changes when a shipping rule changes.
- **Onboarding.** New team members learn the domain by reading the code. If the code uses different words than the business, they learn twice.

The ubiquitous language reduces these costs by making the model and the conversation use the same words.

## Scope: per bounded context
Each bounded context has its own ubiquitous language. The same word can mean different things in different contexts, and that is deliberate.

| Term | Sales context | Billing context | Support context |
|---|---|---|---|
| Customer | Prospect or lead | Invoice recipient with payment method | Account with open tickets |
| Order | Quote or draft | Confirmed line items to bill | Reference number for a complaint |

A canonical *Customer* model that satisfies all three contexts ends up satisfying none well. Per-context language is what makes context boundaries useful.

## What the language covers
The ubiquitous language is not only nouns. It includes:

- **Entities and value objects.** *Order*, *Address*, *Money*.
- **Operations and verbs.** *PlaceOrder*, *ShipOrder*, *RefundPayment*.
- **States.** *Draft*, *Confirmed*, *Cancelled*, *Shipped*.
- **Events.** *OrderPlaced*, *PaymentFailed*.
- **Rules.** *An Order can only be cancelled before it ships.*

Each of these appears in conversation, in documentation, and in code with the same name.

## How the language shows up in code
The language is visible at every level of the model.

```python
class Order:
    def place(self, items: list[LineItem], customer: Customer) -> None: ...
    def confirm(self) -> None: ...
    def cancel(self, reason: CancellationReason) -> None: ...
    def ship(self, address: ShippingAddress) -> None: ...

class OrderPlaced(DomainEvent): ...
class OrderCancelled(DomainEvent): ...
```

The names of the class, methods, parameters, and events are the same words used in the business conversation. A domain expert reading this code can recognise the workflow even without programming knowledge.

Compare to a model where the language is missing:

```python
class OrderManager:
    def process(self, data: dict) -> dict: ...
    def update(self, order_id: int, status: str) -> None: ...
```

Nothing here tells a reader what the domain rules are. The terms *process*, *update*, *status* are technical, not domain.

## How the language develops
Ubiquitous language is not handed down. It emerges from conversation between domain experts and the team.

- **Listen for friction.** When two people consistently use different words for what seems like the same concept, either they mean different things (a hidden distinction worth naming), or one of the words is unclear.
- **Capture and reuse.** Once a term is agreed on, it goes into the code, the tests, the documentation, and the conversation. Do not let the code drift to a synonym.
- **Refine on contradiction.** When the language conflicts with the model — when a rule cannot be expressed cleanly in the chosen words — that is a signal to change the model or the words, not to translate around them.
- **Reject vague terms.** *Process*, *handle*, *data*, *info*, *manager* are signals that the language has not yet been worked out for that part of the domain.

[Event Storming](event-storming.md) is one structured way to surface and align language across the team.

## Common pitfalls
- **Developer-only language.** The team agrees on terminology among themselves; domain experts use different words. The language is no longer ubiquitous, only internal.
- **One language across all contexts.** Forcing the same word to mean the same thing everywhere defeats the point of bounded contexts and pushes back toward a canonical model.
- **Language frozen in time.** Business changes. If the language does not evolve with it, the model drifts from reality.
- **Translation layer at the wrong place.** Translating between contexts is normal and explicit (see [Context Maps](context-maps.md)). Translating *within* a context — code uses one word, business uses another — is the problem.
- **Vague technical terms.** *Handler*, *Service*, *Manager*, *Processor*, *Util*: usually signs that the underlying domain concept has not been named.
- **Legacy names left untouched.** Old code keeps old, mismatched names because renaming is "not the task". The mismatch compounds; what could have been a small refactor becomes a costly one.
- **Treating inconsistent business terms as a blocker.** The business itself often uses several words for the same concept. Reconciling them through conversation is part of the modelling work, not a reason to stop.
