# Interpreter Pattern

The **Interpreter Pattern** is a behavioral design pattern that defines a grammatical representation for a language and an interpreter to interpret the grammar.

## Intent
Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.

## Python 3.12+ Implementation
Using `Protocol` for the expression interface and recursion for evaluation.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Expression(Protocol):
    def interpret(self, context: str) -> bool:
        ...

class TerminalExpression:
    def __init__(self, data: str) -> None:
        self._data = data

    def interpret(self, context: str) -> bool:
        return self._data in context

class OrExpression:
    def __init__(self, expr1: Expression, expr2: Expression) -> None:
        self._expr1 = expr1
        self._expr2 = expr2

    def interpret(self, context: str) -> bool:
        return self._expr1.interpret(context) or self._expr2.interpret(context)

class AndExpression:
    def __init__(self, expr1: Expression, expr2: Expression) -> None:
        self._expr1 = expr1
        self._expr2 = expr2

    def interpret(self, context: str) -> bool:
        return self._expr1.interpret(context) and self._expr2.interpret(context)

if __name__ == "__main__":
    # Rule: Robert and John are male
    robert = TerminalExpression("Robert")
    john = TerminalExpression("John")
    is_male = OrExpression(robert, john)

    # Rule: Julie is a married women
    julie = TerminalExpression("Julie")
    married = TerminalExpression("Married")
    is_married_woman = AndExpression(julie, married)

    print(f"Is Robert male? {is_male.interpret('Robert')}")
    print(f"Is Julie a married woman? {is_married_woman.interpret('Married Julie')}")
```

## Complementary Testing Pattern
- [Interpreter_Harness](../testing/Interpreter_Harness.md)
