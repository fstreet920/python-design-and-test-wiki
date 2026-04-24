# Facade Harness

The **Facade Harness** provides a standardized way to test the Facade Pattern, ensuring that the facade correctly coordinates subsystem interactions and provides the expected simplified interface.

## Strategy
1.  **Subsystem Coordination:** Verify that calling the facade's method triggers the expected sequence of calls on the subsystems.
2.  **Mock Subsystems:** Use mock subsystem objects to isolate the facade's logic and verify interactions.
3.  **Simplified Output:** Verify that the facade returns a consolidated and simplified result from the subsystem operations.
4.  **Integration Testing:** Test with real subsystems to ensure the facade handles actual subsystem behavior correctly.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Facade import Facade, Subsystem1, Subsystem2

@pytest.fixture
def mock_subsystem1() -> MagicMock:
    return MagicMock(spec=Subsystem1)

@pytest.fixture
def mock_subsystem2() -> MagicMock:
    return MagicMock(spec=Subsystem2)

def test_facade_calls_subsystem_methods(mock_subsystem1: MagicMock, mock_subsystem2: MagicMock) -> None:
    """Verify that the facade correctly orchestrates subsystem calls."""
    facade = Facade(mock_subsystem1, mock_subsystem2)
    
    facade.operation()
    
    mock_subsystem1.operation1.assert_called_once()
    mock_subsystem1.operation_n.assert_called_once()
    mock_subsystem2.operation1.assert_called_once()
    mock_subsystem2.operation_z.assert_called_once()

def test_facade_output_format(mock_subsystem1: MagicMock, mock_subsystem2: MagicMock) -> None:
    """Verify the consolidated output of the facade."""
    mock_subsystem1.operation1.return_value = "S1:R"
    mock_subsystem1.operation_n.return_value = "S1:G"
    mock_subsystem2.operation1.return_value = "S2:R"
    mock_subsystem2.operation_z.return_value = "S2:F"
    
    facade = Facade(mock_subsystem1, mock_subsystem2)
    result = facade.operation()
    
    assert "S1:R" in result
    assert "S2:R" in result
    assert "S1:G" in result
    assert "S2:F" in result

def test_facade_initialization() -> None:
    """Verify that the facade can initialize its own subsystems."""
    facade = Facade()
    assert isinstance(facade._subsystem1, Subsystem1)
    assert isinstance(facade._subsystem2, Subsystem2)
```

## Complementary Design Pattern
- [Facade](../patterns/Facade.md)
