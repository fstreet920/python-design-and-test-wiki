# Flyweight Pattern

The **Flyweight Pattern** is a structural design pattern that lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

## Intent
Use sharing to support large numbers of fine-grained objects efficiently.

## Python 3.12+ Implementation
Using a factory to manage shared flyweights.

```python
import json
from typing import Any

class Flyweight:
    """The Flyweight stores a common portion of the state (also called intrinsic state) that belongs to multiple real business entities."""
    def __init__(self, shared_state: Any) -> None:
        self._shared_state = shared_state

    def operation(self, unique_state: Any) -> None:
        s = json.dumps(self._shared_state)
        u = json.dumps(unique_state)
        print(f"Flyweight: Displaying shared ({s}) and unique ({u}) state.", end="")

class FlyweightFactory:
    """The Flyweight Factory creates and manages the Flyweight objects."""
    _flyweights: dict[str, Flyweight] = {}

    def __init__(self, initial_flyweights: list[list[Any]]) -> None:
        for state in initial_flyweights:
            self._flyweights[self.get_key(state)] = Flyweight(state)

    def get_key(self, state: list[Any]) -> str:
        """Returns a Flyweight's string hash for a given state."""
        return "_".join(sorted(state))

    def get_flyweight(self, shared_state: list[Any]) -> Flyweight:
        """Returns an existing Flyweight with a given state or creates a new one."""
        key = self.get_key(shared_state)

        if not self._flyweights.get(key):
            print("FlyweightFactory: Can't find a flyweight, creating new one.")
            self._flyweights[key] = Flyweight(shared_state)
        else:
            print("FlyweightFactory: Reusing existing flyweight.")

        return self._flyweights[key]

    def list_flyweights(self) -> None:
        count = len(self._flyweights)
        print(f"FlyweightFactory: I have {count} flyweights:")
        print("\n".join(self._flyweights.keys()))

def add_car_to_database(
    factory: FlyweightFactory, plates: str, owner: str,
    brand: str, model: str, color: str
) -> None:
    print("\n\nClient: Adding a car to database.")
    flyweight = factory.get_flyweight([brand, model, color])
    # The client code either stores or calculates extrinsic state and passes it
    # to the flyweight's methods.
    flyweight.operation([plates, owner])

if __name__ == "__main__":
    factory = FlyweightFactory([
        ["Chevrolet", "Camaro2018", "pink"],
        ["Mercedes Benz", "C300", "black"],
        ["Mercedes Benz", "C500", "red"],
        ["BMW", "M5", "red"],
        ["BMW", "X6", "white"],
    ])

    factory.list_flyweights()

    add_car_to_database(
        factory, "CL234IR", "James Doe", "BMW", "M5", "red"
    )

    add_car_to_database(
        factory, "CL234IR", "James Doe", "BMW", "X1", "red"
    )

    print("\n")
    factory.list_flyweights()
```

## Complementary Testing Pattern
- [Flyweight_Harness](../testing/Flyweight_Harness.md)
