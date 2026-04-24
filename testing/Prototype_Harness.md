# Prototype Harness

The **Prototype Harness** provides a standardized way to test the Prototype Pattern, focusing on the independence of clones from their prototypes.

## Strategy
1.  **Deep Copy Verification:** Ensure that the clone is a different object from the original and that nested objects are also cloned (not shared).
2.  **Attribute Override:** Verify that attributes passed during cloning correctly override the original's values.
3.  **Registration Management:** Test the ability to register, unregister, and retrieve prototypes from a manager.
4.  **Error Handling:** Verify that attempting to clone an unregistered name raises an appropriate error.

## Pytest Implementation

```python
import pytest
from patterns.Prototype import Prototype

class MockComponent:
    def __init__(self, value: int, nested: list[int]) -> None:
        self.value = value
        self.nested = nested

@pytest.fixture
def prototype_manager() -> Prototype:
    return Prototype()

def test_cloning_creates_new_object(prototype_manager: Prototype) -> None:
    """Verify that the clone is a new object with the same values."""
    original = MockComponent(10, [1, 2, 3])
    prototype_manager.register_object("test", original)
    
    clone = prototype_manager.clone("test")
    
    assert clone is not original
    assert clone.value == original.value
    assert clone.nested == original.nested
    assert clone.nested is not original.nested  # Deep copy check

def test_cloning_with_attribute_overrides(prototype_manager: Prototype) -> None:
    """Verify that cloning allows overriding specific attributes."""
    original = MockComponent(10, [1])
    prototype_manager.register_object("test", original)
    
    clone = prototype_manager.clone("test", value=20)
    
    assert clone.value == 20
    assert original.value == 10

def test_cloning_unregistered_name_raises_error(prototype_manager: Prototype) -> None:
    """Verify that cloning a non-existent prototype raises ValueError."""
    with pytest.raises(ValueError, match="not registered"):
        prototype_manager.clone("non_existent")

def test_registration_management(prototype_manager: Prototype) -> None:
    """Verify that objects can be registered and unregistered."""
    obj = MockComponent(1, [])
    prototype_manager.register_object("obj", obj)
    assert prototype_manager.clone("obj") is not None
    
    prototype_manager.unregister_object("obj")
    with pytest.raises(ValueError):
        prototype_manager.clone("obj")
```

## Complementary Design Pattern
- [[Prototype]]
