# Builder Pattern

The **Builder Pattern** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

## Intent
Separate the construction of a complex object from its representation so that the same construction process can create different representations.

## Python 3.12+ Implementation
Using Python 3.12's PEP 695 generics for the builder's return type.

```python
from typing import Self, Protocol, runtime_checkable

@runtime_checkable
class Product(Protocol):
    parts: list[str]
    def list_parts(self) -> None:
        ...

class ConcreteProduct:
    def __init__(self) -> None:
        self.parts: list[str] = []

    def add(self, part: str) -> None:
        self.parts.append(part)

    def list_parts(self) -> None:
        print(f"Product parts: {', '.join(self.parts)}", end="")

class Builder(Protocol):
    def produce_part_a(self) -> Self:
        ...
    def produce_part_b(self) -> Self:
        ...
    def produce_part_c(self) -> Self:
        ...

class ConcreteBuilder:
    def __init__(self) -> None:
        self.reset()

    def reset(self) -> None:
        self._product = ConcreteProduct()

    @property
    def product(self) -> ConcreteProduct:
        product = self._product
        self.reset()
        return product

    def produce_part_a(self) -> Self:
        self._product.add("PartA1")
        return self

    def produce_part_b(self) -> Self:
        self._product.add("PartB1")
        return self

    def produce_part_c(self) -> Self:
        self._product.add("PartC1")
        return self

class Director:
    def __init__(self) -> None:
        self._builder: Builder | None = None

    @property
    def builder(self) -> Builder | None:
        return self._builder

    @builder.setter
    def builder(self, builder: Builder) -> None:
        self._builder = builder

    def build_minimal_viable_product(self) -> None:
        if self._builder:
            self._builder.produce_part_a()

    def build_full_featured_product(self) -> None:
        if self._builder:
            self._builder.produce_part_a().produce_part_b().produce_part_c()

if __name__ == "__main__":
    director = Director()
    builder = ConcreteBuilder()
    director.builder = builder

    print("Standard basic product: ")
    director.build_minimal_viable_product()
    builder.product.list_parts()

    print("\n\nStandard full featured product: ")
    director.build_full_featured_product()
    builder.product.list_parts()

    print("\n\nCustom product: ")
    builder.produce_part_a().produce_part_c()
    builder.product.list_parts()
```

## Complementary Testing Pattern
- [[Builder_Harness]]
