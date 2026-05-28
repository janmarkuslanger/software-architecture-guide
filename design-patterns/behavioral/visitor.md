# Visitor

The Visitor pattern lets you add new operations to a set of objects without modifying those objects. The operation is defined in a separate visitor class, and each object type accepts the visitor and delegates the work to it. You can add as many visitors as you like without touching the object hierarchy.

## How it works

Each element in the object structure implements an `accept` method that takes a visitor and calls the visitor's method that corresponds to its own type. The visitor defines a method for each element type. When you want a new operation, you write a new visitor class that implements all those methods. The element classes remain unchanged.

## Example

```python
from __future__ import annotations
from abc import ABC, abstractmethod


class DocumentElement(ABC):
    @abstractmethod
    def accept(self, visitor: DocumentVisitor) -> None: ...


class Heading(DocumentElement):
    def __init__(self, text: str, level: int) -> None:
        self.text = text
        self.level = level

    def accept(self, visitor: DocumentVisitor) -> None:
        visitor.visit_heading(self)


class Paragraph(DocumentElement):
    def __init__(self, text: str) -> None:
        self.text = text

    def accept(self, visitor: DocumentVisitor) -> None:
        visitor.visit_paragraph(self)


class CodeBlock(DocumentElement):
    def __init__(self, code: str, language: str) -> None:
        self.code = code
        self.language = language

    def accept(self, visitor: DocumentVisitor) -> None:
        visitor.visit_code_block(self)


class DocumentVisitor(ABC):
    @abstractmethod
    def visit_heading(self, element: Heading) -> None: ...

    @abstractmethod
    def visit_paragraph(self, element: Paragraph) -> None: ...

    @abstractmethod
    def visit_code_block(self, element: CodeBlock) -> None: ...


class HTMLRenderer(DocumentVisitor):
    def visit_heading(self, element: Heading) -> None:
        tag = f"h{element.level}"
        print(f"<{tag}>{element.text}</{tag}>")

    def visit_paragraph(self, element: Paragraph) -> None:
        print(f"<p>{element.text}</p>")

    def visit_code_block(self, element: CodeBlock) -> None:
        print(f'<pre><code class="{element.language}">{element.code}</code></pre>')


class WordCountVisitor(DocumentVisitor):
    def __init__(self) -> None:
        self.count = 0

    def visit_heading(self, element: Heading) -> None:
        self.count += len(element.text.split())

    def visit_paragraph(self, element: Paragraph) -> None:
        self.count += len(element.text.split())

    def visit_code_block(self, element: CodeBlock) -> None:
        pass


document: list[DocumentElement] = [
    Heading("Getting Started", level=1),
    Paragraph("Install the package using pip."),
    CodeBlock("pip install mypackage", "bash"),
    Paragraph("Then import it in your project."),
]

print("=== HTML ===")
renderer = HTMLRenderer()
for element in document:
    element.accept(renderer)

print("\n=== Word Count ===")
counter = WordCountVisitor()
for element in document:
    element.accept(counter)
print(f"Total words: {counter.count}")
```

## When to use

- You need to perform multiple distinct operations on a set of objects with different types, and you want to keep each operation in one place rather than scattered across the object classes.
- The object hierarchy is stable but you frequently add new operations. Adding a visitor is cheaper than adding a method to every class in the hierarchy.
- You are processing an abstract syntax tree, document model, or any composite structure where you need several passes for different purposes (rendering, validation, analytics).

## When not to use

- The object hierarchy changes often. Every time you add a new element type, you must update every visitor, which can be expensive if you have many visitors.
- The operations are simple enough that adding a method to each class is not a burden. Visitor introduces double dispatch which is harder to follow than a straightforward method call.
- The language or runtime already provides a more natural mechanism such as pattern matching or multiple dispatch. Using those is more idiomatic and easier to read.
