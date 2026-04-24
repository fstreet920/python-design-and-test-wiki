# Strategy Harness

The **Strategy Harness** pattern provides a standardized way to test the Strategy Pattern in Python, ensuring that the context correctly uses interchangeable strategies and that each strategy performs its algorithm correctly.

## Strategy
1.  **Isolated Strategy Testing:** Test each concrete strategy independently to verify the correctness of its algorithm.
2.  **Context Delegation:** Use mock strategies to verify that the `Context` correctly calls the `do_algorithm` method.
3.  **Dynamic Swapping:** Verify that the `Context` can change its strategy at runtime and that subsequent calls use the new strategy.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Strategy import Context, Strategy, ConcreteStrategyA, ConcreteStrategyB

@pytest.fixture
def data() -> list[str]:
    return ["c", "a", "b"]

def test_concrete_strategy_a(data: list[str]) -> None:
    """Verify normal sorting strategy."""
    strategy = ConcreteStrategyA()
    assert strategy.do_algorithm(data) == ["a", "b", "c"]

def test_concrete_strategy_b(data: list[str]) -> None:
    """Verify reverse sorting strategy."""
    strategy = ConcreteStrategyB()
    assert strategy.do_algorithm(data) == ["c", "b", "a"]

def test_context_delegates_to_strategy(data: list[str]) -> None:
    """Verify that Context delegates the algorithm to the strategy."""
    mock_strategy = MagicMock(spec=Strategy)
    context = Context(mock_strategy)
    
    context.do_some_business_logic(data)
    mock_strategy.do_algorithm.assert_called_once_with(data)

def test_context_strategy_swapping(data: list[str]) -> None:
    """Verify that the strategy can be changed at runtime."""
    context = Context(ConcreteStrategyA())
    assert context.strategy.do_algorithm(data) == ["a", "b", "c"]
    
    context.strategy = ConcreteStrategyB()
    assert context.strategy.do_algorithm(data) == ["c", "b", "a"]

def test_strategy_protocol_compliance() -> None:
    """Verify that concrete strategies satisfy the Strategy protocol."""
    assert isinstance(ConcreteStrategyA(), Strategy)
    assert isinstance(ConcreteStrategyB(), Strategy)
```

## Complementary Design Pattern
- [Strategy](../patterns/Strategy.md)
