# Factory Fixture

The **Factory Fixture** pattern is a structural testing pattern that allows a single fixture to create multiple distinct objects with varying states within a single test case.

## Strategy
1.  **Functional Return**: Instead of returning a static object, the fixture returns a local function (the factory).
2.  **State Customization**: The factory function accepts arguments to customize the state of the object being created.
3.  **Tracking & Cleanup**: If the objects require cleanup, the fixture can maintain a registry of created objects and perform teardown in the `yield` phase.

## Pytest Implementation

```python
import pytest
from typing import Callable, Any
from dataclasses import dataclass

@dataclass
class User:
    username: str
    role: str

@pytest.fixture
def make_user() -> Callable[[str, str], User]:
    """A factory fixture for creating users with specific roles."""
    created_users: list[User] = []

    def _factory(username: str, role: str = "user") -> User:
        user = User(username=username, role=role)
        created_users.append(user)
        return user

    yield _factory

    # Teardown: Perform cleanup for all created users if necessary
    created_users.clear()

def test_multiple_user_roles(make_user: Callable[[str, str], User]) -> None:
    admin = make_user("alice", "admin")
    guest = make_user("bob", "guest")
    
    assert admin.role == "admin"
    assert guest.role == "guest"
    assert admin.username != guest.username
```

## Complementary Design Patterns
- [[Abstract_Factory]] - The factory fixture is a functional implementation of an abstract factory for tests.
- [[Builder]] - Useful for creating complex test data step-by-step.
