# State Harness

The **State Harness** pattern provides a standardized way to test the State Pattern in Python, ensuring correct transitions and behavior delegation.

## Strategy
1.  **State Transition Verification:** Verify that calls to context methods result in the expected state transitions.
2.  **Mock States:** Use mock state objects to verify that the `Context` correctly delegates requests to the current state.
3.  **Protocol Compliance:** Ensure all concrete states adhere to the `State` protocol.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.State import Context, State, ConcreteStateA, ConcreteStateB

@pytest.fixture
def context() -> Context:
    """Provides a Context initialized with ConcreteStateA."""
    return Context(ConcreteStateA())

def test_initial_state(context: Context) -> None:
    """Verify the initial state of the context."""
    assert isinstance(context._state, ConcreteStateA)

def test_state_transition_a_to_b(context: Context) -> None:
    """Verify transition from State A to State B on request1."""
    context.request1()
    assert isinstance(context._state, ConcreteStateB)

def test_state_transition_b_to_a(context: Context) -> None:
    """Verify transition from State B back to State A on request2."""
    context.transition_to(ConcreteStateB())
    context.request2()
    assert isinstance(context._state, ConcreteStateA)

def test_context_delegates_to_state() -> None:
    """Verify that Context delegates requests to the state object."""
    mock_state = MagicMock(spec=State)
    context = Context(mock_state)
    
    context.request1()
    mock_state.handle1.assert_called_once_with(context)
    
    context.request2()
    mock_state.handle2.assert_called_once_with(context)

def test_state_protocol_compliance() -> None:
    """Verify that concrete states satisfy the State protocol."""
    assert isinstance(ConcreteStateA(), State)
    assert isinstance(ConcreteStateB(), State)
```

## Complementary Design Pattern
- [[State]]
