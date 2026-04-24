# Context Manager

The **Context Manager** pattern provides a clean way to manage resource setup and teardown, typically using the `with` statement.

## Intent
Ensure that resources are properly initialized and cleaned up, even in the event of errors or exceptions.

## Python 3.12+ Implementation
Using the `__enter__` and `__exit__` protocol, or the `contextlib` module.

```python
from typing import Self
from types import TracebackType

class DatabaseConnection:
    """Classic class-based Context Manager."""
    def __init__(self, db_url: str) -> None:
        self.db_url = db_url
        self.connected = False

    def __enter__(self) -> Self:
        print(f"Connecting to {self.db_url}...")
        self.connected = True
        return self

    def __exit__(
        self,
        exc_type: type[BaseException] | None,
        exc_val: BaseException | None,
        exc_tb: TracebackType | None,
    ) -> None:
        print("Closing connection.")
        self.connected = False

# Usage
if __name__ == "__main__":
    with DatabaseConnection("sqlite:///:memory:") as db:
        print(f"Connection active: {db.connected}")
```

## Complementary Testing Pattern
- [Context_Manager_Harness](../testing/Context_Manager_Harness.md)
