# Example Output — Clean Code Refactor

## 1. Repository Map

```
myproject/
├── src/
│   ├── agents/
│   │   ├── __init__.py
│   │   ├── base.py          # Agent base class
│   │   ├── runner.py        # Agent execution logic
│   │   └── tools.py         # Tool definitions
│   ├── api/
│   │   ├── __init__.py
│   │   ├── routes.py        # FastAPI routes
│   │   └── deps.py          # Dependency injection
│   ├── models/
│   │   ├── __init__.py
│   │   └── schemas.py       # Pydantic models
│   └── config.py            # Configuration
├── tests/
│   ├── test_agents.py
│   └── test_api.py
├── pyproject.toml
└── .env.example
```

## 2. Conventions Detected

| Convention | Status | Config |
|------------|--------|--------|
| Formatting | ✅ ruff | pyproject.toml |
| Type hints | ⚠️ Partial | — |
| Linting | ✅ ruff | pyproject.toml |
| Tests | ⚠️ pytest | Missing coverage |

## 3. Findings Table

| ID | Severity | Area | File(s) | Problem | Evidence | Impact | Effort | Risk |
|----|----------|------|---------|---------|----------|--------|--------|------|
| F01 | HIGH | agents/ | runner.py | God class | 450 lines | High | 2h | Medium |
| F02 | MEDIUM | config.py | — | Hardcoded URL | Line 23 | Medium | 15min | Low |
| F03 | LOW | api/routes.py | — | Missing type hints | Lines 45-67 | Low | 30min | Low |

## 4. Refactor Plan

### Phase 1 — Safe/Mechanical

**Task 1.1: Extract constants**
- File: `config.py`
- Before:
  ```python
  API_URL = "https://api.example.com"
  ```
- After:
  ```python
  from pydantic_settings import BaseSettings

  class Settings(BaseSettings):
      api_url: str = "https://api.example.com"

      class Config:
          env_prefix = ""
  ```

**Task 1.2: Add type hints to routes**
- File: `api/routes.py`
- Add return types to all endpoints

### Phase 2 — Moderate Risk

**Task 2.1: Split runner.py**
- Extract `AgentExecutor` to separate file
- Extract `ToolRegistry` to separate file
- Update imports

### Phase 3 — Higher Risk

(None identified — codebase is well-structured)

## 5. Proposed Folder Structure

**Before:**
```
src/agents/runner.py (450 lines)
```

**After:**
```
src/agents/
├── runner.py (150 lines - orchestration only)
├── executor.py (200 lines - execution logic)
└── registry.py (100 lines - tool management)
```

## 6. Top Diffs

```diff
--- a/src/config.py
+++ b/src/config.py
@@ -1,4 +1,5 @@
-API_URL = "https://api.example.com"
+from pydantic_settings import BaseSettings
+
+class Settings(BaseSettings):
+    api_url: str = "https://api.example.com"
+
+settings = Settings()
```

## 7. Test Plan

- Add unit tests for `Settings` class
- Add integration tests for `AgentExecutor`
- Target coverage: 80%

## 8. Risk & Rollback

- All changes are in a `cleancode` branch
- Can be reverted with `git checkout main`
- No breaking changes to public API

## 9. Definition of Done

- [ ] All Phase 1 tasks completed
- [ ] Linter passes with zero errors
- [ ] Test coverage >= 80%
- [ ] No breaking changes
- [ ] Documentation updated
- [ ] PR reviewed and approved
