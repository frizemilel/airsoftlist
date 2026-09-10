---
description: Run all unit and integration tests with coverage report.
agent: qa-engineer
---
# Unit Tests Command

Run unit and integration tests for Airsoftlist.ru.

## Usage
```bash
opencode test
```

## What it does
1. Runs Vitest for all unit and integration tests.
2. Generates coverage report.
3. Reports:
   - Total tests: passed/failed/skipped
   - Coverage percentage by file
   - Any failures with file and line numbers
4. Fails if coverage < 80% on new code.
5. Fails if any test fails.

## Output
```markdown
### Test Results
- Total: 142
- Passed: 142
- Failed: 0
- Skipped: 3
- Coverage: 85.2%

### Coverage by file
| File | Statements | Branches | Functions | Lines |
|------|-----------|----------|-----------|-------|
| src/utils/formatPrice.ts | 100% | 100% | 100% | 100% |
| ... | ... | ... | ... | ... |

### Failures
None
```

## When to use
Run before every commit, on every PR, and when debugging any function or component.