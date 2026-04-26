# Repository Pattern

The **Repository Pattern** mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects. It hides the details of data access (like SQL queries) behind a clean abstraction.

## Intent
Provide a clean interface for data access, allowing the domain layer to remain decoupled from the persistence logic. It makes the code more testable by allowing easy mocking of data sources.

## Python 3.12+ Implementation
Using PEP 695 generics and `typing.Protocol` for a type-safe, structural interface.

```python
from typing import Protocol, Optional, Iterable, runtime_checkable
from dataclasses import dataclass, field

@dataclass(frozen=True)
class User:
    id: int
    username: str
    email: str

@runtime_checkable
class UserRepository(Protocol):
    """The Repository interface for User entities."""
    def add(self, user: User) -> None:
        ...

    def get(self, user_id: int) -> Optional[User]:
        ...

    def list(self) -> Iterable[User]:
        ...

    def remove(self, user: User) -> None:
        ...

class MemoryUserRepository:
    """An in-memory implementation of the UserRepository, useful for testing."""
    def __init__(self) -> None:
        self._users: dict[int, User] = {}

    def add(self, user: User) -> None:
        self._users[user.id] = user

    def get(self, user_id: int) -> Optional[User]:
        return self._users.get(user_id)

    def list(self) -> Iterable[User]:
        return self._users.values()

    def remove(self, user: User) -> None:
        if user.id in self._users:
            del self._users[user.id]

# Usage
if __name__ == "__main__":
    repo: UserRepository = MemoryUserRepository()
    new_user = User(id=1, username="alice", email="alice@example.com")
    
    repo.add(new_user)
    print(f"Added: {repo.get(1)}")
    
    users = list(repo.list())
    print(f"Total users: {len(users)}")
```

## Complementary Testing Pattern
- [Repository_Harness](../testing/Repository_Harness.md)
