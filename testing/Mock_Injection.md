# Mock Injection (Spy/Stub)

**Mock Injection** is a pattern used to isolate the unit under test by replacing its dependencies with controlled objects (Mocks, Spies, or Stubs).

## Strategy
1.  **Isolation**: Replace external APIs, databases, or complex collaborators with mocks.
2.  **Verification (Spying)**: Assert that specific methods were called with expected arguments.
3.  **Controlled Input (Stubbing)**: Force the dependency to return specific values to test different execution paths.

## Pytest Implementation
Using the `mocker` fixture from `pytest-mock` (preferred for automatic cleanup).

```python
import pytest
from unittest.mock import MagicMock

class Service:
    def send_notification(self, message: str) -> bool:
        # Imagine a real API call here
        return True

def notify_user(service: Service, message: str) -> str:
    if service.send_notification(message):
        return "Success"
    return "Failure"

def test_notify_user_success(mocker: Any) -> None:
    # Setup: Create a spy/mock
    mock_service = mocker.Mock(spec=Service)
    mock_service.send_notification.return_value = True
    
    # Act
    result = notify_user(mock_service, "Hello")
    
    # Assert
    assert result == "Success"
    mock_service.send_notification.assert_called_once_with("Hello")
```

## Complementary Design Patterns
- [Observer](../patterns/Observer.md) - Crucial for verifying that observers are notified without executing their actual logic.
- [Proxy](../patterns/Proxy.md) - Used to verify that a proxy correctly delegates calls to the real subject.
- [Bridge](../patterns/Bridge.md) - Useful for testing the abstraction independently of its implementation.
