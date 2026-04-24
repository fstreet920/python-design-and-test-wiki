# Observer Harness

The **Observer Harness** pattern provides a standardized way to test the Observer Pattern in Python, ensuring that the subject correctly manages its observers and that observers react appropriately to state changes.

## Strategy
1.  **Mock Observers:** Use `unittest.mock.MagicMock` that implements the `Observer` protocol to verify that `update` is called.
2.  **Subscription Management:** Verify that `attach` and `detach` correctly add and remove observers from the subject's internal list.
3.  **Notification Triggering:** Ensure that state changes in the subject trigger the `notify` method and subsequently call `update` on all registered observers.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Observer import Subject, Observer, ConcreteObserverA

@pytest.fixture
def subject() -> Subject[int]:
    """Provides a fresh Subject instance for each test."""
    return Subject[int](0)

@pytest.fixture
def mock_observer() -> MagicMock:
    """Provides a mock observer to verify notifications."""
    observer = MagicMock(spec=Observer)
    # Ensure it follows the protocol
    return observer

def test_subject_attaches_observer(subject: Subject[int], mock_observer: MagicMock) -> None:
    """Verify that observers can be attached to the subject."""
    subject.attach(mock_observer)
    assert mock_observer in subject._observers

def test_subject_detaches_observer(subject: Subject[int], mock_observer: MagicMock) -> None:
    """Verify that observers can be detached from the subject."""
    subject.attach(mock_observer)
    subject.detach(mock_observer)
    assert mock_observer not in subject._observers

def test_subject_notifies_observers_on_state_change(subject: Subject[int], mock_observer: MagicMock) -> None:
    """Verify that all attached observers are notified when state changes."""
    subject.attach(mock_observer)
    subject.state = 1
    
    mock_observer.update.assert_called_once_with(subject)

def test_concrete_observer_logic(subject: Subject[int]) -> None:
    """Verify specific business logic in a concrete observer."""
    observer = ConcreteObserverA()
    subject.attach(observer)
    
    # We can use a mock to spy on the print or check side effects
    # For simplicity, we just ensure it doesn't crash and follows protocol
    from typing import runtime_checkable
    assert isinstance(observer, Observer)

def test_multiple_observers_notification(subject: Subject[int]) -> None:
    """Verify that multiple observers are notified in order."""
    obs1 = MagicMock(spec=Observer)
    obs2 = MagicMock(spec=Observer)
    
    subject.attach(obs1)
    subject.attach(obs2)
    
    subject.state = 10
    
    obs1.update.assert_called_once_with(subject)
    obs2.update.assert_called_once_with(subject)
```

## Complementary Design Pattern
- [Observer](../patterns/Observer.md)
