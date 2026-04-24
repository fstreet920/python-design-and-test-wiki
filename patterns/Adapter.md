# Adapter Pattern

The **Adapter Pattern** is a structural design pattern that allows objects with incompatible interfaces to collaborate.

## Intent
Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

## Python 3.12+ Implementation
Using `Protocol` to define the target interface.

```python
from typing import Protocol

class Target(Protocol):
    """The Target defines the domain-specific interface used by the client code."""
    def request(self) -> str:
        ...

class Adaptee:
    """The Adaptee contains some useful behavior, but its interface is incompatible."""
    def specific_request(self) -> str:
        return ".eetpadA eht fo roivaheb laicepS"

class Adapter:
    """The Adapter makes the Adaptee's interface compatible with the Target's interface."""
    def __init__(self, adaptee: Adaptee) -> None:
        self.adaptee = adaptee

    def request(self) -> str:
        return f"Adapter: (TRANSLATED) {self.adaptee.specific_request()[::-1]}"

def client_code(target: Target) -> None:
    """The client code supports all classes that follow the Target interface."""
    print(target.request(), end="")

if __name__ == "__main__":
    print("Client: I can work just fine with the Target objects:")
    class ConcreteTarget:
        def request(self) -> str:
            return "Target: The default target's behavior."
    
    client_code(ConcreteTarget())
    print("\n")

    adaptee = Adaptee()
    print("Client: The Adaptee class has a weird interface. See, I don't understand it:")
    print(f"Adaptee: {adaptee.specific_request()}")

    print("\nClient: But I can work with it via the Adapter:")
    adapter = Adapter(adaptee)
    client_code(adapter)
```

## Complementary Testing Pattern
- [[Adapter_Harness]]
