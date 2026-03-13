# Command

The Command pattern encapsulates a request as an object. This lets you parameterize operations, queue them for later execution, log them, and support undo. The caller that triggers the command is decoupled from the object that carries it out.

## How it works

A command object holds all the information needed to perform an action: the receiver object, the method to call, and any required parameters. An invoker stores and executes command objects without knowing their internals. Each command can optionally implement an `undo` method that reverses its effect.

## Example

```python
from abc import ABC, abstractmethod


class Command(ABC):
    @abstractmethod
    def execute(self) -> None: ...

    @abstractmethod
    def undo(self) -> None: ...


class TextBuffer:
    def __init__(self) -> None:
        self.text = ""


class InsertCommand(Command):
    def __init__(self, buffer: TextBuffer, text: str) -> None:
        self._buffer = buffer
        self._text = text

    def execute(self) -> None:
        self._buffer.text += self._text

    def undo(self) -> None:
        self._buffer.text = self._buffer.text[: -len(self._text)]


class DeleteLastWordCommand(Command):
    def __init__(self, buffer: TextBuffer) -> None:
        self._buffer = buffer
        self._deleted = ""

    def execute(self) -> None:
        words = self._buffer.text.rstrip().rsplit(" ", 1)
        if len(words) == 2:
            self._deleted = " " + words[1]
            self._buffer.text = words[0]
        else:
            self._deleted = self._buffer.text
            self._buffer.text = ""

    def undo(self) -> None:
        self._buffer.text += self._deleted


class CommandHistory:
    def __init__(self) -> None:
        self._stack: list[Command] = []

    def execute(self, command: Command) -> None:
        command.execute()
        self._stack.append(command)

    def undo(self) -> None:
        if self._stack:
            self._stack.pop().undo()


buffer = TextBuffer()
history = CommandHistory()

history.execute(InsertCommand(buffer, "Hello World"))
print(buffer.text)  # Hello World

history.execute(DeleteLastWordCommand(buffer))
print(buffer.text)  # Hello

history.undo()
print(buffer.text)  # Hello World

history.undo()
print(buffer.text)  # (empty)
```

## When to use

- You need undo and redo functionality and want to represent each action as a reversible object.
- You want to queue, schedule, or log operations for later or deferred execution.
- Operations need to be composed into macros or batch jobs where individual steps can be replayed or rolled back independently.

## When not to use

- The operation is simple and one-off with no need for queuing, logging, or undo. A direct method call is clearer.
- The command objects start carrying business logic instead of just delegating to a receiver. Commands should orchestrate, not decide.
- The number of command types grows to mirror every method in the domain. At that point the pattern becomes boilerplate without added value.
