# Core Conventions

Universal principles for ALL projects built with this framework.

## Philosophy

### Incremental Progress
- Small, focused changes over big-bang rewrites
- Each commit should be deployable
- Refactor in separate commits from feature changes

### Study Before Building
- Read existing code patterns before implementing
- Understand the "why" behind current architecture
- Ask questions when patterns seem inconsistent

### Clarity Over Cleverness
- Obvious code beats clever code
- If it needs a comment, consider rewriting
- Future you (and teammates) will thank you

### Simplicity
- Avoid premature abstractions
- Don't add features "just in case"
- Three similar lines > premature abstraction

## Code Quality

### Every Commit Must:
1. Lint/compile successfully
2. Pass all existing tests
3. Include tests for new functionality
4. Have a commit message explaining "why"

### Commit Messages
Follow conventional commits format:
```
<type>(<scope>): <short description>

<body explaining WHY this change was made>

Types: feat, fix, docs, style, refactor, test, chore
```

Examples:
- `feat(auth): add OAuth2 support for GitHub login`
- `fix(api): handle null response from payment provider`
- `refactor(db): extract query builder for reuse`

## Error Handling

### Principles
1. **Define domain-specific exceptions** - Don't use generic exceptions
2. **Handle at the right level** - Where you have context to respond appropriately
3. **Never silently swallow** - At minimum, log the error
4. **Include debugging context** - What operation? What inputs?

### Bad
```python
try:
    process_payment(order)
except Exception:
    pass  # Silent failure - never do this
```

### Good
```python
try:
    process_payment(order)
except PaymentDeclinedError as e:
    logger.warning(f"Payment declined for order {order.id}: {e.reason}")
    notify_customer_payment_failed(order)
except PaymentProviderError as e:
    logger.error(f"Payment provider error for order {order.id}", exc_info=True)
    raise OrderProcessingError(f"Unable to process payment: {e}") from e
```

## Documentation Style

### General Rules
- Tight prose, no fluff
- Explain WHY, not WHAT (code shows what)
- No emojis in documentation
- Update docs in same commit as code changes

### When to Comment
- Non-obvious business logic
- Workarounds for external system bugs
- Performance-critical sections with tradeoffs
- Complex algorithms (cite sources)

### When NOT to Comment
```python
# Bad: Comment restates the code
# Increment counter by one
counter += 1

# Bad: Comment explains obvious method
# Get the user's name
user.get_name()
```

## Security (CRITICAL)

### Never Hardcode Secrets
```python
# NEVER do this
API_KEY = "sk-1234567890abcdef"

# Do this instead
import os
API_KEY = os.environ["API_KEY"]

# Or with Pydantic
from pydantic import SecretStr
class Settings(BaseSettings):
    api_key: SecretStr
```

### Always Use Parameterized Queries
```python
# NEVER do this (SQL injection vulnerability)
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")

# Do this instead
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))
```

### Validate All User Input
- Never trust data from external sources
- Validate at system boundaries
- Use schema validation (Pydantic, Zod, etc.)

### Principle of Least Privilege
- Request minimum permissions needed
- Use read-only access when writes aren't needed
- Scope API keys to specific resources

## Code Organization

### File Structure
- One concept per file
- Group related functionality in directories
- Keep files under 300 lines when possible

### Naming
- Use descriptive, unambiguous names
- Avoid abbreviations (except common ones: `id`, `url`, `api`)
- Boolean variables: `is_`, `has_`, `can_`, `should_`
- Functions: verb phrases (`calculate_total`, `fetch_user`)

### Dependencies
- Minimize external dependencies
- Pin dependency versions
- Document why each dependency was added
- Prefer standard library when sufficient

## Review Checklist

Before submitting code for review:
- [ ] Does it compile/lint without warnings?
- [ ] Do all tests pass?
- [ ] Is there test coverage for new code?
- [ ] Are there any hardcoded secrets?
- [ ] Is error handling appropriate?
- [ ] Is the commit message clear?
- [ ] Would a new team member understand this code?
