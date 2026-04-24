# Parametrization

**Parametrization** is a testing pattern that decouples test logic from test data, allowing the same test scenario to be executed with multiple sets of inputs and expected outcomes.

## Strategy
1.  **Declarative Data**: Use `@pytest.mark.parametrize` to define a list of input/output tuples.
2.  **Logic Reuse**: The test function remains generic, focusing only on the assertion logic.
3.  **Test Explosion Prevention**: Avoids "Copy-Paste" anti-patterns by centralizing the test scenario.

## Pytest Implementation

```python
import pytest

@pytest.mark.parametrize(
    "input_str, expected_len",
    [
        ("hello", 5),
        ("", 0),
        ("pytest", 6),
        (" ", 1),
    ]
)
def test_string_length(input_str: str, expected_len: int) -> None:
    assert len(input_str) == expected_len
```

## Complementary Design Patterns
- [Strategy](../patterns/Strategy.md) - Parametrization is effectively the Strategy pattern applied to test data; it allows you to test interchangeable data sets against a fixed algorithm.
- [Template_Method](../patterns/Template_Method.md) - Useful when testing variations of a structural algorithm.
