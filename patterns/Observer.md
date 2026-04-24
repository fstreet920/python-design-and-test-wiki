# Observer Pattern

The **Observer Pattern** is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they're observing.

## Intent
Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

## Python 3.12+ Implementation
Using `typing.Protocol` for structural typing and Python 3.12's PEP 695 generics for the subject.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Observer(Protocol):
    """The Observer interface declares the update method, used by subjects."""
    def update(self, subject: "Subject") -> None:
        ...

class Subject[T]:
    """
    The Subject owns some important state and notifies observers when the state changes.
    Using PEP 695 Generics for type safety of the state.
    """
    def __init__(self, state: T) -> None:
        self._observers: list[Observer] = []
        self._state: T = state

    def attach(self, observer: Observer) -> None:
        print("Subject: Attached an observer.")
        self._observers.append(observer)

    def detach(self, observer: Observer) -> None:
        self._observers.remove(observer)

    def notify(self) -> None:
        print("Subject: Notifying observers...")
        for observer in self._observers:
            observer.update(self)

    @property
    def state(self) -> T:
        return self._state

    @state.setter
    def state(self, value: T) -> None:
        print(f"Subject: State changed to {value}")
        self._state = value
        self.notify()

class ConcreteObserverA:
    def update(self, subject: Subject) -> None:
        if subject.state < 3:
            print("ConcreteObserverA: Reacted to the event.")

class ConcreteObserverB:
    def update(self, subject: Subject) -> None:
        if subject.state == 0 or subject.state >= 2:
            print("ConcreteObserverB: Reacted to the event.")

# Usage
if __name__ == "__main__":
    subject = Subject[int](0)

    observer_a = ConcreteObserverA()
    subject.attach(observer_a)

    observer_b = ConcreteObserverB()
    subject.attach(observer_b)

    subject.state = 2
    subject.state = 3

    subject.detach(observer_a)
    subject.state = 1
```

## Complementary Testing Pattern
- [[Observer_Harness]]
