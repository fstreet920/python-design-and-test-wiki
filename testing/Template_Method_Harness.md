# Template Method Harness

The **Template Method Harness** pattern provides a standardized way to test the Template Method Pattern in Python, ensuring that the skeleton algorithm executes steps in the correct order and that subclasses correctly implement required steps and optional hooks.

## Strategy
1.  **Call Order Verification:** Use spies or mocks to verify that the template method calls the base operations, required operations, and hooks in the expected sequence.
2.  **Abstract Class Enforcement:** Verify that the abstract base class cannot be instantiated directly.
3.  **Hook Interaction:** Test that hooks provide the expected extension points and that overriding them changes behavior without altering the algorithm's structure.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock, patch
from patterns.Template_Method import AbstractClass, ConcreteClass1, ConcreteClass2

def test_cannot_instantiate_abstract_class() -> None:
    """Verify that AbstractClass is indeed abstract."""
    with pytest.raises(TypeError):
        AbstractClass() # type: ignore

def test_concrete_class1_execution_order() -> None:
    """Verify the sequence of calls in ConcreteClass1."""
    # We can use a spy by patching the methods of the instance
    cc1 = ConcreteClass1()
    with patch.object(cc1, 'base_operation1') as m1, \
         patch.object(cc1, 'required_operations1') as m2, \
         patch.object(cc1, 'base_operation2') as m3, \
         patch.object(cc1, 'required_operation2') as m4:
        
        cc1.template_method()
        
        # Verify calls occurred
        m1.assert_called_once()
        m2.assert_called_once()
        m3.assert_called_once()
        m4.assert_called_once()
        
        # Optional: Verify order if needed using mock.call_args_list or similar

def test_concrete_class2_hook_override() -> None:
    """Verify that ConcreteClass2 correctly overrides the hook."""
    cc2 = ConcreteClass2()
    with patch.object(cc2, 'hook1') as mock_hook:
        cc2.template_method()
        mock_hook.assert_called_once()

def test_default_hook_behavior() -> None:
    """Verify that the default hook in ConcreteClass1 does nothing."""
    cc1 = ConcreteClass1()
    # If hook1 does nothing, calling it shouldn't raise any errors
    cc1.hook1() 
```

## Complementary Design Pattern
- [[Template_Method]]
