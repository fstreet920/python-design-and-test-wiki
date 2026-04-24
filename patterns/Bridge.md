# Bridge Pattern

The **Bridge Pattern** is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

## Intent
Decouple an abstraction from its implementation so that the two can vary independently.

## Python 3.12+ Implementation
Using `Protocol` for the implementation hierarchy.

```python
from typing import Protocol

class Implementation(Protocol):
    def operation_implementation(self) -> str:
        ...

class Abstraction:
    def __init__(self, implementation: Implementation) -> None:
        self.implementation = implementation

    def operation(self) -> str:
        return (f"Abstraction: Base operation with:\n"
                f"{self.implementation.operation_implementation()}")

class ExtendedAbstraction(Abstraction):
    def operation(self) -> str:
        return (f"ExtendedAbstraction: Extended operation with:\n"
                f"{self.implementation.operation_implementation()}")

class ConcreteImplementationA:
    def operation_implementation(self) -> str:
        return "ConcreteImplementationA: The result in platform A."

class ConcreteImplementationB:
    def operation_implementation(self) -> str:
        return "ConcreteImplementationB: The result in platform B."

def client_code(abstraction: Abstraction) -> None:
    print(abstraction.operation(), end="")

if __name__ == "__main__":
    implementation = ConcreteImplementationA()
    abstraction = Abstraction(implementation)
    client_code(abstraction)

    print("\n")

    implementation = ConcreteImplementationB()
    abstraction = ExtendedAbstraction(implementation)
    client_code(abstraction)
```

## Complementary Testing Pattern
- [[Bridge_Harness]]
