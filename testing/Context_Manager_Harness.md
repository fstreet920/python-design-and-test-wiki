# Context Manager Harness

The **Context Manager Harness** ensures that resources are correctly setup upon entry and reliably cleaned up upon exit, including under exception scenarios.

## Strategy
1.  **State Tracking**: Verify the state of the resource inside and outside the `with` block.
2.  **Exception Handling**: Verify that `__exit__` is called even if an exception is raised inside the block.
3.  **Mock Verification**: Use a Spy or Mock to track the lifecycle calls.

## Pytest Implementation

```python
import pytest
from patterns.Context_Manager import DatabaseConnection

def test_context_manager_lifecycle() -> None:
    db = DatabaseConnection("test_db")
    assert not db.connected
    
    with db as connected_db:
        assert connected_db.connected
        assert connected_db is db
        
    assert not db.connected

def test_context_manager_exception_cleanup() -> None:
    db = DatabaseConnection("error_db")
    
    with pytest.raises(ValueError):
        with db:
            assert db.connected
            raise ValueError("Test error")
            
    assert not db.connected
```

## Complementary Design Pattern
- [[Context_Manager]]
- [[Yield_Fixture]] - The testing equivalent of a context manager.
