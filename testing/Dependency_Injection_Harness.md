# Dependency Injection Harness

The **Dependency Injection Harness** focuses on verifying that the client class correctly interacts with injected dependencies, typically using mocks to isolate behavior.

## Strategy
1.  **Mocking the Interface**: Inject a mock that adheres to the `Protocol`.
2.  **Interaction Verification**: Assert that the client calls the correct methods on the injected dependency.
3.  **Substitution Test**: Verify that injecting different implementations changes the behavior of the client without modifying the client's code.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Dependency_Injection import NotificationManager, MessageService

def test_notification_manager_calls_service() -> None:
    # Setup
    mock_service = MagicMock(spec=MessageService)
    manager = NotificationManager(mock_service)
    
    # Act
    manager.notify("Test Message")
    
    # Assert
    mock_service.send.assert_called_once_with("Test Message")

def test_di_substitution() -> None:
    # Verify that different services can be injected
    class MockService:
        def __init__(self) -> None:
            self.sent = False
        def send(self, message: str) -> None:
            self.sent = True

    service = MockService()
    manager = NotificationManager(service) # type: ignore
    manager.notify("Sub Test")
    
    assert service.sent is True
```

## Complementary Design Pattern
- [[Dependency_Injection]]
- [[Mock_Injection]] - The general testing strategy for DI.
