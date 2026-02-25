# Test

Write and run tests to verify that implemented code works correctly. Pick tasks with status `implemented`.

## Steps

1. **Set status to `testing`** in the task file.
2. **Find existing tests** — Look at the project's test patterns, framework, naming conventions, and structure before writing new tests.
3. **Write tests** — Create or update test files covering the acceptance criteria from the task file.
4. **Run the test suite** — Execute tests and capture results.
5. **Update status based on results:**
   - All tests pass → set status to `test-passed`.
   - Any test fails → set status to `test-failed` and write failure details into the task file (what failed, expected vs actual, likely cause).
6. **Commit** test files and task file status update.

## Rules

- **Never modify source code.** Only create and edit test files.
- Follow the project's existing test conventions — check existing tests before writing new ones.
- Follow `.claude/rules/coding-standards.md` for comments and code quality in test files.
- Test the acceptance criteria from the task files, not just happy paths.
