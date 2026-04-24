# Proxy Pattern

The **Proxy Pattern** is a structural design pattern that lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

## Intent
Provide a surrogate or placeholder for another object to control access to it.

## Python 3.12+ Implementation
Using `Protocol` to define the common interface for the Subject and the Proxy.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Subject(Protocol):
    """The Subject interface declares common operations for both RealSubject and the Proxy."""
    def request(self) -> None:
        ...

class RealSubject:
    """The RealSubject contains some core business logic."""
    def request(self) -> None:
        print("RealSubject: Handling request.", end="")

class Proxy:
    """The Proxy has an interface identical to the RealSubject."""
    def __init__(self, real_subject: RealSubject) -> None:
        self._real_subject = real_subject

    def request(self) -> None:
        """The most common applications of the Proxy pattern are lazy loading, caching, controlling the access, logging, etc."""
        if self.check_access():
            self._real_subject.request()
            self.log_access()

    def check_access(self) -> bool:
        print("Proxy: Checking access prior to firing a real request.", end="")
        return True

    def log_access(self) -> None:
        print("Proxy: Logging the time of request.", end="")

def client_code(subject: Subject) -> None:
    """The client code is supposed to work with all objects (both subjects and proxies) via the Subject interface."""
    subject.request()

if __name__ == "__main__":
    print("Client: Executing the client code with a real subject:")
    real_subject = RealSubject()
    client_code(real_subject)

    print("\n\nClient: Executing the same client code with a proxy:")
    proxy = Proxy(real_subject)
    client_code(proxy)
```

## Complementary Testing Pattern
- [Proxy_Harness](../testing/Proxy_Harness.md)
