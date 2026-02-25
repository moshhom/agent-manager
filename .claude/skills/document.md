# Document

Summarize what was done and update the project's documentation. Run after all tasks are `done`.

## Steps

1. **Read task files** — Check `.claude/<branch-name>/` for tasks with status `done` to understand what was planned and implemented.
2. **Review code changes** — Look at what was added, modified, or removed.
3. **Update documentation:**
   - `.claude/knowledge/` files if the architecture or key patterns changed.
   - `README.md` if new features, setup steps, or usage instructions were added.
   - Any other existing docs that reference changed functionality.
4. **Commit** documentation updates.

## Rules

- Only update existing documentation files. Do not create new documentation unless a new feature clearly needs it.
- Keep updates concise and factual — describe what changed and why, not implementation details.
- Follow the existing documentation style and format of each file.
- Do not update task files — those are managed by the workflow.
- Do not modify source code.
