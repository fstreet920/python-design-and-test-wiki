# Visitor Pattern

The **Visitor Pattern** is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

## Intent
Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

## Python 3.12+ Implementation
Using `typing.Protocol` for the Visitor and PEP 695 generics for flexibility if needed.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Component(Protocol):
    def accept(self, visitor: "Visitor") -> None:
        ...

@runtime_checkable
class Visitor(Protocol):
    def visit_concrete_component_a(self, element: "ConcreteComponentA") -> None:
        ...

    def visit_concrete_component_b(self, element: "ConcreteComponentB") -> None:
        ...

class ConcreteComponentA:
    def accept(self, visitor: Visitor) -> None:
        visitor.visit_concrete_component_a(self)

    def exclusive_method_of_concrete_component_a(self) -> str:
        return "A"

class ConcreteComponentB:
    def accept(self, visitor: Visitor) -> None:
        visitor.visit_concrete_component_b(self)

    def special_method_of_concrete_component_b(self) -> str:
        return "B"

class ConcreteVisitor1:
    def visit_concrete_component_a(self, element: ConcreteComponentA) -> None:
        print(f"{element.exclusive_method_of_concrete_component_a()} + ConcreteVisitor1")

    def visit_concrete_component_b(self, element: ConcreteComponentB) -> None:
        print(f"{element.special_method_of_concrete_component_b()} + ConcreteVisitor1")

class ConcreteVisitor2:
    def visit_concrete_component_a(self, element: ConcreteComponentA) -> None:
        print(f"{element.exclusive_method_of_concrete_component_a()} + ConcreteVisitor2")

    def visit_concrete_component_b(self, element: ConcreteComponentB) -> None:
        print(f"{element.special_method_of_concrete_component_b()} + ConcreteVisitor2")

# Usage
if __name__ == "__main__":
    components = [ConcreteComponentA(), ConcreteComponentB()]

    print("The client code works with all visitors via the base Visitor interface:")
    visitor1 = ConcreteVisitor1()
    for component in components:
        component.accept(visitor1)

    print("\nIt allows the same client code to work with different types of visitors:")
    visitor2 = ConcreteVisitor2()
    for component in components:
        component.accept(visitor2)
```

## Complementary Testing Pattern
- [[Visitor_Harness]]
