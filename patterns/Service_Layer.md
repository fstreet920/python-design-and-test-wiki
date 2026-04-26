# Service Layer Pattern

The **Service Layer Pattern** defines an application's boundary with a layer of services that establishes a set of available operations and coordinates the application's response in each operation.

## Intent
Decouple the application logic from the presentation layer (e.g., API controllers, CLI) and the data access layer. It orchestrates the domain objects and the Unit of Work to perform high-level business tasks.

## Python 3.12+ Implementation
Using Dependency Injection of the Unit of Work.

```python
from patterns.Unit_of_Work import UnitOfWork
from patterns.Repository import User

class UserService:
    """The Service Layer orchestrates high-level operations."""
    def __init__(self, uow: UnitOfWork) -> None:
        self.uow = uow

    def create_user(self, user_id: int, username: str, email: str) -> None:
        """A service operation that coordinates UoW and Repositories."""
        with self.uow:
            user = User(id=user_id, username=username, email=email)
            self.uow.users.add(user)
            self.uow.commit()

    def get_user_details(self, user_id: int) -> str:
        """A read-only operation."""
        with self.uow:
            user = self.uow.users.get(user_id)
            if not user:
                return "User not found"
            return f"User: {user.username} ({user.email})"

# Usage
if __name__ == "__main__":
    from patterns.Unit_of_Work import MemoryUnitOfWork
    uow = MemoryUnitOfWork()
    service = UserService(uow)
    
    service.create_user(1, "alice", "alice@example.com")
    print(service.get_user_details(1))
```

## Complementary Testing Pattern
- [Service_Layer_Harness](../testing/Service_Layer_Harness.md)
