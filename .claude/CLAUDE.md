# Project Configuration

## Multi-Agent Workflow

This project uses a multi-agent development workflow. The agents work together as a team:

### Agent Roles (Core Team):
1. **product-owner**: Defines requirements, user stories, and validates deliverables
2. **senior-engineer**: Implements features, writes code, fixes bugs
3. **tester**: Creates tests, runs test suites, performs QA review

### Workflow Rules:
1. All new features start with product-owner defining requirements
2. senior-engineer implements based on clear requirements
3. tester creates tests after implementation
4. product-owner validates against original requirements

### Task States:
- **TODO**: Ready to be worked on
- **IN_PROGRESS**: Currently being implemented
- **IN_REVIEW**: Awaiting review
- **DONE**: Completed and approved
- **BLOCKED**: Cannot proceed (document blocker)

### Communication Protocol:
- Agents communicate through task files in `docs/tasks/`
- Status updates go in task files
- Blockers are escalated to the user immediately
- All decisions are documented

## Multi-Language Support

This project supports both Node.js/TypeScript and Python:

### Node.js/TypeScript:
- Configuration: `package.json`
- Run tests: `npm test`
- Node 18+ required

### Python:
- Configuration: `pyproject.toml`
- Run tests: `pytest`
- Python 3.10+ required

## File Organization

```
docs/
├── tasks/          # Task tracking files (TASK-XXX.md)
├── architecture/   # Architecture Decision Records (ADR-XXX.md)
├── requirements/   # Requirements documents
└── guides/         # Usage documentation

templates/          # File templates for consistency
src/                # Source code
tests/              # Test files
```

## Conventions

### Commit Messages:
Follow conventional commits:
```
<type>(<scope>): <description>

Types: feat, fix, docs, style, refactor, test, chore
```

### Task Files:
Use the template in `templates/task-template.md`

### Branch Naming:
- `main` - stable releases
- `feature/*` - feature branches
- `fix/*` - bug fix branches

## Expanding the Team

Additional agents can be added to `.claude/agents/`:
- **project-manager.md**: Coordinates work, tracks progress
- **software-architect.md**: System design, technology decisions
- **qa-reviewer.md**: Final quality gate, security audits

See the original framework guide for agent definitions.
