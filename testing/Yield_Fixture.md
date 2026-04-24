# Yield Fixture (RAII)

The **Yield Fixture** pattern (Resource Acquisition Is Initialization) is the standard Pytest pattern for managing resource lifecycles, ensuring that setup and teardown are handled reliably.

## Strategy
1.  **Setup phase**: All code before the `yield` statement is executed before the test.
2.  **Execution phase**: The `yield` statement provides the resource to the test.
3.  **Teardown phase**: All code after the `yield` statement is executed after the test, even if the test fails.

## Pytest Implementation

```python
import pytest
import os
from typing import Generator

@pytest.fixture
def temp_config_file() -> Generator[str, None, None]:
    """Setup and teardown of a temporary configuration file."""
    # Setup
    file_path = "test_config.json"
    with open(file_path, "w") as f:
        f.write('{"api_key": "test_key"}')
    
    yield file_path  # Provide the resource
    
    # Teardown
    if os.path.exists(file_path):
        os.remove(file_path)

def test_config_loading(temp_config_file: str) -> None:
    assert os.path.exists(temp_config_file)
    with open(temp_config_file, "r") as f:
        assert "api_key" in f.read()
```

## Complementary Design Patterns
- [Singleton](../patterns/Singleton.md) - Used to ensure state is reset for singletons between tests.
- [Proxy](../patterns/Proxy.md) - Useful for testing proxies that manage heavy resource connections.
