# Review

Verify that code follows the plan and complies with all rules and standards. Pick tasks with status `test-passed`.

## Steps

1. **Set status to `reviewing`** in the task file.
2. **Read the rules** — Load `.claude/rules/coding-standards.md` and `.claude/rules/workflow-rules.md`.
3. **Read project knowledge** — Check `.claude/knowledge/` for project-specific patterns and conventions.
4. **Review code changes against:**
   - **The plan**: Does the code do what was planned? Are all tasks addressed? Any leftover `[PLAN]` comments?
   - **Coding standards**: Header comments, function one-liners, thought-chain comments, function size, error handling, null safety.
   - **Project conventions**: Does it follow existing patterns from `.claude/knowledge/`?
5. **Update status based on review:**
   - Compliant → set status to `done`.
   - Blocking issues found → set status to `test-failed` with specific issues (file, line, what's wrong) in the task file.
   - Fundamentally wrong approach → set status to `replan` with an explanation.

## Blocking vs non-blocking

- Only blocking issues send a task back. Suggestions can be noted but do not block.
- Missing function comments, unhandled errors, null safety violations, leftover `[PLAN]` comments are **blocking**.

## Rules

- **Do not modify code.** Only review and report.
- Be specific — reference exact files and lines when reporting issues.
- Distinguish between blocking issues (must fix) and suggestions (nice to have).
