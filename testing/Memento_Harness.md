# Memento Harness

The **Memento Harness** provides a standardized way to test the Memento Pattern, focusing on state preservation, restoration, and encapsulation.

## Strategy
1.  **State Restoration Verification:** Verify that the Originator correctly restores its internal state from a previously saved Memento.
2.  **Encapsulation Check:** Ensure that the Caretaker cannot access or modify the internal state stored within the Memento.
3.  **History Management:** Test the Caretaker's ability to store multiple mementos and perform undo operations in the correct (LIFO) order.
4.  **Metadata Verification:** Verify that the Memento correctly stores and provides metadata like creation date or name.

## Pytest Implementation

```python
import pytest
from patterns.Memento import Originator, Caretaker, ConcreteMemento

@pytest.fixture
def originator() -> Originator:
    return Originator("Initial State")

@pytest.fixture
def caretaker(originator: Originator) -> Caretaker:
    return Caretaker(originator)

def test_originator_save_and_restore(originator: Originator) -> None:
    """Verify that the originator can save and then restore its state."""
    initial_state = "State 1"
    originator._state = initial_state
    
    memento = originator.save()
    originator._state = "Changed State"
    
    originator.restore(memento)
    assert originator._state == initial_state

def test_caretaker_undo(originator: Originator, caretaker: Caretaker) -> None:
    """Verify that the caretaker correctly manages undo history."""
    originator._state = "State 1"
    caretaker.backup() # Save State 1
    
    originator._state = "State 2"
    caretaker.backup() # Save State 2
    
    originator._state = "Current State"
    
    caretaker.undo() # Rollback to State 2
    assert originator._state == "State 2"
    
    caretaker.undo() # Rollback to State 1
    assert originator._state == "State 1"

def test_memento_metadata() -> None:
    """Verify that the memento correctly captures metadata."""
    state = "Test State"
    memento = ConcreteMemento(state)
    
    assert memento.get_state() == state
    assert len(memento.get_date()) > 0
    assert "Test Stat" in memento.get_name()

def test_caretaker_undo_on_empty_history(caretaker: Caretaker) -> None:
    """Verify that undoing with no history doesn't raise an error."""
    # Should not raise any exception
    caretaker.undo()
```

## Complementary Design Pattern
- [Memento](../patterns/Memento.md)
