# CLAUDE.md — agent-coding-manager

This is the agent-coding-manager repo — the central source of truth for rules, agents, and skills that govern how Claude (AI agent) operates across your repositories.

## How this repo works

The `.claude/` directory is the **distributable package**. Its contents get copied directly into target projects as their `.claude/` directory.

### What's distributable (inside `.claude/`)

- **`CLAUDE.md`** — Minimal instructions for target projects.
- **`rules/`** — Workflow and coding standards. Applied to all repos using agent-coding-manager.
- **`agents/`** — Optional subagent definitions (tester, reviewer, documenter).
- **`skills/`** — Workflow step definitions that agents follow.
- **`knowledge/`** — Project-specific knowledge (not overwritten on update). Ships with a template `main.md`; target projects add their own files here.

### What's repo-only (outside `.claude/`)

- **Root `CLAUDE.md`** (this file) — Instructions for working on this repo.
- **`README.md`** — Repo documentation.

## Installing in a target project

### Preserve existing project knowledge

Before copying `.claude/` into a target project, check whether the project already has files that contain project-specific knowledge. These include:

- **An existing `.claude/CLAUDE.md`** — may contain project-specific instructions.
- **Existing `.claude/knowledge/` files** — project-specific knowledge files.

If any of these exist, **move them into `.claude/knowledge/`** before copying, so they are preserved:

1. Create `.claude/knowledge/` in the target project if it doesn't exist.
2. Move the existing `.claude/CLAUDE.md` to `.claude/knowledge/project-claude.md` (or a descriptive name).
3. Move any other project-specific files from `.claude/` into `.claude/knowledge/`.

Then copy this repo's `.claude/` into the target project. The baseline files will be replaced, but anything in `knowledge/` will remain.

### Verify the root CLAUDE.md

After copying, **verify the target project's root `CLAUDE.md` references the inner file**. It must contain a line like:

```
Read and follow everything in `.claude/CLAUDE.md` before starting any work.
```

Without this, the agent will not discover the rules, skills, or agents.

## Working on this repo

- Edit `.claude/CLAUDE.md`, rules in `.claude/rules/`, agents in `.claude/agents/`, skills in `.claude/skills/`.
- Everything inside `.claude/` will be distributed to target projects — do not put repo-specific content there.
- Use `claude/` prefixed branches for changes. Never push directly to `main`.
