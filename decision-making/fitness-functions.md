# Fitness Functions

## What they are

A fitness function is a check that tells you whether your system still meets an architectural goal. It makes rules explicit and verifiable instead of relying on people to remember them.

Think of them like automated tests — but instead of checking whether your code produces the right output, they check whether your system still has the right structure or behavior.

The term comes from evolutionary biology, where a fitness function measures how well a species survives in its environment. In architecture, it measures how well a system holds up against a defined quality goal.

## Why they matter

Architecture tends to drift. A rule like "services must not share a database" is easy to agree on and easy to forget three months later. A fitness function catches the violation automatically, before it causes real problems.

Without them, architecture reviews become the only safety net — and reviews happen rarely.

## Types

**Automated** — run in a build or deployment pipeline without human involvement.

Examples:
- Check that no module imports from another module it should not depend on
- Check that the average API response time stays below 200ms
- Check that no new circular dependencies have been introduced

**Manual** — require a person to evaluate, usually on a schedule.

Examples:
- A quarterly review of whether service boundaries still match team structure
- A check that a new service added last month follows the agreed logging and naming conventions

**Scheduled** — automated but run on a timer, not on every change.

Examples:
- A weekly scan for outdated or vulnerable dependencies
- A monthly check of security certificates

## Examples

| Goal | Fitness function |
|---|---|
| No unwanted dependencies | Module A may not import from module C |
| Fast responses | 95% of requests complete within 200ms |
| High availability | Service uptime ≥ 99.9% per month |
| No exposed secrets | No service responds over an unencrypted connection |
| Clean dependencies | No circular dependencies in the component graph |

## How to start

1. Pick the one architectural property that matters most right now (e.g. "no circular dependencies").
2. Write a single automated check and run it on every commit.
3. Fix any existing violations.
4. Add more functions as new architectural rules are agreed on.

Do not try to cover everything at once. One working check that runs on every commit is worth more than ten manual checks that happen occasionally.

## Connection to ADRs

Fitness functions and [ADRs](../architect-role/adr.md) work well together. When you write down an architectural decision, ask: *can this be checked automatically?* If yes, add a fitness function. The ADR captures the why; the fitness function enforces the what.
