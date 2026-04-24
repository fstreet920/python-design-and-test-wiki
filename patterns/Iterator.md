# Iterator Pattern

The **Iterator Pattern** is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

## Intent
Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

## Python 3.12+ Implementation
Using Python's built-in `Iterator` and `Iterable` protocols with PEP 695 generics.

```python
from collections.abc import Iterable, Iterator
from typing import Any

class AlphabeticalOrderIterator[T](Iterator[T]):
    """
    Concrete Iterators implement various traversal algorithms. These classes
    store the current traversal position at all times.
    """
    _position: int = 0
    _reverse: bool = False

    def __init__(self, collection: list[T], reverse: bool = False) -> None:
        self._collection = collection
        self._reverse = reverse
        self._position = -1 if reverse else 0

    def __next__(self) -> T:
        """
        The __next__() method must return the next item in the sequence. On
        reaching the end, and in subsequent calls, it must raise StopIteration.
        """
        try:
            value = self._collection[self._position]
            self._position += -1 if self._reverse else 1
        except IndexError:
            raise StopIteration()

        return value

class WordsCollection[T](Iterable[T]):
    """
    Concrete Collections provide one or several methods for retrieving fresh
    iterator instances, compatible with the collection class.
    """
    def __init__(self, collection: list[T] = []) -> None:
        self._collection = collection

    def __iter__(self) -> AlphabeticalOrderIterator[T]:
        """
        The __iter__() method returns the iterator object itself, by default we
        return the iterator in ascending order.
        """
        return AlphabeticalOrderIterator(self._collection)

    def get_reverse_iterator(self) -> AlphabeticalOrderIterator[T]:
        return AlphabeticalOrderIterator(self._collection, True)

    def add_item(self, item: T) -> None:
        self._collection.append(item)

if __name__ == "__main__":
    # The client code may or may not know about the Concrete Iterator or
    # Collection classes, depending on the level of indirection you want to keep
    # in your program.
    collection = WordsCollection[str]()
    collection.add_item("First")
    collection.add_item("Second")
    collection.add_item("Third")

    print("Straight traversal:")
    print("\n".join(collection))
    print("")

    print("Reverse traversal:")
    print("\n".join(collection.get_reverse_iterator()), end="")
```

## Complementary Testing Pattern
- [Iterator_Harness](../testing/Iterator_Harness.md)
