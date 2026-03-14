# Composite

The Composite pattern lets you treat individual objects and groups of objects through the same interface. You can build tree structures where leaf nodes and container nodes are interchangeable from the caller's perspective.

## How it works

A common interface is defined for both leaves and composites. A leaf implements the interface directly. A composite also implements the interface but holds a collection of children, each of which can be either a leaf or another composite. Operations on a composite are usually delegated to all children and the results are combined.

## Example

```python
from abc import ABC, abstractmethod


class FileSystemItem(ABC):
    def __init__(self, name: str) -> None:
        self.name = name

    @abstractmethod
    def size(self) -> int: ...

    @abstractmethod
    def display(self, indent: int = 0) -> None: ...


class File(FileSystemItem):
    def __init__(self, name: str, size_bytes: int) -> None:
        super().__init__(name)
        self._size = size_bytes

    def size(self) -> int:
        return self._size

    def display(self, indent: int = 0) -> None:
        print(f"{'  ' * indent}📄 {self.name} ({self._size} bytes)")


class Folder(FileSystemItem):
    def __init__(self, name: str) -> None:
        super().__init__(name)
        self._children: list[FileSystemItem] = []

    def add(self, item: FileSystemItem) -> None:
        self._children.append(item)

    def size(self) -> int:
        return sum(child.size() for child in self._children)

    def display(self, indent: int = 0) -> None:
        print(f"{'  ' * indent}📁 {self.name}/ ({self.size()} bytes)")
        for child in self._children:
            child.display(indent + 1)


root = Folder("project")
src = Folder("src")
src.add(File("main.py", 1200))
src.add(File("utils.py", 800))
root.add(src)
root.add(File("README.md", 300))

root.display()
# 📁 project/ (2300 bytes)
#   📁 src/ (2000 bytes)
#     📄 main.py (1200 bytes)
#     📄 utils.py (800 bytes)
#   📄 README.md (300 bytes)

print(root.size())  # 2300
```

## When to use

- Your domain naturally forms a tree: menus with submenus, UI component hierarchies, organizational structures, file systems.
- You want client code to treat individual items and groups of items identically without writing special cases for each.
- You need to add new leaf or composite types without changing the operations that traverse the tree.

## When not to use

- The structure is not hierarchical. Forcing a flat or graph-based relationship into a tree makes the code misleading.
- You need different operations for leaves versus composites. The uniform interface then becomes a leaky abstraction that requires type checks inside methods.
- Performance is critical and recursive tree traversal would be too slow for the depth or breadth of the data.
