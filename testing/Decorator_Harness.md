# Decorator Harness

The **Decorator Harness** provides a standardized way to test the Decorator Pattern, ensuring that decorators correctly wrap components and extend their behavior without breaking the interface.

## Strategy
1.  **Interface Consistency:** Verify that all decorators implement the `Component` protocol.
2.  **Behavioral Augmentation:** Verify that decorators add their specific behavior while still including the output of the wrapped component.
3.  **Nesting Verification:** Ensure that decorators can be nested multiple times and in different orders.
4.  **Mock Components:** Use mock components to verify that decorators correctly delegate calls to the objects they wrap.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Decorator import Component, ConcreteComponent, ConcreteDecoratorA, ConcreteDecoratorB

def test_concrete_component_operation() -> None:
    """Verify base component behavior."""
    component = ConcreteComponent()
    assert component.operation() == "ConcreteComponent"

def test_decorator_a_adds_behavior() -> None:
    """Verify that DecoratorA wraps the output correctly."""
    component = ConcreteComponent()
    decorator = ConcreteDecoratorA(component)
    assert decorator.operation() == "ConcreteDecoratorA(ConcreteComponent)"

def test_decorator_nesting() -> None:
    """Verify that decorators can be nested."""
    component = ConcreteComponent()
    dec_a = ConcreteDecoratorA(component)
    dec_b = ConcreteDecoratorB(dec_a)
    
    assert dec_b.operation() == "ConcreteDecoratorB(ConcreteDecoratorA(ConcreteComponent))"

def test_decorator_delegation_with_mock() -> None:
    """Verify that the decorator delegates to the wrapped component."""
    mock_component = MagicMock(spec=Component)
    mock_component.operation.return_value = "MockResult"
    
    decorator = ConcreteDecoratorA(mock_component)
    result = decorator.operation()
    
    assert "MockResult" in result
    mock_component.operation.assert_called_once()

def test_decorator_protocol_compliance() -> None:
    """Verify that decorators follow the Component protocol."""
    component = ConcreteComponent()
    decorator = ConcreteDecoratorA(component)
    assert isinstance(decorator, Component)
```

## Complementary Design Pattern
- [[Decorator]]
