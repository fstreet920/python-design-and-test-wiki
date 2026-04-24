# Proxy Harness

The **Proxy Harness** provides a standardized way to test the Proxy Pattern, focusing on access control, logging, and correct delegation to the real subject.

## Strategy
1.  **Interface Consistency:** Verify that both the `Proxy` and `RealSubject` implement the `Subject` protocol.
2.  **Access Control:** Verify that the proxy correctly allows or denies access to the real subject based on its internal logic.
3.  **Side Effects Verification:** Verify that the proxy performs expected side effects (like logging or caching) before or after delegating to the real subject.
4.  **Mock Real Subject:** Use a mock `RealSubject` to verify that the proxy delegates calls correctly and doesn't bypass its own logic.

## Pytest Implementation

```python
import pytest
from unittest.mock import MagicMock
from patterns.Proxy import Subject, RealSubject, Proxy

@pytest.fixture
def mock_real_subject() -> MagicMock:
    return MagicMock(spec=RealSubject)

def test_proxy_delegates_to_real_subject(mock_real_subject: MagicMock) -> None:
    """Verify that the proxy calls the real subject's request method."""
    proxy = Proxy(mock_real_subject)
    proxy.request()
    
    mock_real_subject.request.assert_called_once()

def test_proxy_access_control(mock_real_subject: MagicMock, capsys: pytest.CaptureFixture[str]) -> None:
    """Verify that the proxy checks access before delegating."""
    proxy = Proxy(mock_real_subject)
    proxy.request()
    
    captured = capsys.readouterr()
    assert "Proxy: Checking access" in captured.out
    assert "RealSubject: Handling request" not in captured.out # Because RealSubject is mocked
    mock_real_subject.request.assert_called_once()

def test_proxy_logging(mock_real_subject: MagicMock, capsys: pytest.CaptureFixture[str]) -> None:
    """Verify that the proxy logs access after delegating."""
    proxy = Proxy(mock_real_subject)
    proxy.request()
    
    captured = capsys.readouterr()
    assert "Proxy: Logging the time of request" in captured.out

def test_proxy_protocol_compliance() -> None:
    """Verify that Proxy follows the Subject protocol."""
    real_subject = RealSubject()
    proxy = Proxy(real_subject)
    assert isinstance(proxy, Subject)
    assert isinstance(real_subject, Subject)
```

## Complementary Design Pattern
- [[Proxy]]
