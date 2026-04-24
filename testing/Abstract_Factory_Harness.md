# Abstract Factory Harness

The **Abstract Factory Harness** provides a standardized way to test the Abstract Factory Pattern, ensuring that factories produce compatible products and that products adhere to their protocols.

## Strategy
1.  **Protocol Validation:** Use `isinstance` with `@runtime_checkable` protocols to ensure concrete products and factories implement the required interfaces.
2.  **Compatibility Testing:** Verify that products created by the same factory work together correctly.
3.  **Factory Isolation:** Test the client code by passing in mock factories to ensure it doesn't depend on concrete factory implementations.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Abstract_Factory import (
    AbstractFactory, AbstractProductA, AbstractProductB,
    ConcreteFactory1, ConcreteFactory2,
    ConcreteProductA1, ConcreteProductB1
)

@pytest.mark.parametrize("factory_class", [ConcreteFactory1, ConcreteFactory2])
def test_factory_implements_protocol(factory_class: type[AbstractFactory]) -> None:
    """Verify that concrete factories implement the AbstractFactory protocol."""
    factory = factory_class()
    assert isinstance(factory, AbstractFactory)

@pytest.mark.parametrize("factory", [ConcreteFactory1(), ConcreteFactory2()])
def test_factory_creates_compatible_products(factory: AbstractFactory) -> None:
    """Verify that factories create products that follow their respective protocols."""
    product_a = factory.create_product_a()
    product_b = factory.create_product_b()
    
    assert isinstance(product_a, AbstractProductA)
    assert isinstance(product_b, AbstractProductB)

def test_concrete_product_b1_collaboration() -> None:
    """Verify specific collaboration logic in a concrete product."""
    product_a = ConcreteProductA1()
    product_b = ConcreteProductB1()
    
    result = product_b.another_useful_function_b(product_a)
    assert "The result of the product A1." in result
    assert "B1 collaborating" in result

def test_client_code_with_mock_factory() -> None:
    """Verify that client code interacts correctly with any AbstractFactory."""
    mock_factory = MagicMock(spec=AbstractFactory)
    mock_product_a = MagicMock(spec=AbstractProductA)
    mock_product_b = MagicMock(spec=AbstractProductB)
    
    mock_factory.create_product_a.return_value = mock_product_a
    mock_factory.create_product_b.return_value = mock_product_b
    
    # Simulate client code usage
    pa = mock_factory.create_product_a()
    pb = mock_factory.create_product_b()
    pb.another_useful_function_b(pa)
    
    mock_factory.create_product_a.assert_called_once()
    mock_factory.create_product_b.assert_called_once()
    mock_product_b.another_useful_function_b.assert_called_once_with(mock_product_a)
```

## Complementary Design Pattern
- [[Abstract_Factory]]
