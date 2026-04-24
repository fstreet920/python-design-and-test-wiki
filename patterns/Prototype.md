# Prototype Pattern

The **Prototype Pattern** is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

## Intent
Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.

## Python 3.12+ Implementation
Using `copy.deepcopy` for cloning and `typing.Self` for return type hints.

```python
import copy
from typing import Self, Any

class Prototype:
    def __init__(self) -> None:
        self._objects: dict[str, Any] = {}

    def register_object(self, name: str, obj: Any) -> None:
        self._objects[name] = obj

    def unregister_object(self, name: str) -> None:
        del self._objects[name]

    def clone(self, name: str, **attrs: Any) -> Any:
        """Clone a registered object and update its attributes."""
        obj = copy.deepcopy(self._objects.get(name))
        if obj is None:
            raise ValueError(f"Object with name {name} not registered")
        obj.__dict__.update(attrs)
        return obj

class ComponentWithBackReference:
    def __init__(self, prototype: "Prototype") -> None:
        self.prototype = prototype

    def __copy__(self) -> Self:
        """Custom shallow copy implementation."""
        # This is just an example of how to handle custom copying if needed
        new = type(self)(self.prototype)
        new.__dict__.update(self.__dict__)
        return new

    def __deepcopy__(self, memo: dict[int, Any]) -> Self:
        """Custom deep copy implementation."""
        # Deepcopy requires handling the memo to avoid infinite recursion
        new = type(self)(copy.deepcopy(self.prototype, memo))
        new.__dict__.update(copy.deepcopy(self.__dict__, memo))
        return new

if __name__ == "__main__":
    class ConcreteComponent:
        def __init__(self, value: int) -> None:
            self.value = value
        def __str__(self) -> str:
            return f"ConcreteComponent(value={self.value})"

    prototype_manager = Prototype()
    c1 = ConcreteComponent(10)
    prototype_manager.register_object("c1", c1)

    c2 = prototype_manager.clone("c1", value=20)
    print(f"Original: {c1}")
    print(f"Clone: {c2}")
    print(f"Is same object? {c1 is c2}")
```

## Complementary Testing Pattern
- [[Prototype_Harness]]
