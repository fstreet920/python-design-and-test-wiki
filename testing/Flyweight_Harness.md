# Flyweight Harness

The **Flyweight Harness** provides a standardized way to test the Flyweight Pattern, focusing on efficient object sharing and correct separation of intrinsic and extrinsic state.

## Strategy
1.  **Object Sharing Verification:** Verify that the Flyweight Factory returns the exact same object for the same shared state.
2.  **Factory State Management:** Test the ability to retrieve existing flyweights and create new ones as needed.
3.  **State Separation:** Verify that the flyweight correctly combines shared (intrinsic) state with passed-in unique (extrinsic) state during operations.
4.  **Memory Efficiency (Indirect):** Verify that the number of flyweight objects doesn't grow when requesting existing states.

## Pytest Implementation

```python
import pytest
from patterns.Flyweight import FlyweightFactory, Flyweight

@pytest.fixture
def factory() -> FlyweightFactory:
    return FlyweightFactory([
        ["Brand1", "Model1", "Color1"],
        ["Brand2", "Model2", "Color2"]
    ])

def test_factory_reuses_flyweight(factory: FlyweightFactory) -> None:
    """Verify that the factory returns the same instance for identical state."""
    state = ["Brand1", "Model1", "Color1"]
    f1 = factory.get_flyweight(state)
    f2 = factory.get_flyweight(state)
    
    assert f1 is f2

def test_factory_creates_new_flyweight_for_new_state(factory: FlyweightFactory) -> None:
    """Verify that the factory creates a new instance for a different state."""
    initial_count = len(factory._flyweights)
    state = ["NewBrand", "NewModel", "NewColor"]
    
    f = factory.get_flyweight(state)
    
    assert len(factory._flyweights) == initial_count + 1
    assert factory.get_key(state) in factory._flyweights

def test_flyweight_operation_combines_states(capsys: pytest.CaptureFixture[str]) -> None:
    """Verify that the flyweight correctly uses both shared and unique state."""
    shared = {"brand": "Tesla"}
    unique = {"owner": "Elon"}
    flyweight = Flyweight(shared)
    
    flyweight.operation(unique)
    
    captured = capsys.readouterr()
    assert "Tesla" in captured.out
    assert "Elon" in captured.out

def test_key_generation_is_stable(factory: FlyweightFactory) -> None:
    """Verify that key generation is independent of attribute order."""
    state1 = ["A", "B", "C"]
    state2 = ["C", "B", "A"]
    assert factory.get_key(state1) == factory.get_key(state2)
```

## Complementary Design Pattern
- [Flyweight](../patterns/Flyweight.md)
