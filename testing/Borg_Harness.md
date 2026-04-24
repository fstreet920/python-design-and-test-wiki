# Borg Harness

The **Borg Harness** verifies that all instances of a Borg class share identical state and that modifications to one instance are reflected in all others.

## Strategy
1.  **Identity vs. Equality**: Verify that instances are different objects (`is not`), but share the same state (`__dict__`).
2.  **Cross-Instance Verification**: Modify an attribute on one instance and assert the change on another.

## Pytest Implementation

```python
import pytest
from patterns.Borg import AppConfig

def test_borg_state_sharing() -> None:
    c1 = AppConfig(debug=True)
    c2 = AppConfig()
    
    # Verify different instances
    assert c1 is not c2
    
    # Verify shared state
    assert c2.debug is True
    
    # Verify modification
    c1.theme = "light"
    assert c2.theme == "light"

def test_borg_initialization_overlap() -> None:
    # Ensure second init doesn't accidentally wipe first init state 
    # unless intended by the implementation logic.
    c1 = AppConfig(val=10)
    c2 = AppConfig(val=20)
    
    assert c1.val == 20
    assert c1.__dict__ is c2.__dict__
```

## Complementary Design Pattern
- [[Borg]]
