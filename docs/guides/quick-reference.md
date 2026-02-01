# Quick Reference

## Agent Commands

| Action | Command |
|--------|---------|
| View all agents | `/agents` |
| Use Product Owner | `Use the product-owner agent to...` |
| Use Senior Engineer | `Use the senior-engineer agent to...` |
| Use Tester | `Use the tester agent to...` |

## Common Tasks

### Requirements
```
Use the product-owner agent to define requirements for [feature]
Use the product-owner agent to create user stories for [feature]
Use the product-owner agent to validate [feature] against requirements
```

### Implementation
```
Use the senior-engineer agent to implement [feature]
Use the senior-engineer agent to fix the bug in [component]
Use the senior-engineer agent to refactor [module]
```

### Testing
```
Use the tester agent to create tests for [feature]
Use the tester agent to run the test suite
Use the tester agent to review [component] for quality issues
```

## Task States

| Status | Meaning |
|--------|---------|
| `TODO` | Ready to start |
| `IN_PROGRESS` | Currently being worked on |
| `IN_REVIEW` | Waiting for review |
| `DONE` | Completed |
| `BLOCKED` | Cannot proceed |

## Task Priorities

| Priority | Meaning |
|----------|---------|
| `P0` | Critical - do immediately |
| `P1` | High - do soon |
| `P2` | Normal - do when possible |

## File Locations

| Type | Location |
|------|----------|
| Tasks | `docs/tasks/TASK-XXX.md` |
| ADRs | `docs/architecture/ADR-XXX.md` |
| Requirements | `docs/requirements/` |
| Source code | `src/` |
| Tests | `tests/` |

## Templates

| Template | Location |
|----------|----------|
| Task | `templates/task-template.md` |
| ADR | `templates/adr-template.md` |
| User Story | `templates/user-story-template.md` |

## Git Workflow

```bash
# Create feature branch
git checkout -b feature/my-feature

# Commit with conventional message
git commit -m "feat(auth): add login functionality"

# Push and create PR
git push -u origin feature/my-feature
```

## Commit Types

| Type | Use For |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation |
| `style` | Formatting |
| `refactor` | Code restructuring |
| `test` | Adding tests |
| `chore` | Maintenance |
