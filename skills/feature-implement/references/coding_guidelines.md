# Coding Guidelines

## Fail-Fast Design

Prefer explicit failures over silent fallbacks. Fail loudly and early when something goes wrong.

### Principles

- **Fail on invalid state**: Throw exceptions, return errors — never continue with corrupted data
- **Fail on missing assumptions**: Validate preconditions explicitly; do not assume they hold
- **Fail on configuration errors**: App should crash on startup if required config is missing
- **No silent defaults**: Do not use fallback values that mask bugs (e.g., defaulting to empty string, returning null instead of error)

### Examples

```python
# BAD: Silent fallback - masks bugs
def get_user(id):
    user = db.query(id)
    return user or {}  # Hides missing user

# GOOD: Fail-fast - explicit error
def get_user(id):
    user = db.query(id)
    if not user:
        raise UserNotFoundError(f"User {id} not found")
    return user
```

```python
# BAD: Silent fallback - hides configuration error
config = load_config()
api_key = config.get("api_key") or "DEFAULT_KEY"  # Uses wrong key silently

# GOOD: Fail-fast - crash early
config = load_config()
api_key = config.get("api_key")
if not api_key:
    raise ConfigError("Missing required 'api_key' in config")
```

---

## Debug Logging

Aggressively add logging throughout the implementation, especially when transition blocks lack clear observable outcomes.

### When to Log

**Always log these key moments:**

- Entry points to functions/handlers
- State transitions (before and after)
- Conditional branches (each branch taken)
- External calls (request/response)
- Error conditions and exceptions

**Especially important when:**

- No observable UI/API response
- Background processing or async operations
- State mutations without visible side effects
- Any "fire and forget" operations

### Log Quality Guidelines

Include context that helps pinpoint the exact location and flow:

- **Function/method name**: Identify where the log came from
- **Line number**: Use `__line__` or logging framework's built-in
- **Instance/class name**: For class methods, show instance identifier
- **Relevant state**: Key variable values (without dumping entire objects)

**Good logs (help debugging):**

```text
DEBUG [auth.py:42] AuthService.authenticate: Entering with username="alice"
DEBUG [auth.py:56] AuthService.authenticate: Password verified, generating token
DEBUG [auth.py:61] AuthService.authenticate: Token generated id="tok_abc123", expires=3600s
ERROR [auth.py:67] AuthService.authenticate: Authentication failed for "alice": invalid credentials
```

**Bad logs (useless for debugging):**

```text
INFO: Starting authentication
INFO: Doing work
INFO: Done
```

### Log Levels

- **DEBUG**: Key steps, variable values, state transitions
- **INFO**: Business milestones, major operations
- **WARN**: Recoverable issues
- **ERROR**: Failures that need attention

---

## Bounded Loops and Timeouts

All loops and waits must have explicit bounds to prevent infinite execution.

### Loops

Always use bounded iteration. Avoid `while True` without exit conditions.

```python
# BAD: Unbounded while - no exit guarantee
while result.pending():
    result = check_server()

# GOOD: Explicit bound with range
for attempt in range(MAX_ATTEMPTS):
    result = check_server()
    if not result.pending():
        break
else:
    raise TimeoutError(f"Server did not respond after {MAX_ATTEMPTS} attempts")
```

When iterating over collections that could be large:

```python
# GOOD: Explicit bound with range
MAX_RETRIES = 100
for attempt in range(MAX_RETRIES):
    result = attempt_operation()
    if result.success:
        break
else:
    raise TimeoutError(f"Operation failed after {MAX_RETRIES} attempts")
```

### Waits and Polling

Always set explicit timeouts for waits and polling operations.

```python
# BAD: Potentially infinite wait
while not result.ready():
    sleep(0.1)

# GOOD: Timeout-bounded wait
from tenacity import retry, stop_after_attempt, wait_fixed

@retry(stop=stop_after_attempt(30), wait=wait_fixed(1))
def wait_for_result():
    result = check_status()
    if not result.ready():
        raise RetryError("Not ready yet")
    return result
```

```python
# GOOD: Explicit timeout
import asyncio

async def wait_for_ready(timeout: float = 30.0):
    start = time.monotonic()
    while time.monotonic() - start < timeout:
        if await check_ready():
            return True
        await asyncio.sleep(0.5)
    raise TimeoutError(f"Operation not ready after {timeout}s")
```

### Key Principle

If you cannot write `for _ in range(N)` or `timeout=N`, the operation is not safe to run.
