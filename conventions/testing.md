# Testing Conventions

Testing standards for all projects.

## Philosophy

### Test Behavior, Not Implementation
```python
# Bad: Tests implementation details
def test_user_service_calls_repository_save():
    mock_repo = Mock()
    service = UserService(mock_repo)
    service.create_user(name="Alice")
    mock_repo.save.assert_called_once()  # Brittle to refactoring

# Good: Tests behavior
def test_created_user_can_be_retrieved():
    service = UserService(InMemoryUserRepository())
    user = service.create_user(name="Alice")
    retrieved = service.get_user(user.id)
    assert retrieved.name == "Alice"
```

### Start Simple
- Write the simplest test that could fail
- Add complexity only when needed
- One assertion per test when possible

### Prefer Real Dependencies
```python
# Don't mock everything
# Bad: Mocking things you own
def test_with_mocks():
    mock_db = Mock()
    mock_db.query.return_value = [{"id": 1, "name": "Alice"}]
    service = UserService(mock_db)
    # ...

# Good: Use real implementations for things you control
def test_with_real_db():
    db = create_test_database()
    service = UserService(db)
    service.create_user(name="Alice")
    users = service.list_users()
    assert len(users) == 1
```

### Use Dependency Injection
```python
# Design for testability
class PaymentService:
    def __init__(self, payment_gateway: PaymentGateway):
        self._gateway = payment_gateway

# In production
service = PaymentService(StripeGateway())

# In tests
service = PaymentService(FakePaymentGateway())
```

## Test Organization

### Co-locate Tests with Code
```
src/
├── users/
│   ├── service.py
│   ├── service_test.py    # Tests next to implementation
│   ├── repository.py
│   └── repository_test.py
```

Or use a separate tests directory mirroring src:
```
src/
├── users/
│   ├── service.py
│   └── repository.py
tests/
├── users/
│   ├── test_service.py
│   └── test_repository.py
```

### Naming Convention
```python
# Python: test_<method>_<scenario>_<expected>
def test_create_user_with_valid_email_returns_user():
    ...

def test_create_user_with_invalid_email_raises_validation_error():
    ...
```

```typescript
// TypeScript: describe/it pattern
describe('UserService', () => {
  describe('createUser', () => {
    it('returns user when email is valid', () => { ... });
    it('throws ValidationError when email is invalid', () => { ... });
  });
});
```

## Test Doubles

### Types of Test Doubles
- **Fake**: Working implementation (e.g., in-memory database)
- **Stub**: Returns canned responses
- **Spy**: Records calls for verification
- **Mock**: Pre-programmed expectations (use sparingly)

### Prefer Fakes Over Mocks
```python
# Define a protocol/interface
from typing import Protocol

class UserRepository(Protocol):
    def save(self, user: User) -> User: ...
    def find_by_id(self, id: UUID) -> User | None: ...

# Create a fake for testing
class InMemoryUserRepository:
    def __init__(self):
        self._users: dict[UUID, User] = {}

    def save(self, user: User) -> User:
        self._users[user.id] = user
        return user

    def find_by_id(self, id: UUID) -> User | None:
        return self._users.get(id)
```

### When to Mock
- External services (payment gateways, email providers)
- Time-sensitive operations (use frozen time)
- Non-deterministic operations (random values)

### Never Mock Internal Code
```python
# Bad: Mocking your own code
@patch('myapp.services.user_service.UserRepository')
def test_something(mock_repo):
    ...  # Brittle, breaks on refactoring

# Good: Use dependency injection with fakes
def test_something():
    repo = InMemoryUserRepository()
    service = UserService(repo)
    ...
```

## Test Structure

### Arrange-Act-Assert
```python
def test_user_can_be_created():
    # Arrange
    repo = InMemoryUserRepository()
    service = UserService(repo)

    # Act
    user = service.create_user(name="Alice", email="alice@example.com")

    # Assert
    assert user.name == "Alice"
    assert user.email == "alice@example.com"
```

### Given-When-Then (BDD style)
```python
def test_user_can_be_created():
    # Given a user service
    service = UserService(InMemoryUserRepository())

    # When creating a user
    user = service.create_user(name="Alice", email="alice@example.com")

    # Then the user is created with correct details
    assert user.name == "Alice"
```

## Test Categories

### Unit Tests
- Test single units in isolation
- Fast (< 10ms per test)
- No I/O, no network, no database
- Use fakes for dependencies

### Integration Tests
- Test components working together
- May use real databases (test containers)
- Test API endpoints end-to-end
- Slower, run less frequently

### End-to-End Tests
- Test complete user workflows
- Use real browser (Playwright, Cypress)
- Slowest, most valuable for critical paths
- Keep to minimum (test pyramid)

## Fixtures

### Python (pytest)
```python
# conftest.py
import pytest

@pytest.fixture
def user_repository():
    return InMemoryUserRepository()

@pytest.fixture
def user_service(user_repository):
    return UserService(user_repository)

@pytest.fixture
def sample_user(user_service):
    return user_service.create_user(name="Test User", email="test@example.com")
```

### TypeScript (Vitest)
```typescript
// setup.ts
import { beforeEach } from 'vitest';

let userRepository: InMemoryUserRepository;
let userService: UserService;

beforeEach(() => {
  userRepository = new InMemoryUserRepository();
  userService = new UserService(userRepository);
});
```

## Assertions

### Be Specific
```python
# Bad: Vague assertion
assert result is not None

# Good: Specific assertion
assert result.status == "completed"
assert result.items_processed == 5
```

### Use Matcher Libraries
```python
# Python: pytest assertions with good messages
assert user.name == "Alice", f"Expected name 'Alice', got '{user.name}'"

# Or use pytest-check for multiple assertions
from pytest_check import check
with check:
    assert user.name == "Alice"
    assert user.email == "alice@example.com"
```

## Running Tests

### Python
```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=src --cov-report=html

# Run specific test file
pytest tests/test_users.py

# Run specific test
pytest tests/test_users.py::test_create_user

# Run tests matching pattern
pytest -k "create_user"
```

### TypeScript
```bash
# Run all tests
npm test

# Run with coverage
npm test -- --coverage

# Run specific file
npm test -- src/users/service.test.ts

# Watch mode
npm test -- --watch
```

## Checklist

Before merging:
- [ ] All tests pass
- [ ] New code has tests
- [ ] Tests are deterministic (no flakiness)
- [ ] Tests are independent (no shared state)
- [ ] Test names clearly describe the scenario
- [ ] No mocking of internal code
- [ ] Integration tests for critical paths
