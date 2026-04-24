# Given-When-Then (BDD)

**Given-When-Then** is a behavioral testing pattern that structures tests in a way that maps directly to business requirements and user stories.

## Strategy
1.  **Given**: Establish the initial state (Preconditions).
2.  **When**: Perform the action or event (Behavior).
3.  **Then**: Verify the expected outcome (Postconditions).

## Pytest Implementation
Using `pytest-bdd` style or standard Pytest structure.

```python
# Standard Pytest BDD-style structure
def test_user_authentication():
    # Given
    auth_service = AuthService()
    user = User(username="testuser", password="password")
    auth_service.register(user)

    # When
    result = auth_service.login("testuser", "password")

    # Then
    assert result is True
    assert auth_service.current_user == user
```

## Complementary Design Patterns
- [[Mediator]] - Useful for testing the interactions between multiple components managed by a mediator.
- [[State]] - Ideal for testing transitions between different states of an object.
- [[Chain_of_Responsibility]] - Structures the test to verify a request's journey through the chain.
