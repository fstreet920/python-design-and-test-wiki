# Descriptor

The **Descriptor** pattern allows you to define how an attribute is accessed, modified, or deleted by defining a protocol on a separate class.

## Intent
Encapsulate attribute access logic (like validation or lazy loading) without polluting the main class with getter and setter methods.

## Python 3.12+ Implementation
Using `__get__`, `__set__`, and `__set_name__`.

```python
from typing import Any

class Validator:
    """Descriptor for basic integer validation."""
    def __set_name__(self, owner: Any, name: str) -> None:
        self.public_name = name
        self.private_name = "_" + name

    def __get__(self, obj: Any, objtype: Any = None) -> Any:
        return getattr(obj, self.private_name)

    def __set__(self, obj: Any, value: int) -> None:
        if not isinstance(value, int):
            raise TypeError(f"Expected {self.public_name} to be an int")
        if value < 0:
            raise ValueError(f"Expected {self.public_name} to be non-negative")
        setattr(obj, self.private_name, value)

class InventoryItem:
    quantity = Validator()

    def __init__(self, name: str, quantity: int) -> None:
        self.name = name
        self.quantity = quantity

# Usage
if __name__ == "__main__":
    item = InventoryItem("Widgets", 10)
    print(f"{item.name}: {item.quantity}")
    # item.quantity = -5  # Raises ValueError
```

## Complementary Testing Pattern
- [Descriptor_Harness](../testing/Descriptor_Harness.md)
