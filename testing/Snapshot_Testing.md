# Snapshot Testing

**Snapshot Testing** is used to verify that complex data structures or UI outputs do not change unexpectedly by comparing them against a stored "golden" representation.

## Strategy
1.  **Initial Capture**: Run the test to generate and save a snapshot file.
2.  **Regression Check**: Subsequent runs compare the current output against the saved snapshot.
3.  **Explicit Updates**: Changes are only accepted when the developer explicitly updates the snapshot file.

## Pytest Implementation
Using `syrupy` (recommended for modern Python).

```python
import pytest

def test_complex_api_response(snapshot: Any) -> None:
    response = {
        "id": 123,
        "status": "active",
        "metadata": {
            "created_at": "2026-04-23",
            "tags": ["python", "design-patterns", "testing"]
        }
    }
    
    # This will compare 'response' against the saved snapshot in __snapshots__
    assert response == snapshot
```

## Complementary Design Patterns
- [Memento](../patterns/Memento.md) - Snapshot testing is an externalized form of the Memento pattern used for state verification.
- [Composite](../patterns/Composite.md) - Excellent for verifying complex tree structures.
- [Visitor](../patterns/Visitor.md) - Useful for capturing the results of an operation performed across a complex object structure.
