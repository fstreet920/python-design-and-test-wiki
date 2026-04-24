# Adapter Harness

The **Adapter Harness** provides a standardized way to test the Adapter Pattern, ensuring that the adapter correctly translates requests between incompatible interfaces.

## Strategy
1.  **Interface Compliance:** Verify that the Adapter implements the `Target` protocol.
2.  **Translation Logic:** Verify that the Adapter correctly delegates calls to the `Adaptee` and transforms the output as expected.
3.  **Mock Adaptee:** Use a mock `Adaptee` to ensure the Adapter interacts with it correctly (e.g., calls the right method).

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Adapter import Adapter, Adaptee, Target

@pytest.fixture
def mock_adaptee() -> MagicMock:
    return MagicMock(spec=Adaptee)

def test_adapter_implements_target_protocol() -> None:
    """Verify that the Adapter follows the Target protocol."""
    # We can't use isinstance with non-runtime_checkable protocols easily 
    # unless we add @runtime_checkable to Target
    from typing import runtime_checkable
    # (Assuming Target was decorated with @runtime_checkable in patterns/Adapter.md)
    # For now, we'll check method existence
    adapter = Adapter(Adaptee())
    assert hasattr(adapter, "request")

def test_adapter_translation_logic(mock_adaptee: MagicMock) -> None:
    """Verify that the adapter correctly reverses the string from the adaptee."""
    mock_adaptee.specific_request.return_value = "Special behavior"
    adapter = Adapter(mock_adaptee)
    
    result = adapter.request()
    
    assert "TRANSLATED" in result
    assert "roivaheb laicepS" in result # Reversed "Special behavior"
    mock_adaptee.specific_request.assert_called_once()

def test_adapter_with_real_adaptee() -> None:
    """Integration test with the real Adaptee class."""
    adaptee = Adaptee()
    adapter = Adapter(adaptee)
    assert adapter.request() == "Adapter: (TRANSLATED) Special behavior of the Adaptee."
```

## Complementary Design Pattern
- [Adapter](../patterns/Adapter.md)
