# Template Method Pattern

The **Template Method Pattern** is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

## Intent
Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

## Python 3.12+ Implementation
Using an abstract base class (ABC) to define the template and hooks.

```python
from abc import ABC, abstractmethod

class AbstractClass(ABC):
    """
    The Abstract Class defines a template method that contains a skeleton of some
    algorithm, composed of calls to (usually) abstract primitive operations.
    """

    def template_method(self) -> None:
        """
        The template method defines the skeleton of an algorithm.
        """
        self.base_operation1()
        self.required_operations1()
        self.base_operation2()
        self.hook1()
        self.required_operation2()
        self.base_operation3()
        self.hook2()

    # These operations already have implementations.
    def base_operation1(self) -> None:
        print("AbstractClass says: I am doing the bulk of the work")

    def base_operation2(self) -> None:
        print("AbstractClass says: But I let subclasses override some operations")

    def base_operation3(self) -> None:
        print("AbstractClass says: But I am am in charge of the process anyway")

    # These operations have to be implemented in subclasses.
    @abstractmethod
    def required_operations1(self) -> None:
        pass

    @abstractmethod
    def required_operation2(self) -> None:
        pass

    # These are "hooks." Subclasses may override them, but it's not mandatory
    # since the hooks already have default (but empty) implementation. Hooks
    # provide additional extension points in some crucial places of the algorithm.
    def hook1(self) -> None:
        pass

    def hook2(self) -> None:
        pass

class ConcreteClass1(AbstractClass):
    """
    Concrete classes have to implement all abstract operations of the base class.
    They can also override some operations with a default implementation.
    """
    def required_operations1(self) -> None:
        print("ConcreteClass1 says: Implemented RequiredOperations1")

    def required_operation2(self) -> None:
        print("ConcreteClass1 says: Implemented RequiredOperation2")

class ConcreteClass2(AbstractClass):
    """
    Usually, concrete classes override only a fraction of base class' operations.
    """
    def required_operations1(self) -> None:
        print("ConcreteClass2 says: Implemented RequiredOperations1")

    def required_operation2(self) -> None:
        print("ConcreteClass2 says: Implemented RequiredOperation2")

    def hook1(self) -> None:
        print("ConcreteClass2 says: Overridden Hook1")

# Usage
if __name__ == "__main__":
    print("Same client code can work with different subclasses:")
    ConcreteClass1().template_method()
    print("\n")
    ConcreteClass2().template_method()
```

## Complementary Testing Pattern
- [Template_Method_Harness](../testing/Template_Method_Harness.md)
