# Specification Pattern

The **Specification Pattern** is a behavioral design pattern used to codify business rules that state whether an object satisfies some criteria. It is particularly useful for building complex queries and validation logic that can be combined using logical operators.

## Intent
Encapsulate a business rule in a reusable object. Specifications can be combined to create complex rules from simple ones, making the domain logic more expressive and easier to test.

## Python 3.12+ Implementation
Using PEP 695 generics and operator overloading for a fluent interface.

```python
from typing import Protocol, runtime_checkable
from dataclasses import dataclass

@dataclass
class User:
    username: str
    age: int
    is_active: bool

@runtime_checkable
class Specification[T](Protocol):
    """The Specification interface."""
    def is_satisfied_by(self, candidate: T) -> bool:
        ...

    def __and__(self, other: "Specification[T]") -> "AndSpecification[T]":
        return AndSpecification(self, other)

    def __or__(self, other: "Specification[T]") -> "OrSpecification[T]":
        return OrSpecification(self, other)

    def __invert__(self) -> "NotSpecification[T]":
        return NotSpecification(self)

@dataclass
class AndSpecification[T]:
    left: Specification[T]
    right: Specification[T]

    def is_satisfied_by(self, candidate: T) -> bool:
        return self.left.is_satisfied_by(candidate) and self.right.is_satisfied_by(candidate)

@dataclass
class OrSpecification[T]:
    left: Specification[T]
    right: Specification[T]

    def is_satisfied_by(self, candidate: T) -> bool:
        return self.left.is_satisfied_by(candidate) or self.right.is_satisfied_by(candidate)

@dataclass
class NotSpecification[T]:
    spec: Specification[T]

    def is_satisfied_by(self, candidate: T) -> bool:
        return not self.spec.is_satisfied_by(candidate)

# Concrete Specifications
class UserIsActive:
    def is_satisfied_by(self, candidate: User) -> bool:
        return candidate.is_active

class UserIsAdult:
    def is_satisfied_by(self, candidate: User) -> bool:
        return candidate.age >= 18

# Usage
if __name__ == "__main__":
    active_adult_spec = UserIsActive() & UserIsAdult()
    
    user1 = User("alice", 25, True)
    user2 = User("bob", 15, True)
    
    print(f"Alice satisfies: {active_adult_spec.is_satisfied_by(user1)}")
    print(f"Bob satisfies: {active_adult_spec.is_satisfied_by(user2)}")
```

## Complementary Testing Pattern
- [Specification_Harness](../testing/Specification_Harness.md)
