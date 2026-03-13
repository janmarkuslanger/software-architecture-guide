# Flyweight

The Flyweight pattern reduces memory usage by sharing common state across many fine-grained objects. Instead of each object storing all its data, shared data is extracted into a flyweight and reused. Only the data that truly differs per instance is stored on the instance itself.

## How it works

State is split into two parts. Intrinsic state is the shared, immutable data that can be stored in the flyweight. Extrinsic state is the context-specific data that changes per usage and is passed in at call time. A factory manages a pool of flyweight instances and returns an existing one whenever the same intrinsic state is requested.

## Example

A document has thousands of characters, but only a handful of distinct styles (font, size, bold). Instead of storing style data on every character, each character references a shared `TextStyle` flyweight and supplies its own position as extrinsic state.

```python
class TextStyle:  # Flyweight — intrinsic (shared, immutable) state
    def __init__(self, font: str, size: int, bold: bool) -> None:
        self.font = font
        self.size = size
        self.bold = bold


_cache: dict[tuple, TextStyle] = {}

def get_style(font: str, size: int, bold: bool) -> TextStyle:
    key = (font, size, bold)
    if key not in _cache:
        _cache[key] = TextStyle(font, size, bold)
    return _cache[key]


def render(char: str, x: int, style: TextStyle) -> str:
    weight = "bold" if style.bold else "normal"
    return f"'{char}' at x={x}  {style.font} {style.size}pt {weight}"


body    = get_style("Arial", 12, False)
heading = get_style("Arial", 18, True)

# Extrinsic state (char + position) lives outside the flyweight
document = [
    ("H", 0,  heading),
    ("e", 20, heading),
    ("l", 40, body),
    ("l", 55, body),
    ("o", 70, body),
]

for char, x, style in document:
    print(render(char, x, style))

print(f"\nTextStyle objects in memory: {len(_cache)}")  # 2, not 5
# 'H' at x=0   Arial 18pt bold
# 'e' at x=20  Arial 18pt bold
# 'l' at x=40  Arial 12pt normal
# 'l' at x=55  Arial 12pt normal
# 'o' at x=70  Arial 12pt normal
#
# TextStyle objects in memory: 2  (not 5)
```

The two `TextStyle` instances are created once and reused for every character that shares the same style. Scale this to a 100,000-character document with ten styles and you still allocate only ten style objects.

## When to use

- Your application creates a very large number of similar objects and memory consumption is a measurable problem.
- Most of the object's data is the same across instances and only a small amount of context-specific data varies.
- The objects can be replaced by a shared instance plus externally supplied context without breaking the application's logic.

## When not to use

- The number of objects is small enough that memory is not a concern. The extra indirection adds complexity for no measurable gain.
- Extrinsic state is large or complex. Passing it in on every call can outweigh the savings from shared intrinsic state.
- Objects need to hold mutable state that differs per instance. Flyweights work with immutable shared state; mixing in mutability makes the pattern unsafe.
