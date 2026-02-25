# Agent Coding Manager

Central source of truth for rules, agents, and skills that govern how Claude (AI agent) operates across your repositories.

## Structure

```
.claude/                         <- distributable — copied into target projects
├── CLAUDE.md                    # Instructions for target projects
├── rules/
│   ├── workflow-rules.md        # Branches, context files, task tracking
│   └── coding-standards.md      # Architecture, comments, error handling, null safety
├── skills/                      # Workflow steps — agents follow these, or use inline
│   ├── test.md                  # Testing workflow
│   ├── review.md                # Review workflow
│   └── document.md              # Documentation workflow
├── agents/                      # Optional subagent wrappers — point subagents to skills
│   ├── tester.md                # Wraps skills/test.md
│   ├── reviewer.md              # Wraps skills/review.md
│   └── documenter.md            # Wraps skills/document.md
└── knowledge/                   # Project-specific knowledge (not overwritten on update)
    └── main.md                  # Template — projects add their own files here

CLAUDE.md                        <- repo-only — how to work on this repo
README.md                        <- repo-only — this file
```

## How to install in a target project

### 1. Preserve existing project knowledge

If the target project already has a `.claude/` directory with project-specific content, **preserve it as knowledge** before copying:

- Move the existing `.claude/CLAUDE.md` to `.claude/knowledge/project-claude.md` (or a descriptive name).
- Move any other project-explaining files from `.claude/` into `.claude/knowledge/`.

```bash
# Example: preserve existing project files before install
mkdir -p <target-project>/.claude/knowledge
mv <target-project>/.claude/CLAUDE.md <target-project>/.claude/knowledge/project-claude.md
```

### 2. Copy the distributable package

```bash
cp -r <path-to-agent-coding-manager>/.claude/ <target-project>/.claude/
```

This gives the target project the rules, agents, and skills. Files in `knowledge/` are not overwritten.

### 3. Verify the root CLAUDE.md

The target project's root `CLAUDE.md` must tell the agent to read `.claude/CLAUDE.md`. Add this line:

```
Read and follow everything in `.claude/CLAUDE.md` before starting any work.
```

Without this, the agent won't pick up the rules, skills, or agents.

## How to update a target project

Same as install — copy `.claude/` again, replacing baseline files. Files in `.claude/knowledge/` are project-specific and will not be overwritten.

After copying, verify the project's root `CLAUDE.md` still references `.claude/CLAUDE.md`.

## How to edit rules, agents, or skills

1. Create a `claude/` prefixed branch in this repo.
2. Edit files inside `.claude/`.
3. Push the branch for review.
4. After merging, update target projects by copying `.claude/` again.
