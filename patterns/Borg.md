# Borg (Monostate)

The **Borg Pattern** (also known as Monostate) is a Pythonic alternative to the Singleton pattern. Instead of ensuring only one instance of a class exists, it ensures that all instances of the class share the same state.

## Intent
Share state among all instances of a class, allowing multiple instances to exist but behave as if they are a single object.

## Python 3.12+ Implementation
Using a shared dictionary for the class's `__dict__`.

```python
from typing import Any

class Borg:
    """Borg pattern: all instances share the same state."""
    _shared_state: dict[str, Any] = {}

    def __init__(self) -> None:
        self.__dict__ = self._shared_state

class AppConfig(Borg):
    """Example of shared configuration using Borg."""
    def __init__(self, **kwargs: Any) -> None:
        super().__init__()
        self.__dict__.update(kwargs)

# Usage
if __name__ == "__main__":
    c1 = AppConfig(debug=True)
    c2 = AppConfig()
    
    print(f"c1.debug: {c1.debug}")
    print(f"c2.debug: {c2.debug}")  # Shares state with c1
    
    c2.theme = "dark"
    print(f"c1.theme: {c1.theme}")  # Updates across all instances
```

## Complementary Testing Pattern
- [Borg_Harness](../testing/Borg_Harness.md)
