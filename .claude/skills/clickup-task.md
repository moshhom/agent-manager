# ClickUp Task

Pull and work on ClickUp tasks tagged with `claude`. Requires ClickUp MCP server configured with API token.

## Prerequisites

- ClickUp MCP server configured in `.mcp.json` or Claude settings
- API token with access to the target workspace
- Tasks must have `claude` tag to be picked up

## Flow

1. **Fetch tasks** — Query ClickUp for tasks with `claude` tag that are not `in progress`.
2. **Set status** — Move task to `in progress` immediately to prevent re-processing.
3. **Parse task** — Extract:
   - Repository URLs/names from task description
   - Task requirements and acceptance criteria
   - Any linked tasks or dependencies
4. **Clone repos** — For each repo mentioned:
   - Clone if not present locally
   - Pull latest if already cloned
   - Create `claude/<task-id>` branch
5. **Execute task** — Follow `.claude/rules/workflow-rules.md`:
   - Create task files in `.claude/<branch-name>/`
   - Plan, implement, test, review
6. **Update ClickUp** — When complete:
   - Add comment with summary of changes
   - Link PRs created
   - Move to `complete` or `review` status

## Task Description Format

Tasks should include repo info in description:

```
Repos: owner/repo1, owner/repo2
---
<actual task description>
```

Or use ClickUp custom fields for repo list.

## Status Mapping

| ClickUp Status | Meaning |
|----------------|---------|
| Open/To Do | Available for pickup |
| In Progress | Claude is working on it |
| Review | Work done, needs human review |
| Complete | Task finished |

## Error Handling

- If task fails, add error comment to ClickUp task and set status to `blocked`
- Include error details and what was attempted
- Do not leave tasks in `in progress` if work cannot continue
