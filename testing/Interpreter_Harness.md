# Interpreter Harness

The **Interpreter Harness** provides a standardized way to test the Interpreter Pattern, focusing on the correct evaluation of terminal and non-terminal expressions.

## Strategy
1.  **Terminal Expression Verification:** Verify that terminal expressions correctly identify their presence in the context.
2.  **Logical Composition:** Verify that `AndExpression` and `OrExpression` correctly combine the results of their sub-expressions.
3.  **Complex Expression Evaluation:** Test nested expressions to ensure correct recursive evaluation.
4.  **Context Sensitivity:** Verify that the interpreter correctly handles different context strings.

## Pytest Implementation

```python
import pytest
from patterns.Interpreter import TerminalExpression, OrExpression, AndExpression

def test_terminal_expression() -> None:
    """Verify terminal expression identifies data in context."""
    expr = TerminalExpression("Hello")
    assert expr.interpret("Hello World") is True
    assert expr.interpret("Goodbye") is False

def test_or_expression() -> None:
    """Verify OR logic between two expressions."""
    expr1 = TerminalExpression("A")
    expr2 = TerminalExpression("B")
    or_expr = OrExpression(expr1, expr2)
    
    assert or_expr.interpret("A") is True
    assert or_expr.interpret("B") is True
    assert or_expr.interpret("C") is False

def test_and_expression() -> None:
    """Verify AND logic between two expressions."""
    expr1 = TerminalExpression("A")
    expr2 = TerminalExpression("B")
    and_expr = AndExpression(expr1, expr2)
    
    assert and_expr.interpret("A B") is True
    assert and_expr.interpret("A") is False
    assert and_expr.interpret("B") is False

def test_nested_expressions() -> None:
    """Verify complex nested expression evaluation."""
    # (A AND B) OR C
    a = TerminalExpression("A")
    b = TerminalExpression("B")
    c = TerminalExpression("C")
    
    complex_expr = OrExpression(AndExpression(a, b), c)
    
    assert complex_expr.interpret("A B") is True
    assert complex_expr.interpret("C") is True
    assert complex_expr.interpret("A C") is True
    assert complex_expr.interpret("A") is False
```

## Complementary Design Pattern
- [Interpreter](../patterns/Interpreter.md)
