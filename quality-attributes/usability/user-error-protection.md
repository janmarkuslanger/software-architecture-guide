# User Error Protection

## Overview

User error protection is the degree to which a system protects users against making errors. This includes preventing errors before they occur (validation, constraints, confirmation), reducing the impact of errors when they do occur (reversibility, undo), and helping users recover (clear error messages with actionable guidance).

Error protection is an architectural concern because it requires deliberate design of validation logic, state management for reversibility, and error communication interfaces.

---

## Core concepts

### Error prevention vs. error recovery

A commonly effective form of user error protection is prevention: making invalid states impossible or requiring explicit confirmation for destructive actions. Recovery is the fallback: when errors occur, the system helps users return to a correct state with minimal loss.

Prevention mechanisms: input constraints, disabled controls for unavailable actions, confirmation dialogs for irreversible operations, preview before commit.

Recovery mechanisms: undo, soft deletion, version history, clear error messages with correction guidance.

### Validation feedback timing

Immediate validation (inline, as users type) catches errors earlier and with less context switching than post-submission validation. However, premature validation (before the user has finished entering input) can create friction and is generally counterproductive.

A common approach: validate on field exit (blur), not on every keystroke. Show errors at the field level, not only as a summary at the top of the form.

### Destructive action confirmation

Irreversible or high-impact operations (deletion, bulk updates, payments, sending communications) generally warrant explicit confirmation. The confirmation should communicate what will happen and be designed so it cannot be dismissed by accident.

---

## Trade-offs

| Decision | Benefit | Risk |
|---|---|---|
| Soft deletion (mark as deleted, retain data) | Full recoverability; audit trail | Increases storage; requires purging strategy |
| Confirmation dialogs for destructive actions | Prevents accidental irreversible actions | Confirmation fatigue if overused; users learn to dismiss without reading |
| Client-side validation | Immediate feedback; reduces server round-trips | Must be duplicated server-side; cannot be the only validation layer |
| Optimistic UI with rollback | Fast perceived response; recoverable on failure | Requires rollback logic; user sees inconsistent state on failure |

---

## Common pitfalls

- **Server-side validation only**: errors are reported only after a round-trip, increasing friction. Client-side validation is generally recommended for common cases (but not as a substitute for server validation).
- **Generic error messages**: "An error occurred" provides little actionable information. Errors that identify what went wrong and what the user can do to correct it are generally more effective.
- **Irreversible operations without confirmation**: bulk deletes, account terminations, and data purges that execute immediately on a single click can lead to user errors with serious consequences.
- **Validation that blocks rather than guides**: forms that refuse submission without telling users what to fix, or that clear all input on error, can increase user friction. Preserving input and highlighting what needs correction is generally preferable.
