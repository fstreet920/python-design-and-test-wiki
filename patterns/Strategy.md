# Strategy Pattern

The **Strategy Pattern** is a behavioral design pattern that lets you define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

## Intent
Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

## Python 3.12+ Implementation
Using `typing.Protocol` for the strategy interface and PEP 695 generics for type-safe context.

```python
from typing import Protocol, runtime_checkable, Iterable

@runtime_checkable
class Strategy(Protocol):
    """The Strategy interface declares operations common to all supported versions of some algorithm."""
    def do_algorithm(self, data: Iterable[str]) -> list[str]:
        ...

class Context:
    """The Context defines the interface of interest to clients."""
    def __init__(self, strategy: Strategy) -> None:
        self._strategy = strategy

    @property
    def strategy(self) -> Strategy:
        return self._strategy

    @strategy.setter
    def strategy(self, strategy: Strategy) -> None:
        self._strategy = strategy

    def do_some_business_logic(self, data: Iterable[str]) -> None:
        print("Context: Sorting data using the strategy (not sure how it'll do it)")
        result = self._strategy.do_algorithm(data)
        print(",".join(result))

class ConcreteStrategyA:
    def do_algorithm(self, data: Iterable[str]) -> list[str]:
        return sorted(data)

class ConcreteStrategyB:
    def do_algorithm(self, data: Iterable[str]) -> list[str]:
        return sorted(data, reverse=True)

# Usage
if __name__ == "__main__":
    context = Context(ConcreteStrategyA())
    print("Client: Strategy is set to normal sorting.")
    context.do_some_business_logic(["a", "b", "c", "d", "e"])

    print("\nClient: Strategy is set to reverse sorting.")
    context.strategy = ConcreteStrategyB()
    context.do_some_business_logic(["a", "b", "c", "d", "e"])
```

## Complementary Testing Pattern
- [[Strategy_Harness]]
