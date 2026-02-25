# Workflow Rules

These rules apply to every task Claude works on in any repository using agent-coding-manager.

---

## 1. Read Rules 

- Read all files in `.claude/rules/` and `.claude/knowledge/`.

## 2. Respect Project Configuration

- If the project has a root `CLAUDE.md`, read and follow it first.
- Project-level instructions take priority over these general rules.

## 3. Branch Discipline

- Never work directly on team branches (`main`, `develop`, feature branches owned by humans).
- Always create and work on `claude/` prefixed branches, push only to your own `claude/` branches unless explicitly told otherwise.

## 4. Know the Codebase

- Always search the codebase for existing functions, utilities, and patterns before writing new code.
- Do not reinvent what already exists. Reuse existing helpers, services, and abstractions.

## 5. Planning

Plan before coding.

### Research

- Read `.claude/knowledge/` for project architecture and patterns.
- Search the codebase for existing code relevant to the task — key files, utilities, patterns, dependencies.

### Create task files

Every new task should have its task file, if task is big then create many task files as described below.

Task files live in `.claude/<branch-name>/` inside the **target project repo** (not in a workspace root `.claude/`). Create the folder if it doesn't exist. Replace `/` with `-` in the branch name.

```
<target-project>/.claude/<branch-name>/
  task_context.md    ← codebase research: key files, patterns, decisions, gotchas
  task_1.md          ← one file per task/subtask
  task_2.md
```

Each task file should contain:
- **Description** — what needs to be done.
- **Acceptance criteria** — how to know it's done.
- **Status** — `planned`, `implementing`, `implemented`, `test-failed`, `done`.
- **Key files** — which files will be touched.

`task_context.md` captures research: key files found, patterns to follow, decisions made, risks.

### Mark change locations

Place `// [PLAN]` comments in the codebase at the exact locations where changes will go. These act as anchors for implementation.

### Commit the plan

Stage all planning artifacts (task files + `[PLAN]` comments) and commit. No implementation code in the plan commit. Push after committing.

## 6. Implementation

- Pick tasks with status `planned` (or `test-failed` if fixing).
- Update status to `implementing` before starting.
- Write the code. Remove `[PLAN]` comments as you implement them.
- Update status to `implemented` when done.
- One commit per task. Don't bundle unrelated changes into one commit.
- If the approach is fundamentally broken, set status to `replan` with an explanation and stop.

## 7. Review Your Work

- After implementing, review your changes before pushing.
- Check against:
  - **Coding standards** — `.claude/rules/coding-standards.md` (comments, error handling, null safety, etc.).
  - **Project patterns** — the project's `CLAUDE.md` and `.claude/knowledge/` conventions.
  - **The original plan** — does the code do what was planned? Any leftover `[PLAN]` comments?
- Check review skill    
- Fix any issues found during review before pushing.
- Update task status to `done` after review passes.

## 8. Test

- Check test skill  
- Write or update tests for changes that affect behavior.
- Run existing tests to verify nothing is broken.
- If tests fail, fix the code — don't skip or disable tests. Update task status to `test-failed` with failure details.

## 9. Document After Code Is Done

- After all code is written and reviewed, update documentation:
  - Project docs if features, endpoints, or APIs were added or changed.
  - README if setup steps, usage, or architecture changed.
  - `.claude/knowledge/` files if key patterns or architectural decisions changed.
- Check document skill   
- Only update existing docs unless a new feature clearly needs a new doc file.
- Commit documentation updates separately.
