# Usage Guide

## Getting Started

### View Available Agents

To see all available agents:
```
/agents
```

### Invoke an Agent

To use a specific agent:
```
Use the [agent-name] agent to [task description]
```

## Workflow Examples

### Example 1: Building a New Feature

**Step 1: Define Requirements (Product Owner)**
```
Use the product-owner agent to define requirements for a user authentication system
```

The Product Owner will:
- Ask clarifying questions
- Create a PROJECT_BRIEF.md in docs/requirements/
- Define user stories with acceptance criteria

**Step 2: Implement (Senior Engineer)**
```
Use the senior-engineer agent to implement the login functionality based on the requirements in docs/requirements/
```

The Senior Engineer will:
- Read the requirements
- Implement the feature
- Document the implementation

**Step 3: Test (Tester)**
```
Use the tester agent to create tests for the authentication system
```

The Tester will:
- Create unit tests
- Create integration tests
- Report test coverage

### Example 2: Fixing a Bug

```
Use the senior-engineer agent to fix the bug where users can't logout
```

The Senior Engineer will:
- Investigate the issue
- Implement a fix
- Verify the fix works

### Example 3: Validating a Feature

```
Use the product-owner agent to validate the authentication feature against the original requirements
```

The Product Owner will:
- Compare implementation against requirements
- Create a validation report
- Identify any gaps

## Task Management

### Creating a Task

Tasks are stored in `docs/tasks/` using the template in `templates/task-template.md`.

Example task file: `docs/tasks/TASK-001.md`

### Task Status Flow

```
TODO → IN_PROGRESS → IN_REVIEW → DONE
                 ↓
              BLOCKED
```

### Checking Task Status

Read the task files in `docs/tasks/` to see current status.

## Best Practices

### 1. Clear Requirements First
Always start with the Product Owner defining clear requirements before implementation.

### 2. Small, Focused Tasks
Break large features into smaller tasks that can be completed in a few hours.

### 3. Document Everything
All decisions, implementations, and reviews should be documented in task files.

### 4. Use Templates
Use the templates in `templates/` for consistency.

### 5. Validate at the End
Use the Product Owner to validate that the implementation meets the original requirements.

## Multi-Language Development

### Node.js/TypeScript
```bash
npm install        # Install dependencies
npm test           # Run tests
npm run lint       # Run linter
```

### Python
```bash
pip install -e ".[dev]"  # Install with dev dependencies
pytest                    # Run tests
pytest --cov=src         # Run tests with coverage
ruff check .             # Run linter
```
