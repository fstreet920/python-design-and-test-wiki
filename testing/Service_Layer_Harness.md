# Service Layer Harness

The **Service Layer Harness** focuses on testing the high-level orchestration of business processes, often using Mocks or Fakes for the Unit of Work to ensure isolation.

## Strategy
1.  **Faked Dependencies:** Use a `FakeUnitOfWork` (in-memory) to test the service layer without a real database.
2.  **Orchestration Verification:** Ensure the service correctly calls UoW methods (like `commit()`).
3.  **Boundary Testing:** Test how the service handles missing entities or validation errors.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Service_Layer import UserService
from patterns.Unit_of_Work import MemoryUnitOfWork, UnitOfWork

@pytest.fixture
def uow() -> UnitOfWork:
    return MemoryUnitOfWork()

@pytest.fixture
def service(uow: UnitOfWork) -> UserService:
    return UserService(uow)

def test_create_user_service_orchestration(service: UserService, uow: MemoryUnitOfWork) -> None:
    """Verify that the service correctly uses the UoW to create a user."""
    service.create_user(1, "alice", "alice@example.com")
    
    assert uow.committed is True
    assert uow.users.get(1).username == "alice"

def test_get_user_details_not_found(service: UserService) -> None:
    """Verify service behavior when entity is not found."""
    result = service.get_user_details(999)
    assert result == "User not found"

def test_service_calls_commit(uow: UnitOfWork) -> None:
    """Verify that commit is explicitly called by the service."""
    # Using a Mock to verify specific method call
    mock_uow = MagicMock(spec=UnitOfWork)
    # Mocking context manager behavior
    mock_uow.__enter__.return_value = mock_uow
    
    service = UserService(mock_uow)
    service.create_user(1, "alice", "alice@example.com")
    
    mock_uow.commit.assert_called_once()
```

## Complementary Design Pattern
- [Service_Layer](../patterns/Service_Layer.md)
