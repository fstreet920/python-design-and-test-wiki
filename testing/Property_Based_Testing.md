# Property-Based Testing

**Property-Based Testing** shifts the focus from testing specific examples to testing the *properties* or invariants that should always hold true for a given range of inputs.

## Strategy
1.  **Invariants**: Define what should always be true (e.g., "sorting a list should not change its length").
2.  **Generative Inputs**: Use a tool like `Hypothesis` to generate hundreds of edge-case inputs.
3.  **Shrinking**: When a failure is found, the tool automatically simplifies the input to the smallest possible case that breaks the test.

## Pytest Implementation
Requires `hypothesis`.

```python
from hypothesis import given, strategies as st

def sort_list(numbers: list[int]) -> list[int]:
    return sorted(numbers)

@given(st.lists(st.integers()))
def test_sort_maintains_length(numbers: list[int]) -> None:
    sorted_numbers = sort_list(numbers)
    assert len(sorted_numbers) == len(numbers)

@given(st.lists(st.integers()))
def test_sort_is_idempotent(numbers: list[int]) -> None:
    # Property: sorting twice is the same as sorting once
    once = sort_list(numbers)
    twice = sort_list(once)
    assert once == twice
```

## Complementary Design Patterns
- [[Strategy]] - Ideal for verifying that different algorithmic strategies all maintain the same invariants.
- [[Interpreter]] - Useful for ensuring a grammar interpreter handles all possible token sequences within a specification.
