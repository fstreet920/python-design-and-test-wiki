# Visitor Harness

The **Visitor Harness** pattern provides a standardized way to test the Visitor Pattern in Python, ensuring that components correctly accept visitors and that visitors correctly dispatch operations based on component types.

## Strategy
1.  **Double Dispatch Verification:** Verify that components correctly call the corresponding visitor method (e.g., `ConcreteComponentA` calls `visit_concrete_component_a`).
2.  **Isolated Visitor Testing:** Use mock components to verify that visitors perform the correct operations and access the expected component methods.
3.  **Protocol Compliance:** Ensure that all components and visitors adhere to their respective protocols.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Visitor import (
    Visitor, Component, ConcreteComponentA, ConcreteComponentB, 
    ConcreteVisitor1
)

def test_component_a_accepts_visitor() -> None:
    """Verify that Component A calls the correct visitor method."""
    component = ConcreteComponentA()
    mock_visitor = MagicMock(spec=Visitor)
    
    component.accept(mock_visitor)
    mock_visitor.visit_concrete_component_a.assert_called_once_with(component)

def test_component_b_accepts_visitor() -> None:
    """Verify that Component B calls the correct visitor method."""
    component = ConcreteComponentB()
    mock_visitor = MagicMock(spec=Visitor)
    
    component.accept(mock_visitor)
    mock_visitor.visit_concrete_component_b.assert_called_once_with(component)

def test_visitor1_logic_with_component_a() -> None:
    """Verify Visitor 1's interaction with Component A."""
    visitor = ConcreteVisitor1()
    # Using a real component to verify interaction with its methods
    component = ConcreteComponentA()
    
    # We can check side effects (like print) or use a mock component
    # Here we just ensure it follows protocol and runs without error
    visitor.visit_concrete_component_a(component)
    assert isinstance(visitor, Visitor)

def test_visitor_protocol_compliance() -> None:
    """Verify that ConcreteVisitor1 satisfies the Visitor protocol."""
    assert isinstance(ConcreteVisitor1(), Visitor)

def test_component_protocol_compliance() -> None:
    """Verify that concrete components satisfy the Component protocol."""
    assert isinstance(ConcreteComponentA(), Component)
    assert isinstance(ConcreteComponentB(), Component)
```

## Complementary Design Pattern
- [[Visitor]]
