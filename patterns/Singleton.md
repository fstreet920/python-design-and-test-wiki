# Singleton Pattern

The **Singleton Pattern** is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

## Intent
Ensure a class has only one instance and provide a global point of access to it.

## Python 3.12+ Implementation
Using a thread-safe metaclass.

```python
from threading import Lock
from typing import Any

class SingletonMeta(type):
    """
    This is a thread-safe implementation of Singleton.
    """
    _instances: dict[type, Any] = {}
    _lock: Lock = Lock()

    def __call__(cls, *args: Any, **kwargs: Any) -> Any:
        # Now, imagine that the program has just been launched. Since there's no
        # singleton instance yet, multiple threads can simultaneously pass the
        # previous conditional and reach this point almost at the same time. The
        # first of them will acquire lock and will proceed further, while the
        # rest will wait here.
        with cls._lock:
            # The first thread to acquire the lock, reaches this conditional,
            # goes inside and creates the Singleton instance. Once it leaves the
            # lock block, a thread that might have been waiting for the lock
            # release may then enter this section. But since the Singleton field
            # is already initialized, the thread won't create a new object.
            if cls not in cls._instances:
                instance = super().__call__(*args, **kwargs)
                cls._instances[cls] = instance
        return cls._instances[cls]

class Singleton(metaclass=SingletonMeta):
    value: str | None = None

    def __init__(self, value: str) -> None:
        self.value = value

    def some_business_logic(self) -> None:
        """
        Finally, any singleton should define some business logic, which can be
        executed on its instance.
        """

if __name__ == "__main__":
    # The client code.
    s1 = Singleton("FOO")
    s2 = Singleton("BAR")

    if id(s1) == id(s2):
        print("Singleton works, both variables contain the same instance.")
    else:
        print("Singleton failed, variables contain different instances.")
```

## Complementary Testing Pattern
- [[Singleton_Harness]]
