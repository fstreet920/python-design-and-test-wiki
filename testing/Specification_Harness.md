# Specification Harness

The **Specification Harness** ensures that business rules are correctly evaluated and that logical combinations (AND, OR, NOT) behave according to Boolean logic.

## Strategy
1.  **Atomic Verification:** Test each concrete specification individually with positive and negative cases.
2.  **Logical Composition:** Test combined specifications (e.g., `A & B`, `A | B`, `~A`) to verify that the operators are correctly implemented.
3.  **Complex Nesting:** Verify that deeply nested specifications (e.g., `(A & B) | C`) evaluate correctly.

## Pytest Implementation

```python
import pytest
from patterns.Specification import User, UserIsActive, UserIsAdult

@pytest.fixture
def active_adult() -> User:
    return User("active_adult", 25, True)

@pytest.fixture
def inactive_adult() -> User:
    return User("inactive_adult", 25, False)

@pytest.fixture
def active_child() -> User:
    return User("active_child", 10, True)

def test_atomic_spec_active(active_adult, inactive_adult) -> None:
    spec = UserIsActive()
    assert spec.is_satisfied_by(active_adult) is True
    assert spec.is_satisfied_by(inactive_adult) is False

def test_and_specification(active_adult, inactive_adult, active_child) -> None:
    spec = UserIsActive() & UserIsAdult()
    assert spec.is_satisfied_by(active_adult) is True
    assert spec.is_satisfied_by(inactive_adult) is False
    assert spec.is_satisfied_by(active_child) is False

def test_or_specification(inactive_adult, active_child) -> None:
    spec = UserIsActive() | UserIsAdult()
    assert spec.is_satisfied_by(inactive_adult) is True  # Adult
    assert spec.is_satisfied_by(active_child) is True    # Active

def test_not_specification(active_adult) -> None:
    spec = ~UserIsActive()
    assert spec.is_satisfied_by(active_adult) is False
```

## Complementary Design Pattern
- [Specification](../patterns/Specification.md)
