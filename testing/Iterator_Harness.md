# Iterator Harness

The **Iterator Harness** provides a standardized way to test the Iterator Pattern, focusing on correct traversal sequences and adherence to Python's iterator protocol.

## Strategy
1.  **StopIteration Verification:** Ensure that the iterator correctly raises `StopIteration` after the last element has been traversed.
2.  **Traversal Order:** Verify that the iterator returns elements in the expected order (e.g., straight, reverse).
3.  **Independence:** Verify that multiple iterators over the same collection maintain their own state and can traverse independently.
4.  **Generic Support:** Test the iterator with different data types to ensure PEP 695 generics are handled correctly.

## Pytest Implementation

```python
import pytest
from patterns.Iterator import WordsCollection, AlphabeticalOrderIterator

@pytest.fixture
def collection() -> WordsCollection[str]:
    c = WordsCollection[str]()
    c.add_item("A")
    c.add_item("B")
    c.add_item("C")
    return c

def test_straight_traversal(collection: WordsCollection[str]) -> None:
    """Verify standard forward traversal."""
    results = list(collection)
    assert results == ["A", "B", "C"]

def test_reverse_traversal(collection: WordsCollection[str]) -> None:
    """Verify reverse traversal logic."""
    results = list(collection.get_reverse_iterator())
    assert results == ["C", "B", "A"]

def test_iterator_raises_stop_iteration() -> None:
    """Verify that StopIteration is raised at the end."""
    it = AlphabeticalOrderIterator(["One"])
    next(it)
    with pytest.raises(StopIteration):
        next(it)

def test_independent_iterators(collection: WordsCollection[str]) -> None:
    """Verify that multiple iterators don't interfere with each other."""
    it1 = iter(collection)
    it2 = iter(collection)
    
    assert next(it1) == "A"
    assert next(it2) == "A"
    assert next(it1) == "B"
    assert next(it2) == "B"

def test_generic_int_collection() -> None:
    """Verify that the collection works with integers (Generics)."""
    c = WordsCollection[int]()
    c.add_item(1)
    c.add_item(2)
    assert list(c) == [1, 2]
```

## Complementary Design Pattern
- [Iterator](../patterns/Iterator.md)
