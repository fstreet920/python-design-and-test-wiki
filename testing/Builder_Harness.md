# Builder Harness

The **Builder Harness** provides a standardized way to test the Builder Pattern, focusing on the correct sequence of construction steps and the integrity of the final product.

## Strategy
1.  **Fluent Interface Verification:** Ensure that builder methods return `self` to support method chaining.
2.  **Step Isolation:** Verify that each builder method correctly adds the intended part without affecting others.
3.  **Director Coordination:** Test the Director's ability to orchestrate different construction sequences using a mock builder.
4.  **Product State:** Verify the final state of the product after various construction sequences.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Builder import Builder, ConcreteBuilder, Director, ConcreteProduct

@pytest.fixture
def builder() -> ConcreteBuilder:
    return ConcreteBuilder()

@pytest.fixture
def director() -> Director:
    return Director()

def test_builder_fluent_interface(builder: ConcreteBuilder) -> None:
    """Verify that builder methods return the builder instance for chaining."""
    assert builder.produce_part_a() is builder
    assert builder.produce_part_b() is builder
    assert builder.produce_part_c() is builder

def test_builder_produces_parts(builder: ConcreteBuilder) -> None:
    """Verify that builder methods correctly add parts to the product."""
    builder.produce_part_a().produce_part_b()
    product = builder.product
    assert "PartA1" in product.parts
    assert "PartB1" in product.parts
    assert "PartC1" not in product.parts

def test_builder_reset_after_getting_product(builder: ConcreteBuilder) -> None:
    """Verify that the builder resets after the product is retrieved."""
    builder.produce_part_a()
    _ = builder.product
    new_product = builder.product
    assert len(new_product.parts) == 0

def test_director_builds_minimal_product(director: Director) -> None:
    """Verify that the director correctly orchestrates a minimal build."""
    mock_builder = MagicMock(spec=Builder)
    director.builder = mock_builder
    
    director.build_minimal_viable_product()
    
    mock_builder.produce_part_a.assert_called_once()
    mock_builder.produce_part_b.assert_not_called()

def test_director_builds_full_product(director: Director) -> None:
    """Verify that the director correctly orchestrates a full build."""
    mock_builder = MagicMock(spec=Builder)
    # Mocking fluent interface
    mock_builder.produce_part_a.return_value = mock_builder
    mock_builder.produce_part_b.return_value = mock_builder
    mock_builder.produce_part_c.return_value = mock_builder
    
    director.builder = mock_builder
    director.build_full_featured_product()
    
    mock_builder.produce_part_a.assert_called_once()
    mock_builder.produce_part_b.assert_called_once()
    mock_builder.produce_part_c.assert_called_once()
```

## Complementary Design Pattern
- [[Builder]]
