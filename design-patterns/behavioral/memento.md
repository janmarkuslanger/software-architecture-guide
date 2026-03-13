# Memento

The Memento pattern captures and stores the internal state of an object so it can be restored later, without exposing the object's internals to the outside world. It is the mechanism behind undo, checkpoints, and state snapshots.

## How it works

Three roles are involved. The originator is the object whose state needs to be saved. It creates a memento containing a snapshot of its current state and can restore itself from a memento. The memento is a simple value object that holds the saved state and exposes nothing. The caretaker stores mementos and decides when to save or restore them, but never inspects their contents.

## Example

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class DrawingState:
    shapes: tuple
    selected_tool: str
    zoom_level: float


class DrawingCanvas:
    def __init__(self) -> None:
        self._shapes: list[str] = []
        self._tool = "pen"
        self._zoom = 1.0

    def add_shape(self, shape: str) -> None:
        self._shapes.append(shape)
        print(f"Added shape: {shape}")

    def select_tool(self, tool: str) -> None:
        self._tool = tool

    def zoom(self, level: float) -> None:
        self._zoom = level

    def save(self) -> DrawingState:
        return DrawingState(
            shapes=tuple(self._shapes),
            selected_tool=self._tool,
            zoom_level=self._zoom,
        )

    def restore(self, state: DrawingState) -> None:
        self._shapes = list(state.shapes)
        self._tool = state.selected_tool
        self._zoom = state.zoom_level
        print(f"Restored: {self._shapes}, tool={self._tool}, zoom={self._zoom}")


class History:
    def __init__(self) -> None:
        self._snapshots: list[DrawingState] = []

    def push(self, state: DrawingState) -> None:
        self._snapshots.append(state)

    def pop(self) -> DrawingState | None:
        if self._snapshots:
            return self._snapshots.pop()
        return None


canvas = DrawingCanvas()
history = History()

history.push(canvas.save())
canvas.add_shape("Circle")
canvas.zoom(1.5)

history.push(canvas.save())
canvas.add_shape("Rectangle")
canvas.select_tool("eraser")

print(canvas._shapes)  # ['Circle', 'Rectangle']

snapshot = history.pop()
if snapshot:
    canvas.restore(snapshot)
# Restored: ['Circle'], tool=pen, zoom=1.5
```

## When to use

- You need undo or redo functionality and want to save snapshots of an object's state at specific moments.
- Taking a direct snapshot of the object's fields would violate encapsulation by exposing private state to external code. The memento pattern keeps the snapshot opaque.
- You need checkpointing in long-running processes, such as saving a game state or a document draft that can be rolled back if an operation fails.

## When not to use

- The object's state is large or changes frequently. Storing full snapshots on every change is expensive in both memory and time.
- The caretaker needs to inspect or manipulate the snapshot content. A memento should be opaque — if the caretaker needs to understand it, a different mechanism is more appropriate.
- Rollback is only needed in exceptional error cases, not as a regular feature. In that case simpler error handling or a transaction boundary is a better fit.
