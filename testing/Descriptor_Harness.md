# Descriptor Harness

The **Descriptor Harness** verifies that attribute access logic (validation, transformation, etc.) is correctly applied when interacting with the owner class.

## Strategy
1.  **Valid Input Verification**: Ensure the descriptor accepts and stores valid values.
2.  **Invalid Input Verification**: Ensure the descriptor raises appropriate exceptions (TypeError, ValueError) for bad data.
3.  **Instance Isolation**: Verify that descriptor state is stored per-instance, not on the descriptor itself.

## Pytest Implementation

```python
import pytest
from patterns.Descriptor import InventoryItem

def test_descriptor_valid_assignment() -> None:
    item = InventoryItem("Box", 50)
    assert item.quantity == 50
    
    item.quantity = 100
    assert item.quantity == 100

def test_descriptor_invalid_type() -> None:
    item = InventoryItem("Box", 10)
    with pytest.raises(TypeError, match="Expected quantity to be an int"):
        item.quantity = "many"  # type: ignore

def test_descriptor_invalid_value() -> None:
    item = InventoryItem("Box", 10)
    with pytest.raises(ValueError, match="Expected quantity to be non-negative"):
        item.quantity = -1

def test_descriptor_instance_isolation() -> None:
    item1 = InventoryItem("Item 1", 10)
    item2 = InventoryItem("Item 2", 20)
    
    assert item1.quantity == 10
    assert item2.quantity == 20
```

## Complementary Design Pattern
- [Descriptor](../patterns/Descriptor.md)
