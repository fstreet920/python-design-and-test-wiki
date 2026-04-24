# Facade Pattern

The **Facade Pattern** is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.

## Intent
Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

## Python 3.12+ Implementation
Using a simple class that orchestrates multiple subsystems.

```python
class Subsystem1:
    def operation1(self) -> str:
        return "Subsystem1: Ready!"

    def operation_n(self) -> str:
        return "Subsystem1: Go!"

class Subsystem2:
    def operation1(self) -> str:
        return "Subsystem2: Get ready!"

    def operation_z(self) -> str:
        return "Subsystem2: Fire!"

class Facade:
    """The Facade class provides a simple interface to the complex logic of one or several subsystems."""
    def __init__(self, subsystem1: Subsystem1 | None = None, subsystem2: Subsystem2 | None = None) -> None:
        """Depending on your application's needs, you can provide the Facade with existing subsystem objects or force the Facade to create them on its own."""
        self._subsystem1 = subsystem1 or Subsystem1()
        self._subsystem2 = subsystem2 or Subsystem2()

    def operation(self) -> str:
        """The Facade's methods are convenient shortcuts to the sophisticated functionality of the subsystems."""
        results = []
        results.append("Facade initializes subsystems:")
        results.append(self._subsystem1.operation1())
        results.append(self._subsystem2.operation1())
        results.append("Facade orders subsystems to perform the action:")
        results.append(self._subsystem1.operation_n())
        results.append(self._subsystem2.operation_z())
        return "\n".join(results)

def client_code(facade: Facade) -> None:
    """The client code works with complex subsystems through a simple interface provided by the Facade."""
    print(facade.operation(), end="")

if __name__ == "__main__":
    subsystem1 = Subsystem1()
    subsystem2 = Subsystem2()
    facade = Facade(subsystem1, subsystem2)
    client_code(facade)
```

## Complementary Testing Pattern
- [[Facade_Harness]]
