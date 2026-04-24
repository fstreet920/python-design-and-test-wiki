# State Pattern

The **State Pattern** is a behavioral design pattern that allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

## Intent
Encapsulate varying behavior for the same object based on its internal state. This pattern is particularly useful for implementing finite state machines.

## Python 3.12+ Implementation
Using `typing.Protocol` for state interfaces and modern type hinting.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class State(Protocol):
    """The base State protocol declares methods that all Concrete States should implement."""
    def handle1(self, context: "Context") -> None:
        ...

    def handle2(self, context: "Context") -> None:
        ...

class Context:
    """The Context defines the interface of interest to clients."""
    _state: State

    def __init__(self, state: State) -> None:
        self.transition_to(state)

    def transition_to(self, state: State) -> None:
        print(f"Context: Transition to {type(state).__name__}")
        self._state = state

    def request1(self) -> None:
        self._state.handle1(self)

    def request2(self) -> None:
        self._state.handle2(self)

class ConcreteStateA:
    def handle1(self, context: Context) -> None:
        print("ConcreteStateA handles request1.")
        print("ConcreteStateA wants to change the state of the context.")
        context.transition_to(ConcreteStateB())

    def handle2(self, context: Context) -> None:
        print("ConcreteStateA handles request2.")

class ConcreteStateB:
    def handle1(self, context: Context) -> None:
        print("ConcreteStateB handles request1.")

    def handle2(self, context: Context) -> None:
        print("ConcreteStateB handles request2.")
        print("ConcreteStateB wants to change the state of the context.")
        context.transition_to(ConcreteStateA())

# Usage
if __name__ == "__main__":
    context = Context(ConcreteStateA())
    context.request1()
    context.request2()
```

## Complementary Testing Pattern
- [[State_Harness]]
