# Repository Harness

The **Repository Harness** provides a blueprint for testing repository implementations, ensuring they adhere to the interface and behave correctly regardless of the underlying storage mechanism.

## Strategy
1.  **Interface Verification:** Ensure the concrete repository implements the `Protocol`.
2.  **CRUD Operations:** Test adding, retrieving, listing, and removing entities.
3.  **Edge Cases:** Test retrieving non-existent entities (should return `None`) and removing non-existent entities.
4.  **Identity Map (Optional):** If the repository implements an identity map, verify that multiple retrievals of the same ID return the same object instance.

## Pytest Implementation

```python
import pytest
from patterns.Repository import User, MemoryUserRepository, UserRepository

@pytest.fixture
def repo() -> UserRepository:
    return MemoryUserRepository()

@pytest.fixture
def user() -> User:
    return User(id=42, username="testbot", email="test@example.com")

def test_repository_add_and_get(repo: UserRepository, user: User) -> None:
    """Verify that a user can be added and then retrieved."""
    repo.add(user)
    retrieved = repo.get(user.id)
    assert retrieved == user
    assert retrieved is user  # In memory implementation often preserves identity

def test_repository_get_nonexistent(repo: UserRepository) -> None:
    """Verify that getting a non-existent ID returns None."""
    assert repo.get(999) is None

def test_repository_list(repo: UserRepository) -> None:
    """Verify that listing returns all added users."""
    users = [
        User(id=1, username="u1", email="e1"),
        User(id=2, username="u2", email="e2"),
    ]
    for u in users:
        repo.add(u)
    
    all_users = list(repo.list())
    assert len(all_users) == 2
    assert all(u in all_users for u in users)

def test_repository_remove(repo: UserRepository, user: User) -> None:
    """Verify that a user can be removed."""
    repo.add(user)
    repo.remove(user)
    assert repo.get(user.id) is None

def test_repository_protocol_compliance() -> None:
    """Verify that MemoryUserRepository satisfies the UserRepository protocol."""
    assert isinstance(MemoryUserRepository(), UserRepository)
```

## Complementary Design Pattern
- [Repository](../patterns/Repository.md)
