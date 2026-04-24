# Dependency Injection (Pythonic)

**Dependency Injection (DI)** is a pattern where an object receives its dependencies from an external source rather than creating them itself. In Python, this is often implemented simply via constructor injection.

## Intent
Decouple components to improve testability, maintainability, and flexibility.

## Python 3.12+ Implementation
Using `Protocol` for interface definition and constructor injection.

```python
from typing import Protocol

class MessageService(Protocol):
    def send(self, message: str) -> None:
        ...

class EmailService:
    def send(self, message: str) -> None:
        print(f"Sending email: {message}")

class SMSService:
    def send(self, message: str) -> None:
        print(f"Sending SMS: {message}")

class NotificationManager:
    """The client that depends on an injected service."""
    def __init__(self, service: MessageService) -> None:
        self.service = service

    def notify(self, message: str) -> None:
        self.service.send(message)

# Usage
if __name__ == "__main__":
    email_mgr = NotificationManager(EmailService())
    email_mgr.notify("Hello via Email")
    
    sms_mgr = NotificationManager(SMSService())
    sms_mgr.notify("Hello via SMS")
```

## Complementary Testing Pattern
- [[Dependency_Injection_Harness]]
