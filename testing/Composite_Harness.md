# Composite Harness

The **Composite Harness** provides a standardized way to test the Composite Pattern, focusing on uniform treatment of leaf and composite nodes and correct recursive execution.

## Strategy
1.  **Uniformity Verification:** Ensure that both `Leaf` and `Composite` implement the `Component` protocol.
2.  **Recursive Execution:** Verify that calling `operation()` on a composite correctly triggers `operation()` on all its children.
3.  **Tree Management:** Test adding and removing components from a composite.
4.  **Leaf Integrity:** Verify that leaves correctly refuse or ignore management operations (add/remove).

## Pytest Implementation

```python
import pytest
from patterns.Composite import Component, Leaf, Composite

def test_leaf_operation() -> None:
    """Verify simple leaf operation."""
    leaf = Leaf()
    assert leaf.operation() == "Leaf"
    assert not leaf.is_composite()

def test_composite_operation_with_children() -> None:
    """Verify composite correctly aggregates child operations."""
    tree = Composite()
    tree.add(Leaf())
    tree.add(Leaf())
    
    assert tree.operation() == "Branch(Leaf+Leaf)"
    assert tree.is_composite()

def test_nested_composite_operation() -> None:
    """Verify recursive aggregation in nested composites."""
    root = Composite()
    branch = Composite()
    branch.add(Leaf())
    root.add(branch)
    root.add(Leaf())
    
    assert root.operation() == "Branch(Branch(Leaf)+Leaf)"

def test_composite_remove() -> None:
    """Verify removing a component from a composite."""
    tree = Composite()
    leaf = Leaf()
    tree.add(leaf)
    tree.remove(leaf)
    assert tree.operation() == "Branch()"

def test_leaf_raises_on_add() -> None:
    """Verify that leaves raise NotImplementedError on management operations."""
    leaf = Leaf()
    with pytest.raises(NotImplementedError):
        leaf.add(Composite())
```

## Complementary Design Pattern
- [Composite](../patterns/Composite.md)
