# Object Mother / Test Data Builder

These patterns provide a centralized and flexible way to create complex test objects, preventing "mystery guest" and "fragile test" anti-patterns.

## Strategy
- **Object Mother**: A module of static methods that return "named" states (e.g., `create_admin_user()`).
- **Test Data Builder**: A fluent or factory-based approach to build objects step-by-step with sensible defaults (e.g., `UserFactory(email='test@example.com')`).

## Pytest Implementation
Using `factory_boy` for the Builder pattern.

```python
import factory
from dataclasses import dataclass

@dataclass
class User:
    username: str
    email: str
    is_admin: bool

class UserFactory(factory.Factory):
    class Meta:
        model = User

    username = factory.Faker("user_name")
    email = factory.Faker("email")
    is_admin = False

# Hybrid: Mother fixture using a Builder
@pytest.fixture
def admin_user() -> User:
    return UserFactory(is_admin=True, username="admin_boss")

def test_admin_access(admin_user: User) -> None:
    assert admin_user.is_admin is True
    assert admin_user.username == "admin_boss"
```

## Complementary Design Patterns
- [Builder](../patterns/Builder.md) - Directly maps to the Test Data Builder pattern.
- [Prototype](../patterns/Prototype.md) - Object Mothers often act as a registry of prototypes for tests.
- [Factory_Method](../patterns/Factory_Method.md) - The underlying mechanism for generating test data objects.
