---
name: backend-bugfix-with-unit-tests
description: Workflow command scaffold for backend-bugfix-with-unit-tests in sub2api.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /backend-bugfix-with-unit-tests

Use this workflow when working on **backend-bugfix-with-unit-tests** in `sub2api`.

## Goal

Fixes a backend bug and adds or updates corresponding unit/integration tests to prevent regressions.

## Common Files

- `backend/internal/service/*.go`
- `backend/internal/service/*_test.go`
- `backend/internal/repository/*.go`
- `backend/internal/repository/*_test.go`
- `backend/internal/handler/*.go`
- `backend/internal/handler/*_test.go`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Identify and fix the bug in backend service/repository/handler code.
- Update or add new unit/integration tests covering the bug scenario.
- Commit both code and test changes together.

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.