# Composite Pattern

The **Composite Pattern** is a structural design pattern that lets you compose objects into tree structures and then work with these structures as if they were individual objects.

## Intent
Compose objects into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.

## Python 3.12+ Implementation
Using `Protocol` to define the component interface.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Component(Protocol):
    """The base Component interface defines operations that are common to both simple and complex objects of the composition."""
    def operation(self) -> str:
        ...

    def add(self, component: "Component") -> None:
        ...

    def remove(self, component: "Component") -> None:
        ...

    def is_composite(self) -> bool:
        return False

class Leaf:
    """The Leaf represents the end objects of a composition. A leaf can't have any children."""
    def operation(self) -> str:
        return "Leaf"

    def add(self, component: Component) -> None:
        raise NotImplementedError("Cannot add to a leaf")

    def remove(self, component: Component) -> None:
        raise NotImplementedError("Cannot remove from a leaf")

    def is_composite(self) -> bool:
        return False

class Composite:
    """The Composite represents the complex components that may have children."""
    def __init__(self) -> None:
        self._children: list[Component] = []

    def add(self, component: Component) -> None:
        self._children.append(component)

    def remove(self, component: Component) -> None:
        self._children.remove(component)

    def is_composite(self) -> bool:
        return True

    def operation(self) -> str:
        results = []
        for child in self._children:
            results.append(child.operation())
        return f"Branch({'+'.join(results)})"

def client_code(component: Component) -> None:
    print(f"RESULT: {component.operation()}")

if __name__ == "__main__":
    simple = Leaf()
    print("Client: I've got a simple component:")
    client_code(simple)

    tree = Composite()
    branch1 = Composite()
    branch1.add(Leaf())
    branch1.add(Leaf())

    branch2 = Composite()
    branch2.add(Leaf())

    tree.add(branch1)
    tree.add(branch2)

    print("\nClient: Now I've got a composite tree:")
    client_code(tree)
```

## Complementary Testing Pattern
- [[Composite_Harness]]
