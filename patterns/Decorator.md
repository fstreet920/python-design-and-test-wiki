# Decorator Pattern

The **Decorator Pattern** is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

## Intent
Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

## Python 3.12+ Implementation
Using `Protocol` to define the component interface.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Component(Protocol):
    def operation(self) -> str:
        ...

class ConcreteComponent:
    def operation(self) -> str:
        return "ConcreteComponent"

class Decorator(Component):
    """The base Decorator class follows the same interface as the other components."""
    _component: Component

    def __init__(self, component: Component) -> None:
        self._component = component

    @property
    def component(self) -> Component:
        return self._component

    def operation(self) -> str:
        return self._component.operation()

class ConcreteDecoratorA(Decorator):
    def operation(self) -> str:
        return f"ConcreteDecoratorA({super().operation()})"

class ConcreteDecoratorB(Decorator):
    def operation(self) -> str:
        return f"ConcreteDecoratorB({super().operation()})"

def client_code(component: Component) -> None:
    print(f"RESULT: {component.operation()}", end="")

if __name__ == "__main__":
    simple = ConcreteComponent()
    print("Client: I've got a simple component:")
    client_code(simple)
    print("\n")

    decorator1 = ConcreteDecoratorA(simple)
    decorator2 = ConcreteDecoratorB(decorator1)
    print("Client: Now I've got a decorated component:")
    client_code(decorator2)
```

## Complementary Testing Pattern
- [[Decorator_Harness]]
