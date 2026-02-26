# cleancode-refactor

> Senior software engineer refactoring agent for Claude Code

[![Claude Code Skill](https://img.shields.io/badge/Claude_Code-Skill-blue)](https://code.claude.com/docs/skills)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## Description

A Claude Code Agent Skill that analyzes your entire workspace, identifies code smells, and creates a structured Clean Code implementation plan following Robert C. Martin's principles.

## Installation

### Option 1: Project-level (recommended)

```bash
mkdir -p .claude/skills/cleancode-refactor
cd .claude/skills/cleancode-refactor
curl -O https://raw.githubusercontent.com/prof-ramos/cleancode-refactor/main/SKILL.md
mkdir examples && cd examples
curl -O https://raw.githubusercontent.com/prof-ramos/cleancode-refactor/main/examples/example-output.md
```

### Option 2: Global/personal

```bash
mkdir -p ~/.claude/skills/cleancode-refactor
cd ~/.claude/skills/cleancode-refactor
curl -O https://raw.githubusercontent.com/prof-ramos/cleancode-refactor/main/SKILL.md
mkdir examples && cd examples
curl -O https://raw.githubusercontent.com/prof-ramos/cleancode-refactor/main/examples/example-output.md
```

## Usage

### Direct invocation with arguments

```
/cleancode-refactor "Python 3.12 + FastAPI" "agents/, api/, models/" "main.py, agent.py"
```

### Without arguments (Claude infers the stack)

```
/cleancode-refactor
```

### Natural language

```
"refactor this codebase following Clean Code principles"
```

## Features

### 5-Step Workflow

1. **Inventory** — Repository map, configs, conventions
2. **Findings** — Severity-ranked issues table
3. **Refactor Plan** — 3 phases (Safe → Moderate → Higher risk)
4. **Git Branch** — Creates `cleancode` branch
5. **Apply Patches** — Batched diffs with approval gates

### What It Detects

| Category | Checks |
|----------|--------|
| **Architecture** | God modules, circular dependencies, layering issues |
| **Modularity** | DRY violations, low cohesion, poor structure |
| **Code Quality** | Long functions, deep nesting, dead code, magic strings |
| **Type Safety** | Missing hints, `Any` leakage, unsafe casts |
| **Error Handling** | Swallowed exceptions, print() vs logging |
| **Config/Env** | Hardcoded secrets, scattered env access |
| **Testing** | Brittle tests, missing coverage |
| **Performance** | N+1 patterns, blocking I/O in async |
| **Security** | Input validation gaps, unsafe eval/exec |

### Guardrails

- ✅ Maintains all public APIs
- ✅ No breaking changes without deprecation plan
- ✅ Incremental, reviewable diffs
- ✅ Never reduces test coverage
- ⚠️ Flags uncertain changes for human review

## Example Output

See [examples/example-output.md](examples/example-output.md) for a complete reference.

## Configuration

The skill respects existing project configs:

- `pyproject.toml` / `.ruff.toml` / `.mypy.ini`
- `.pre-commit-config.yaml`
- `Dockerfile`
- CI/CD workflows

## Conventions (if not defined)

| Area | Recommendation |
|------|----------------|
| Linting/Formatting | ruff (replaces flake8 + isort + black) |
| Type checking | mypy --strict or pyright |
| Env management | pydantic-settings |
| Error handling | Custom exception hierarchy |
| Logging | structlog or loguru |
| Package manager | uv |

## Skill Metadata

```yaml
name: cleancode-refactor
context: fork        # Runs in isolated subagent
agent: Plan          # Optimized for structured planning
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash(git *)
  - Bash(find *)
  - Bash(cat *)
```

## License

MIT

## Author

Gabriel Ramos ([@prof-ramos](https://github.com/prof-ramos))
