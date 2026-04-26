# Unit of Work Harness

The **Unit of Work Harness** ensures that the transactional boundaries are correctly managed and that commits and rollbacks occur as expected.

## Strategy
1.  **Context Manager Compliance:** Verify that the UoW can be used in a `with` statement.
2.  **Commit Verification:** Ensure that `commit()` correctly persists changes.
3.  **Rollback on Error:** Verify that if an exception occurs within the `with` block, `rollback()` is called (or at least `commit()` is NOT called).
4.  **Atomicity:** Verify that multiple operations within the same UoW are treated as a single unit.

## Pytest Implementation

```python
import pytest
from patterns.Unit_of_Work import MemoryUnitOfWork, UnitOfWork
from patterns.Repository import User

@pytest.fixture
def uow() -> UnitOfWork:
    return MemoryUnitOfWork()

def test_uow_commits_successfully(uow: UnitOfWork) -> None:
    """Verify that explicit commit works."""
    with uow:
        uow.users.add(User(id=1, username="alice", email="a@b.com"))
        uow.commit()
    
    assert uow.committed is True
    assert uow.users.get(1) is not None

def test_uow_rolls_back_on_exception(uow: UnitOfWork) -> None:
    """Verify that an exception causes a rollback (or prevents commit)."""
    class MyException(Exception):
        pass

    try:
        with uow:
            uow.users.add(User(id=1, username="alice", email="a@b.com"))
            raise MyException("Something went wrong")
            uow.commit()
    except MyException:
        pass
    
    assert uow.committed is False

def test_uow_protocol_compliance() -> None:
    """Verify that MemoryUnitOfWork satisfies the UnitOfWork protocol."""
    assert isinstance(MemoryUnitOfWork(), UnitOfWork)
```

## Complementary Design Pattern
- [Unit_of_Work](../patterns/Unit_of_Work.md)
