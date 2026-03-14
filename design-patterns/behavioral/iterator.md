# Iterator

The Iterator pattern provides a way to access the elements of a collection sequentially without exposing the underlying data structure. The caller gets a cursor object that moves through the collection one step at a time, regardless of whether the collection is a list, a tree, a database result, or something else entirely.

## How it works

An iterator object keeps track of the current position within the collection. It exposes two operations: one to check whether more elements are available and one to advance and return the next element. The collection creates the iterator on request. Python formalizes this through the `__iter__` and `__next__` protocols, which integrate directly with `for` loops.

## Example

```python
from __future__ import annotations
from typing import Generic, TypeVar

T = TypeVar("T")


class TreeNode(Generic[T]):
    def __init__(self, value: T) -> None:
        self.value = value
        self.left: TreeNode[T] | None = None
        self.right: TreeNode[T] | None = None


class InOrderIterator(Generic[T]):
    def __init__(self, root: TreeNode[T] | None) -> None:
        self._stack: list[TreeNode[T]] = []
        self._push_left(root)

    def _push_left(self, node: TreeNode[T] | None) -> None:
        while node:
            self._stack.append(node)
            node = node.left

    def __iter__(self) -> InOrderIterator[T]:
        return self

    def __next__(self) -> T:
        if not self._stack:
            raise StopIteration
        node = self._stack.pop()
        self._push_left(node.right)
        return node.value


class BinaryTree(Generic[T]):
    def __init__(self, root: TreeNode[T] | None = None) -> None:
        self._root = root

    def __iter__(self) -> InOrderIterator[T]:
        return InOrderIterator(self._root)


root: TreeNode[int] = TreeNode(4)
root.left = TreeNode(2)
root.right = TreeNode(6)
root.left.left = TreeNode(1)
root.left.right = TreeNode(3)
root.right.left = TreeNode(5)
root.right.right = TreeNode(7)

tree: BinaryTree[int] = BinaryTree(root)
print(list(tree))  # [1, 2, 3, 4, 5, 6, 7]
```

## When to use

- You want to provide a standard traversal interface over a custom data structure such as a tree, graph, or paginated result set, without exposing its internals.
- You need multiple independent iterators over the same collection, each maintaining their own position.
- You want your custom collection to work seamlessly with Python's `for` loop, list comprehensions, and the standard library's iteration utilities.

## When not to use

- The collection is a built-in Python type such as a list or dictionary. These already support iteration natively and wrapping them adds no value.
- You need random access by index rather than sequential traversal. Iteration is inherently sequential and does not replace indexed lookups.
- The traversal is stateful in a complex way that the iterator cannot encapsulate cleanly. In that case an explicit visitor or recursive algorithm communicates the logic better.
