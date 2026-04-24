# Factory Method Pattern

The **Factory Method Pattern** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

## Intent
Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

## Python 3.12+ Implementation
Using `Protocol` for the product and PEP 695 generics for the creator.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Product(Protocol):
    def operation(self) -> str:
        ...

class Creator(Protocol):
    def factory_method(self) -> Product:
        ...

    def some_operation(self) -> str:
        product = self.factory_method()
        result = f"Creator: The same creator's code has just worked with {product.operation()}"
        return result

# Concrete Products
class ConcreteProduct1:
    def operation(self) -> str:
        return "{Result of ConcreteProduct1}"

class ConcreteProduct2:
    def operation(self) -> str:
        return "{Result of ConcreteProduct2}"

# Concrete Creators
class ConcreteCreator1:
    def factory_method(self) -> Product:
        return ConcreteProduct1()

class ConcreteCreator2:
    def factory_method(self) -> Product:
        return ConcreteProduct2()

def client_code(creator: Creator) -> None:
    print(f"Client: I'm not aware of the creator's class, but it still works.\n"
          f"{creator.some_operation()}", end="")

if __name__ == "__main__":
    print("App: Launched with ConcreteCreator1.")
    client_code(ConcreteCreator1())
    print("\n")

    print("App: Launched with ConcreteCreator2.")
    client_code(ConcreteCreator2())
```

## Complementary Testing Pattern
- [[Factory_Method_Harness]]
