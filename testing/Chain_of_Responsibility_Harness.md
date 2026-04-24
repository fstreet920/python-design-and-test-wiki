# Chain of Responsibility Harness

The **Chain of Responsibility Harness** provides a standardized way to test the Chain of Responsibility Pattern, ensuring that requests are correctly processed or passed along the chain.

## Strategy
1.  **Request Handling Verification:** Verify that a handler correctly processes requests it's responsible for.
2.  **Delegation Verification:** Verify that a handler correctly passes requests it can't handle to the next handler in the chain.
3.  **Chain Termination:** Verify that the request correctly terminates if no handler in the chain can process it.
4.  **Mock Handlers:** Use mock handlers to verify that `handle` and `set_next` are called correctly.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Chain_of_Responsibility import Handler, MonkeyHandler, SquirrelHandler, DogHandler

@pytest.fixture
def monkey() -> MonkeyHandler:
    return MonkeyHandler()

@pytest.fixture
def squirrel() -> SquirrelHandler:
    return SquirrelHandler()

def test_handler_processes_correct_request(monkey: MonkeyHandler) -> None:
    """Verify that a handler handles its specific request."""
    result = monkey.handle("Banana")
    assert "Monkey: I'll eat the Banana" in result

def test_handler_delegates_request(monkey: MonkeyHandler, squirrel: SquirrelHandler) -> None:
    """Verify that a handler passes the request to the next handler."""
    monkey.set_next(squirrel)
    result = monkey.handle("Nut")
    assert "Squirrel: I'll eat the Nut" in result

def test_chain_termination(monkey: MonkeyHandler) -> None:
    """Verify that an unhandled request returns None at the end of the chain."""
    result = monkey.handle("Coffee")
    assert result is None

def test_mock_delegation() -> None:
    """Verify delegation logic using a mock handler."""
    handler = MonkeyHandler()
    mock_next = MagicMock(spec=Handler)
    mock_next.handle.return_value = "Mock Handled"
    
    handler.set_next(mock_next)
    result = handler.handle("Nut")
    
    assert result == "Mock Handled"
    mock_next.handle.assert_called_once_with("Nut")

def test_fluent_interface(monkey: MonkeyHandler, squirrel: SquirrelHandler) -> None:
    """Verify that set_next returns the passed handler for chaining."""
    returned = monkey.set_next(squirrel)
    assert returned is squirrel
```

## Complementary Design Pattern
- [[Chain_of_Responsibility]]
