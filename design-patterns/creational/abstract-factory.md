# Abstract Factory

The Abstract Factory pattern provides an interface for creating families of related objects without specifying their concrete classes. Where Factory Method creates one product, Abstract Factory creates a whole set of related products that belong together.

## How it works

An abstract factory interface declares creation methods for each product in the family. A concrete factory implements all of them for one specific variant. Client code receives a factory and only ever calls its creation methods — it never knows which concrete classes are being instantiated.

## Example

```python
from abc import ABC, abstractmethod


class Button(ABC):
    @abstractmethod
    def render(self) -> str: ...


class TextInput(ABC):
    @abstractmethod
    def render(self) -> str: ...


class DarkButton(Button):
    def render(self) -> str:
        return "<button class='dark'>Click</button>"


class DarkTextInput(TextInput):
    def render(self) -> str:
        return "<input class='dark' />"


class LightButton(Button):
    def render(self) -> str:
        return "<button class='light'>Click</button>"


class LightTextInput(TextInput):
    def render(self) -> str:
        return "<input class='light' />"


class UIFactory(ABC):
    @abstractmethod
    def create_button(self) -> Button: ...

    @abstractmethod
    def create_input(self) -> TextInput: ...


class DarkThemeFactory(UIFactory):
    def create_button(self) -> Button:
        return DarkButton()

    def create_input(self) -> TextInput:
        return DarkTextInput()


class LightThemeFactory(UIFactory):
    def create_button(self) -> Button:
        return LightButton()

    def create_input(self) -> TextInput:
        return LightTextInput()


def build_form(factory: UIFactory) -> None:
    button = factory.create_button()
    field = factory.create_input()
    print(button.render())
    print(field.render())


build_form(DarkThemeFactory())
# <button class='dark'>Click</button>
# <input class='dark' />
```

## When to use

- Your system needs to work with multiple families of related objects and you want to guarantee they are always used together consistently.
- You want to switch the entire family of objects (for example, a UI theme or a database backend) by swapping one factory, without touching client code.
- You are building a platform that must support multiple variants from the start and the variants share the same structure.

## When not to use

- You only have one family of products and no plans for variants. The extra abstraction is overhead without benefit.
- The product families are not truly related. Grouping unrelated products into one factory creates false coupling.
- Adding a new product type to the family requires changing every concrete factory, which can become expensive if factories are numerous.
