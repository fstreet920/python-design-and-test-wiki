# Command Pattern

The **Command Pattern** is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request's execution, and support undoable operations.

## Intent
Encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

## Python 3.12+ Implementation
Using `typing.Protocol` for structural typing and Python 3.12's PEP 695 generics.

```python
from typing import Protocol, runtime_checkable, Generic, TypeVar

@runtime_checkable
class Command(Protocol):
    """The Command interface defines a method for executing an operation."""
    def execute(self) -> None:
        ...

class Receiver:
    """The Receiver contains some important business logic."""
    def action(self, message: str) -> None:
        print(f"Receiver: Working on ({message})")

class SimpleCommand:
    """A simple command that wraps a receiver and a fixed message."""
    def __init__(self, receiver: Receiver, payload: str) -> None:
        self._receiver = receiver
        self._payload = payload

    def execute(self) -> None:
        self._receiver.action(self._payload)

class Invoker:
    """The Invoker is associated with one or several commands. It sends a request to the command."""
    def __init__(self) -> None:
        self._history: list[Command] = []

    def set_on_start(self, command: Command) -> None:
        self._on_start = command

    def set_on_finish(self, command: Command) -> None:
        self._on_finish = command

    def do_something_important(self) -> None:
        print("Invoker: Does anyone want something done before I begin?")
        if hasattr(self, "_on_start"):
            self._on_start.execute()
            self._history.append(self._on_start)

        print("Invoker: Doing something really important...")

        print("Invoker: Does anyone want something done after I finish?")
        if hasattr(self, "_on_finish"):
            self._on_finish.execute()
            self._history.append(self._on_finish)

# Usage
if __name__ == "__main__":
    receiver = Receiver()
    invoker = Invoker()
    invoker.set_on_start(SimpleCommand(receiver, "Say Hi!"))
    invoker.set_on_finish(SimpleCommand(receiver, "Send email"))
    invoker.do_something_important()
```

## Complementary Testing Pattern
- [[Command_Harness]]
