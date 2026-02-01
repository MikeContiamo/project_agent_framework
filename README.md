# Multi-Agent Vibecoding Framework

A multi-agent development framework using Claude Code's native subagent capabilities for autonomous project development.

## Overview

This framework provides a team of specialized AI agents that work together to build projects with minimal human intervention:

| Agent | Role | Model |
|-------|------|-------|
| **Product Owner** | Defines requirements, user stories, and validates deliverables | opus |
| **Project Manager** | Coordinates work, breaks down tasks, tracks progress | sonnet |
| **Software Architect** | System design, API design, technology decisions | opus |
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

2. **Plan & Coordinate** (Project Manager):
   ```
   Use the project-manager agent to break down the requirements into tasks
   ```

3. **Design** (Software Architect):
   ```
   Use the software-architect agent to design the system architecture
   ```

4. **Implement** (Senior Engineer):
   ```
   Use the senior-engineer agent to implement [task description]
   ```

5. **Write Tests** (Tester):
   ```
   Use the tester agent to create tests for [feature]
   ```

### Quick Reference

| Action | Command |
|--------|---------|
| View agents | `/agents` |
| Define requirements | `Use the product-owner agent to...` |
| Coordinate tasks | `Use the project-manager agent to...` |
| Design architecture | `Use the software-architect agent to...` |
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

1. **Product Owner** defines clear requirements and user stories
2. **Project Manager** breaks down work into tasks and coordinates
3. **Software Architect** designs system architecture (for significant features)
4. **Senior Engineer** implements the solution
5. **Tester** creates comprehensive tests
6. **Product Owner** validates against original requirements

## Task Tracking

Tasks are tracked in `docs/tasks/` as markdown files. Each task includes:
- Status (TODO, IN_PROGRESS, IN_REVIEW, DONE, BLOCKED)
- Assigned agent
- Priority and dependencies
- Acceptance criteria

## Expanding the Team

Additional agents can be added when needed:
- **QA Reviewer**: Final quality gate and security audits

See `docs/guides/` for instructions on adding new agents.

## License

MIT
