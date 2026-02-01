# Python Conventions

Python-specific conventions for projects using Python 3.10+.

## Type System

### Modern Type Hints (Python 3.10+)
```python
# Use lowercase generic types
def process_items(items: list[str]) -> dict[str, int]:
    ...

# Use X | None instead of Optional[X]
def find_user(id: int) -> User | None:
    ...

# Use X | Y for unions
def parse_value(value: str | int) -> float:
    ...
```

### Avoid These
```python
# Don't use typing module for basic types
from typing import List, Dict, Optional  # Avoid

# Don't use Any unless absolutely necessary
def process(data: Any):  # Avoid

# Don't use type: ignore without explanation
result = some_call()  # type: ignore  # Why?
```

### When `Any` is Acceptable
```python
# With explanation
def serialize(obj: Any) -> str:  # Any: accepts arbitrary JSON-serializable types
    return json.dumps(obj)
```

## Imports

### Order
```python
# 1. Standard library
import os
from pathlib import Path

# 2. Third-party packages
import httpx
from pydantic import BaseModel

# 3. Local imports
from myapp.models import User
from myapp.services.auth import authenticate
```

### Rules
- All imports at module level (never inside functions)
- No wildcard imports (`from x import *`)
- Use absolute imports
- One import per line for clarity

## Models

### Use Pydantic v2
```python
from datetime import datetime
from uuid import UUID
from pydantic import BaseModel, Field

class User(BaseModel):
    id: UUID
    external_id: str  # External IDs as strings
    email: str
    name: str
    created_at: datetime = Field(default_factory=datetime.utcnow)
    updated_at: datetime = Field(default_factory=datetime.utcnow)

    model_config = {"strict": True}
```

### ID Conventions
- Internal IDs: UUID
- External IDs (from APIs): string
- Always include `created_at` and `updated_at`

### Sensitive Data
```python
from pydantic import BaseModel, SecretStr

class DatabaseConfig(BaseModel):
    host: str
    port: int
    password: SecretStr  # Won't be logged/serialized
```

## Database

### Never Use F-Strings for SQL
```python
# NEVER
query = f"SELECT * FROM users WHERE email = '{email}'"

# ALWAYS use parameterized queries
query = "SELECT * FROM users WHERE email = %s"
cursor.execute(query, (email,))
```

### Repository Pattern
```python
from abc import ABC, abstractmethod

class UserRepository(ABC):
    @abstractmethod
    async def get_by_id(self, user_id: UUID) -> User | None:
        ...

    @abstractmethod
    async def save(self, user: User) -> User:
        ...

class PostgresUserRepository(UserRepository):
    def __init__(self, connection_pool):
        self._pool = connection_pool

    async def get_by_id(self, user_id: UUID) -> User | None:
        async with self._pool.acquire() as conn:
            row = await conn.fetchrow(
                "SELECT * FROM users WHERE id = $1",
                user_id
            )
            return User(**row) if row else None
```

## Async

### Use `async`/`await` Consistently
```python
# If a function is async, callers should be async too
async def get_user(user_id: int) -> User:
    return await user_repository.get_by_id(user_id)

# Don't mix sync and async in the same layer
```

### HTTP Clients
```python
import httpx

# Prefer httpx over requests for async support
async def fetch_data(url: str) -> dict:
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        response.raise_for_status()
        return response.json()
```

## Error Handling

### Define Domain Exceptions
```python
class DomainError(Exception):
    """Base exception for domain errors."""
    pass

class UserNotFoundError(DomainError):
    def __init__(self, user_id: UUID):
        self.user_id = user_id
        super().__init__(f"User not found: {user_id}")

class ValidationError(DomainError):
    def __init__(self, field: str, message: str):
        self.field = field
        self.message = message
        super().__init__(f"Validation error on {field}: {message}")
```

## Project Structure

```
project/
├── src/
│   └── myapp/
│       ├── __init__.py
│       ├── main.py           # Entry point
│       ├── config.py         # Configuration
│       ├── models/           # Pydantic models
│       ├── repositories/     # Data access
│       ├── services/         # Business logic
│       └── api/              # API endpoints
├── tests/
│   ├── conftest.py          # Shared fixtures
│   ├── unit/
│   └── integration/
├── pyproject.toml
└── README.md
```

## Tools

### Required in `pyproject.toml`
```toml
[tool.ruff]
line-length = 100
select = ["E", "F", "I", "UP"]

[tool.mypy]
strict = true
python_version = "3.10"

[tool.pytest.ini_options]
testpaths = ["tests"]
asyncio_mode = "auto"
```

### Running Checks
```bash
# Format
ruff format .

# Lint
ruff check .

# Type check
mypy src/

# Test
pytest
```
