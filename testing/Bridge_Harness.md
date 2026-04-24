# Bridge Harness

The **Bridge Harness** provides a standardized way to test the Bridge Pattern, ensuring that abstraction and implementation are correctly decoupled and can vary independently.

## Strategy
1.  **Independent Variation:** Test different combinations of Abstraction and Implementation to verify they work together correctly.
2.  **Mock Implementation:** Use mock implementations to test the Abstraction in isolation.
3.  **Mock Abstraction:** Verify that Abstractions correctly delegate to their Implementations.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Bridge import (
    Abstraction, ExtendedAbstraction, Implementation,
    ConcreteImplementationA, ConcreteImplementationB
)

@pytest.fixture
def mock_implementation() -> MagicMock:
    return MagicMock(spec=Implementation)

def test_abstraction_delegates_to_implementation(mock_implementation: MagicMock) -> None:
    """Verify that the base abstraction calls the implementation's method."""
    mock_implementation.operation_implementation.return_value = "Mock Result"
    abstraction = Abstraction(mock_implementation)
    
    result = abstraction.operation()
    
    assert "Abstraction: Base operation" in result
    assert "Mock Result" in result
    mock_implementation.operation_implementation.assert_called_once()

def test_extended_abstraction_delegates_to_implementation(mock_implementation: MagicMock) -> None:
    """Verify that the extended abstraction calls the implementation's method."""
    mock_implementation.operation_implementation.return_value = "Mock Result"
    abstraction = ExtendedAbstraction(mock_implementation)
    
    result = abstraction.operation()
    
    assert "ExtendedAbstraction: Extended operation" in result
    assert "Mock Result" in result
    mock_implementation.operation_implementation.assert_called_once()

@pytest.mark.parametrize("implementation_class", [ConcreteImplementationA, ConcreteImplementationB])
def test_concrete_implementations(implementation_class: type[Implementation]) -> None:
    """Verify that concrete implementations return the expected format."""
    implementation = implementation_class()
    result = implementation.operation_implementation()
    assert implementation_class.__name__ in result
```

## Complementary Design Pattern
- [[Bridge]]
