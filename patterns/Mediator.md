# Mediator Pattern

The **Mediator Pattern** is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

## Intent
Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.

## Python 3.12+ Implementation
Using `Protocol` for the mediator interface.

```python
from typing import Protocol, Any

class Mediator(Protocol):
    def notify(self, sender: Any, event: str) -> None:
        ...

class BaseComponent:
    """The Base Component provides the basic functionality of storing a mediator's instance inside component objects."""
    def __init__(self, mediator: Mediator | None = None) -> None:
        self._mediator = mediator

    @property
    def mediator(self) -> Mediator | None:
        return self._mediator

    @mediator.setter
    def mediator(self, mediator: Mediator) -> None:
        self._mediator = mediator

class Component1(BaseComponent):
    def do_a(self) -> None:
        print("Component 1 does A.")
        if self._mediator:
            self._mediator.notify(self, "A")

    def do_b(self) -> None:
        print("Component 1 does B.")
        if self._mediator:
            self._mediator.notify(self, "B")

class Component2(BaseComponent):
    def do_c(self) -> None:
        print("Component 2 does C.")
        if self._mediator:
            self._mediator.notify(self, "C")

    def do_d(self) -> None:
        print("Component 2 does D.")
        if self._mediator:
            self._mediator.notify(self, "D")

class ConcreteMediator:
    def __init__(self, component1: Component1, component2: Component2) -> None:
        self._component1 = component1
        self._component1.mediator = self
        self._component2 = component2
        self._component2.mediator = self

    def notify(self, sender: Any, event: str) -> None:
        if event == "A":
            print("Mediator reacts on A and triggers following operations:")
            self._component2.do_c()
        elif event == "D":
            print("Mediator reacts on D and triggers following operations:")
            self._component1.do_b()
            self._component2.do_c()

if __name__ == "__main__":
    c1 = Component1()
    c2 = Component2()
    mediator = ConcreteMediator(c1, c2)

    print("Client triggers operation A.")
    c1.do_a()

    print("\nClient triggers operation D.")
    c2.do_d()
```

## Complementary Testing Pattern
- [Mediator_Harness](../testing/Mediator_Harness.md)
