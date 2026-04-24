# Factory Method Harness

The **Factory Method Harness** provides a standardized way to test the Factory Method Pattern, ensuring that creators produce the correct products and that the base creator logic remains robust.

## Strategy
1.  **Product Validation:** Verify that the factory method returns an object that adheres to the `Product` protocol.
2.  **Creator Logic:** Test the base creator's logic (e.g., `some_operation`) using different concrete creators or mock creators.
3.  **Subclass Independence:** Ensure that adding a new creator/product pair doesn't require changes to the client or base creator logic.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Factory_Method import (
    Product, Creator, ConcreteCreator1, ConcreteCreator2, 
    ConcreteProduct1, ConcreteProduct2
)

@pytest.mark.parametrize("creator_class, expected_product_type", [
    (ConcreteCreator1, ConcreteProduct1),
    (ConcreteCreator2, ConcreteProduct2),
])
def test_factory_method_returns_correct_product_type(creator_class: type[Creator], expected_product_type: type[Product]) -> None:
    """Verify that each concrete creator produces its specific product."""
    creator = creator_class()
    product = creator.factory_method()
    assert isinstance(product, expected_product_type)
    assert isinstance(product, Product)

def test_creator_operation_uses_factory_method() -> None:
    """Verify that the creator's operation correctly uses the product from the factory method."""
    creator = ConcreteCreator1()
    result = creator.some_operation()
    assert "ConcreteProduct1" in result

def test_creator_protocol_adherence() -> None:
    """Verify that creators follow the Creator protocol."""
    assert isinstance(ConcreteCreator1(), Creator)
    assert isinstance(ConcreteCreator2(), Creator)

def test_mock_creator_for_client_testing() -> None:
    """Verify client-side logic using a mock creator."""
    mock_creator = MagicMock(spec=Creator)
    mock_product = MagicMock(spec=Product)
    mock_product.operation.return_value = "MockResult"
    mock_creator.factory_method.return_value = mock_product
    
    # Simulate Creator.some_operation logic if needed or test client interaction
    product = mock_creator.factory_method()
    assert product.operation() == "MockResult"
    mock_creator.factory_method.assert_called_once()
```

## Complementary Design Pattern
- [[Factory_Method]]
