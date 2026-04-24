# Abstract Factory Pattern

The **Abstract Factory Pattern** is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

## Intent
Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

## Python 3.12+ Implementation
Using `typing.Protocol` for structural typing and Python 3.12's PEP 695 generics.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class AbstractProductA(Protocol):
    def useful_function_a(self) -> str:
        ...

@runtime_checkable
class AbstractProductB(Protocol):
    def useful_function_b(self) -> str:
        ...

    def another_useful_function_b(self, collaborator: AbstractProductA) -> str:
        ...

class AbstractFactory(Protocol):
    def create_product_a(self) -> AbstractProductA:
        ...

    def create_product_b(self) -> AbstractProductB:
        ...

# Concrete Products A
class ConcreteProductA1:
    def useful_function_a(self) -> str:
        return "The result of the product A1."

class ConcreteProductA2:
    def useful_function_a(self) -> str:
        return "The result of the product A2."

# Concrete Products B
class ConcreteProductB1:
    def useful_function_b(self) -> str:
        return "The result of the product B1."

    def another_useful_function_b(self, collaborator: AbstractProductA) -> str:
        result = collaborator.useful_function_a()
        return f"The result of the B1 collaborating with ({result})"

class ConcreteProductB2:
    def useful_function_b(self) -> str:
        return "The result of the product B2."

    def another_useful_function_b(self, collaborator: AbstractProductA) -> str:
        result = collaborator.useful_function_a()
        return f"The result of the B2 collaborating with ({result})"

# Concrete Factories
class ConcreteFactory1:
    def create_product_a(self) -> AbstractProductA:
        return ConcreteProductA1()

    def create_product_b(self) -> AbstractProductB:
        return ConcreteProductB1()

class ConcreteFactory2:
    def create_product_a(self) -> AbstractProductA:
        return ConcreteProductA2()

    def create_product_b(self) -> AbstractProductB:
        return ConcreteProductB2()

def client_code(factory: AbstractFactory) -> None:
    product_a = factory.create_product_a()
    product_b = factory.create_product_b()

    print(f"{product_b.useful_function_b()}")
    print(f"{product_b.another_useful_function_b(product_a)}")

if __name__ == "__main__":
    print("Client: Testing client code with the first factory type:")
    client_code(ConcreteFactory1())

    print("\nClient: Testing the same client code with the second factory type:")
    client_code(ConcreteFactory2())
```

## Complementary Testing Pattern
- [[Abstract_Factory_Harness]]
