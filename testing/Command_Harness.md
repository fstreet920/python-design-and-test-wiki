# Command Harness

The **Command Harness** pattern provides a standardized way to test the Command Pattern in Python, ensuring proper isolation between the command, its receiver, and the invoker.

## Strategy
1.  **Mock Receivers:** Use `unittest.mock.MagicMock` to verify that the command calls the correct methods with expected arguments.
2.  **State Verification:** In integration tests, verify that the receiver's state changes as expected.
3.  **Invoker Decoupling:** Test the invoker using mock commands to ensure it manages execution and history correctly without depending on concrete command logic.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from typing import Generator

# Import the patterns (assuming they are in the same scope or path)
# from my_app.patterns import Command, Receiver, SimpleCommand, Invoker

@pytest.fixture
def mock_receiver() -> MagicMock:
    """Provides a mock receiver to verify interactions."""
    # Using spec=Receiver if the class is defined
    return MagicMock()

@pytest.fixture
def invoker() -> Invoker:
    """Provides a fresh invoker for each test."""
    return Invoker()

def test_command_calls_receiver_method(mock_receiver: MagicMock) -> None:
    """Verify the command correctly delegates to the receiver."""
    payload = "Test Message"
    command = SimpleCommand(mock_receiver, payload)
    
    command.execute()
    
    mock_receiver.action.assert_called_once_with(payload)

def test_invoker_executes_and_records_command(invoker: Invoker, mock_receiver: MagicMock) -> None:
    """Verify the invoker triggers execution and maintains history."""
    command = SimpleCommand(mock_receiver, "Invoker Test")
    
    invoker.set_on_start(command)
    invoker.do_something_important()
    
    # Verify execution via the mock receiver
    mock_receiver.action.assert_called_once()
    
    # Verify history tracking
    assert len(invoker._history) == 1
    assert invoker._history[0] == command

def test_command_adheres_to_protocol() -> None:
    """Verify that SimpleCommand follows the Command Protocol at runtime."""
    from typing import runtime_checkable
    from patterns.Command import Command
    
    mock_receiver = MagicMock()
    command = SimpleCommand(mock_receiver, "Protocol Test")
    
    assert isinstance(command, Command)
```

## Complementary Design Pattern
- [Command](../patterns/Command.md)
