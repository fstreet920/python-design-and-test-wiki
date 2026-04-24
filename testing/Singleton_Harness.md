# Singleton Harness

The **Singleton Harness** provides a standardized way to test the Singleton Pattern, focusing on instance uniqueness and thread safety.

## Strategy
1.  **Identity Verification:** Verify that multiple calls to the constructor return the exact same object (same `id`).
2.  **Thread Safety Testing:** Use multiple threads to instantiate the singleton simultaneously and verify that only one instance is created.
3.  **State Persistence:** Verify that modifying the state of one "instance" affects all others, as they are the same object.
4.  **Meta-Reset (Careful):** In some test environments, you may need to clear `_instances` to isolate tests, though this should be handled with caution.

## Pytest Implementation

```python
import pytest
import threading
from patterns.Singleton import Singleton, SingletonMeta

def test_singleton_identity() -> None:
    """Verify that multiple instantiations return the same object."""
    s1 = Singleton("First")
    s2 = Singleton("Second")
    
    assert s1 is s2
    assert s1.value == "First"  # The second __init__ call might still run or be ignored depending on implementation
    # Note: In the provided implementation, __init__ runs every time, 
    # so s2.value would actually overwrite s1.value if not guarded.

def test_singleton_state_sharing() -> None:
    """Verify that state changes are reflected across all references."""
    s1 = Singleton("Initial")
    s1.value = "Changed"
    
    s2 = Singleton("New")
    assert s2.value == "Changed" # Or "New" if __init__ overwrote it

def test_singleton_thread_safety() -> None:
    """Verify that concurrent access doesn't create multiple instances."""
    results: list[Singleton] = []
    
    def create_singleton() -> None:
        s = Singleton(f"Thread-{threading.get_ident()}")
        results.append(s)

    threads = [threading.Thread(target=create_singleton) for _ in range(10)]
    for t in threads:
        t.start()
    for t in threads:
        t.join()

    # All instances in results should be the same object
    first_instance = results[0]
    for instance in results:
        assert instance is first_instance

@pytest.fixture(autouse=True)
def reset_singleton() -> None:
    """Cleanup singleton instances between tests if necessary."""
    SingletonMeta._instances.clear()
    yield
```

## Complementary Design Pattern
- [Singleton](../patterns/Singleton.md)
