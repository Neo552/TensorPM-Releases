# MCP Tools

## Tool Catalog

| Tool | Purpose |
|------|---------|
| `list_projects` | List project IDs and names |
| `create_project` | Create project (`basic`, `fromPrompt`, `fromFile`) |
| `get_project` | Read complete project data |
| `list_action_items` | Query/filter action items |
| `submit_action_items` | Create action items |
| `update_action_items` | Update existing action items |
| `propose_updates` | Propose project updates for human review |
| `set_api_key` | Store AI provider API key (`openai`, `anthropic`, `google`, `mistral`) |
| `list_workspaces` | List workspaces and active workspace ID |
| `set_active_workspace` | Switch active workspace |

## Usage Boundary

- MCP is best for direct, structured CRUD workflows.
- Core project-context edits beyond explicit action-item fields should go through A2A `message/send`.
- Use `list_projects` first if project IDs are unknown.
- Use `get_project` when you need category/person identifiers for valid action-item assignments or filters.

## API-Key Setup Example

```text
set_api_key
  provider: "openai"
  api_key: "sk-..."
```

Behavior:
- Secure storage in TensorPM.
- Write-only; keys are not readable back.
