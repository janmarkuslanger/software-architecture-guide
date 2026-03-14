# Interpreter

The Interpreter pattern defines a grammar for a language and provides an interpreter that processes sentences in that language. Each grammar rule is represented as a class, and complex expressions are built by composing simpler ones into a tree.

## How it works

An abstract expression interface declares an `interpret` method. Terminal expressions implement it directly (for example, a number or a variable). Non-terminal expressions implement it by delegating to their child expressions and combining the results. To evaluate an expression, you call `interpret` on the root of the tree.

## Example

```python
from abc import ABC, abstractmethod


class Expression(ABC):
    @abstractmethod
    def interpret(self, context: dict[str, int]) -> int: ...


class NumberExpression(Expression):
    def __init__(self, value: int) -> None:
        self._value = value

    def interpret(self, context: dict[str, int]) -> int:
        return self._value


class VariableExpression(Expression):
    def __init__(self, name: str) -> None:
        self._name = name

    def interpret(self, context: dict[str, int]) -> int:
        return context[self._name]


class AddExpression(Expression):
    def __init__(self, left: Expression, right: Expression) -> None:
        self._left = left
        self._right = right

    def interpret(self, context: dict[str, int]) -> int:
        return self._left.interpret(context) + self._right.interpret(context)


class MultiplyExpression(Expression):
    def __init__(self, left: Expression, right: Expression) -> None:
        self._left = left
        self._right = right

    def interpret(self, context: dict[str, int]) -> int:
        return self._left.interpret(context) * self._right.interpret(context)


class GreaterThanExpression(Expression):
    def __init__(self, left: Expression, right: Expression) -> None:
        self._left = left
        self._right = right

    def interpret(self, context: dict[str, int]) -> int:
        return int(self._left.interpret(context) > self._right.interpret(context))


# Represents: (x + 3) * y > 10
expression = GreaterThanExpression(
    MultiplyExpression(
        AddExpression(VariableExpression("x"), NumberExpression(3)),
        VariableExpression("y"),
    ),
    NumberExpression(10),
)

context_a = {"x": 2, "y": 4}
print(expression.interpret(context_a))  # 1  →  (2+3)*4 = 20 > 10

context_b = {"x": 1, "y": 2}
print(expression.interpret(context_b))  # 0  →  (1+3)*2 = 8 > 10 is False
```

## When to use

- You are implementing a domain-specific language or a rule engine where expressions need to be composed and evaluated programmatically.
- The grammar is simple and stable. A small number of rules that rarely change map cleanly onto a class hierarchy.
- Each grammar rule has a natural object representation and the tree structure makes the evaluation logic easy to read and extend.

## When not to use

- The grammar is complex or grows frequently. Each new rule requires a new class, and the class hierarchy becomes unwieldy quickly.
- Performance is critical. Recursive tree traversal for each evaluation can be slow for large or deeply nested expressions. A compiled approach or a dedicated parser is more appropriate.
- You only need to evaluate a few fixed expressions. Hard-coding them directly is simpler and more readable than building a full interpreter.
