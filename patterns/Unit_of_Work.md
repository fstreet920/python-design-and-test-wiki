# Unit of Work Pattern

The **Unit of Work Pattern** maintains a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems. In Python, it is most idiomatically implemented using context managers.

## Intent
Group multiple operations into a single atomic transaction. It ensures that either all changes are committed to the data store, or none are, maintaining data integrity.

## Python 3.12+ Implementation
Using `typing.Protocol`, `typing.Self`, and context manager protocols.

```python
from typing import Protocol, Self, runtime_checkable
from patterns.Repository import UserRepository, MemoryUserRepository

@runtime_checkable
class UnitOfWork(Protocol):
    """The Unit of Work interface."""
    users: UserRepository

    def __enter__(self) -> Self:
        ...

    def __exit__(self, exc_type, exc_val, exc_tb) -> None:
        self.rollback()

    def commit(self) -> None:
        ...

    def rollback(self) -> None:
        ...

class MemoryUnitOfWork:
    """A mock implementation of Unit of Work for in-memory testing."""
    def __init__(self) -> None:
        self.users = MemoryUserRepository()
        self.committed = False

    def __enter__(self) -> Self:
        return self

    def __exit__(self, exc_type, exc_val, exc_tb) -> None:
        if exc_type is not None:
            self.rollback()
        # In a real implementation, we might auto-commit if no error
        # but usually we want explicit commits.

    def commit(self) -> None:
        self.committed = True

    def rollback(self) -> None:
        self.committed = False

# Usage
def create_user_service(uow: UnitOfWork, user_data: dict) -> None:
    with uow:
        # Business logic
        from patterns.Repository import User
        user = User(**user_data)
        uow.users.add(user)
        uow.commit()

if __name__ == "__main__":
    uow = MemoryUnitOfWork()
    create_user_service(uow, {"id": 1, "username": "bob", "email": "bob@example.com"})
    print(f"Committed: {uow.committed}")
```

## Complementary Testing Pattern
- [Unit_of_Work_Harness](../testing/Unit_of_Work_Harness.md)
