# CLAUDE.md

This `.claude/` directory is managed by agent-coding-manager. Follow everything in it.

## Rules

Read and follow all files in `.claude/rules/` and `.claude/knowledge/` **directly using the Read tool** before starting any work. Never delegate reading these to subagents.

- **`rules/workflow-rules.md`** — Branches, context files, task tracking.
- **`rules/coding-standards.md`** — Architecture, comments, error handling, null safety.

## Agents & Skills

Agents and skills are available as **optional tools** — use them when they add value, not as a mandatory workflow. You decide when to delegate to a subagent vs do the work yourself.

| Agent | File | Skill | Use when... |
|---|---|---|---|
| Tester | `agents/tester.md` | `skills/test.md` | You want to delegate test writing/running. |
| Reviewer | `agents/reviewer.md` | `skills/review.md` | You want an independent code review. |
| Documenter | `agents/documenter.md` | `skills/document.md` | You want to delegate documentation updates. |

**How to use:** Read the agent's `.md` file and spawn a subagent via the Task tool. Or read the skill directly and follow it yourself. Your call.

Projects may add their own agents and skills. Don't modify the baseline ones — they're managed by agent-coding-manager and will be overwritten on update.

## Knowledge Files

`.claude/knowledge/` holds project-specific knowledge — architecture, patterns, codebase context. These files are never overwritten when agent-coding-manager is updated.

## Updating

Copy `.claude/` from agent-coding-manager to update baseline files. Knowledge files are preserved. Verify the project's root `CLAUDE.md` references `.claude/CLAUDE.md`.
