# Mediator Harness

The **Mediator Harness** provides a standardized way to test the Mediator Pattern, focusing on the correct orchestration of component interactions by the mediator.

## Strategy
1.  **Notification Verification:** Verify that components correctly notify the mediator of events.
2.  **Reaction Logic:** Verify that the mediator triggers the correct component actions in response to specific events.
3.  **Loose Coupling Verification:** Ensure that components do not have direct references to each other.
4.  **Mock Components:** Use mock components to verify that the mediator calls the expected methods on them.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Mediator import Mediator, Component1, Component2, ConcreteMediator

@pytest.fixture
def mock_mediator() -> MagicMock:
    return MagicMock(spec=Mediator)

def test_component_notifies_mediator(mock_mediator: MagicMock) -> None:
    """Verify that a component triggers the notify method on its mediator."""
    c1 = Component1(mock_mediator)
    c1.do_a()
    mock_mediator.notify.assert_called_once_with(c1, "A")

def test_mediator_coordinates_components() -> None:
    """Verify that the mediator correctly triggers reactions in other components."""
    mock_c1 = MagicMock(spec=Component1)
    mock_c2 = MagicMock(spec=Component2)
    
    # We need to manually set up the mediator with mocks since it expects real classes in __init__
    # but we can simulate the notification.
    mediator = ConcreteMediator.__new__(ConcreteMediator)
    mediator._component1 = mock_c1
    mediator._component2 = mock_c2
    
    mediator.notify(mock_c1, "A")
    
    mock_c2.do_c.assert_called_once()
    mock_c1.do_b.assert_not_called()

def test_mediator_reaction_to_d() -> None:
    """Verify complex reaction logic in the mediator."""
    mock_c1 = MagicMock(spec=Component1)
    mock_c2 = MagicMock(spec=Component2)
    
    mediator = ConcreteMediator.__new__(ConcreteMediator)
    mediator._component1 = mock_c1
    mediator._component2 = mock_c2
    
    mediator.notify(mock_c2, "D")
    
    mock_c1.do_b.assert_called_once()
    mock_c2.do_c.assert_called_once()

def test_component_mediator_linkage() -> None:
    """Verify that the mediator correctly registers itself with components."""
    c1 = Component1()
    c2 = Component2()
    mediator = ConcreteMediator(c1, c2)
    
    assert c1.mediator is mediator
    assert c2.mediator is mediator
```

## Complementary Design Pattern
- [[Mediator]]
