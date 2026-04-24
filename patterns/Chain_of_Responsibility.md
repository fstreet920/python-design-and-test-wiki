# Chain of Responsibility Pattern

The **Chain of Responsibility Pattern** is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

## Intent
Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.

## Python 3.12+ Implementation
Using `Protocol` to define the handler interface and `typing.Self` for the linking method.

```python
from typing import Protocol, Self, Any

class Handler(Protocol):
    def set_next(self, handler: "Handler") -> "Handler":
        ...

    def handle(self, request: Any) -> str | None:
        ...

class AbstractHandler:
    """The default chaining behavior can be implemented inside a base handler class."""
    _next_handler: Handler | None = None

    def set_next(self, handler: Handler) -> Handler:
        self._next_handler = handler
        # Returning a handler from here will let us link handlers in a
        # convenient way like this:
        # monkey.set_next(squirrel).set_next(dog)
        return handler

    def handle(self, request: Any) -> str | None:
        if self._next_handler:
            return self._next_handler.handle(request)
        return None

class MonkeyHandler(AbstractHandler):
    def handle(self, request: Any) -> str | None:
        if request == "Banana":
            return f"Monkey: I'll eat the {request}."
        else:
            return super().handle(request)

class SquirrelHandler(AbstractHandler):
    def handle(self, request: Any) -> str | None:
        if request == "Nut":
            return f"Squirrel: I'll eat the {request}."
        else:
            return super().handle(request)

class DogHandler(AbstractHandler):
    def handle(self, request: Any) -> str | None:
        if request == "MeatBall":
            return f"Dog: I'll eat the {request}."
        else:
            return super().handle(request)

def client_code(handler: Handler) -> None:
    for food in ["Nut", "Banana", "Cup of coffee"]:
        print(f"\nClient: Who wants a {food}?")
        result = handler.handle(food)
        if result:
            print(f"  {result}", end="")
        else:
            print(f"  {food} was left untouched.", end="")

if __name__ == "__main__":
    monkey = MonkeyHandler()
    squirrel = SquirrelHandler()
    dog = DogHandler()

    monkey.set_next(squirrel).set_next(dog)

    print("Chain: Monkey > Squirrel > Dog")
    client_code(monkey)
    print("\n")

    print("Subchain: Squirrel > Dog")
    client_code(squirrel)
```

## Complementary Testing Pattern
- [[Chain_of_Responsibility_Harness]]
