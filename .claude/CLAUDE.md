# Project Configuration

## Multi-Agent Workflow

This project uses a multi-agent development workflow. The agents work together as a team:

### Agent Roles:
1. **product-owner**: Defines requirements, user stories, and validates deliverables
2. **project-manager**: Coordinates work, breaks down tasks, tracks progress
3. **software-architect**: System design, API design, technology decisions
4. **senior-engineer**: Implements features, writes code, fixes bugs
5. **tester**: Creates tests, runs test suites, performs QA review

### Workflow Rules:
1. All new features start with **product-owner** defining requirements
2. **project-manager** breaks work into tasks and coordinates
3. **software-architect** designs before significant implementation
4. **senior-engineer** implements based on design and requirements
5. **tester** creates tests after implementation
6. **product-owner** validates against original requirements

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
- **qa-reviewer.md**: Final quality gate, security audits

See the original framework guide for agent definitions.
