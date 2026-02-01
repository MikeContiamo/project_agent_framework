# Multi-Agent Vibecoding Framework

A multi-agent development framework using Claude Code's native subagent capabilities for autonomous project development.

## Overview

This framework provides a team of specialized AI agents that work together to build projects with minimal human intervention:

| Agent | Role | Model |
|-------|------|-------|
| **Product Owner** | Defines requirements, user stories, and validates deliverables | opus |
| **Senior Engineer** | Implements features, fixes bugs, and writes code | sonnet |
| **Tester** | Creates tests, runs test suites, and ensures quality | sonnet |

## Getting Started

### Prerequisites

- [Claude Code CLI](https://claude.ai/code) installed
- Node.js 18+ (optional, for JavaScript/TypeScript projects)
- Python 3.10+ (optional, for Python projects)

### Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/project_agent_framework.git
   cd project_agent_framework
   ```

2. The agents are automatically available in Claude Code. Verify with:
   ```
   /agents
   ```

## Usage

### Starting a New Feature

1. **Define Requirements** (Product Owner):
   ```
   Use the product-owner agent to define requirements for [feature description]
   ```

2. **Implement** (Senior Engineer):
   ```
   Use the senior-engineer agent to implement [task description]
   ```

3. **Write Tests** (Tester):
   ```
   Use the tester agent to create tests for [feature]
   ```

### Quick Reference

| Action | Command |
|--------|---------|
| View agents | `/agents` |
| Define requirements | `Use the product-owner agent to...` |
| Implement feature | `Use the senior-engineer agent to...` |
| Create tests | `Use the tester agent to...` |

## Project Structure

```
project_agent_framework/
├── .claude/
│   ├── agents/           # Agent definitions
│   └── CLAUDE.md         # Project configuration
├── docs/
│   ├── tasks/            # Task tracking
│   ├── architecture/     # Architecture Decision Records
│   ├── requirements/     # Requirements documentation
│   └── guides/           # Usage guides
├── templates/            # Task and documentation templates
├── src/                  # Source code
├── tests/                # Test files
├── package.json          # Node.js configuration
└── pyproject.toml        # Python configuration
```

## Workflow

1. All features start with the **Product Owner** defining clear requirements
2. The **Senior Engineer** implements the solution
3. The **Tester** creates comprehensive tests
4. Iterate until all acceptance criteria are met

## Task Tracking

Tasks are tracked in `docs/tasks/` as markdown files. Each task includes:
- Status (TODO, IN_PROGRESS, IN_REVIEW, DONE, BLOCKED)
- Assigned agent
- Priority and dependencies
- Acceptance criteria

## Expanding the Team

Additional agents can be added when needed:
- **Project Manager**: Coordinates work and tracks progress
- **Software Architect**: System design and technology decisions
- **QA Reviewer**: Final quality gate and security audits

See `docs/guides/` for instructions on adding new agents.

## License

MIT
