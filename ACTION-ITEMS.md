# Action Items and Dependencies

## Action Item Fields

| Field | Type | Notes |
|-------|------|-------|
| `id` | string | UUID; auto-generated on create |
| `displayId` | number | Human-readable running number |
| `text` | string | Short title |
| `description` | string | Detailed body |
| `status` | string | `open`, `inProgress`, `completed`, `blocked` |
| `categoryId` | string | Category UUID |
| `assignedPeople` | string[] | Person UUIDs or names |
| `dueDate` | string | ISO date (`YYYY-MM-DD`), required |
| `startDate` | string or `null` | ISO date or clear with `null` |
| `urgency` | string | `very low`, `low`, `medium`, `high`, `overdue` |
| `impact` | string | `minimal`, `low`, `medium`, `high`, `critical` |
| `complexity` | string | `very simple`, `simple`, `moderate`, `complex`, `very complex` |
| `priority` | number | Score `1-100` |
| `planEffort` | object or `null` | `{ value, unit: "hours" | "days" }` |
| `planBudget` | object or `null` | `{ amount, currency? }` |
| `manualEffort` | object or `null` | Actual effort |
| `isBudget` | object or `null` | Actual budget spent |
| `blockReason` | string | Required context when blocked |
| `dependencies` | array | Dependency links |

## Dependency Types

| Type | Name | Meaning |
|------|------|---------|
| `FS` | Finish-to-Start | B starts after A finishes |
| `SS` | Start-to-Start | B starts after A starts |
| `FF` | Finish-to-Finish | B finishes after A finishes |
| `SF` | Start-to-Finish | B finishes after A starts |

## Dependency Payload Examples

Create with dependency:

```json
{
  "actionItems": [
    {
      "text": "Task A - Research",
      "complexity": "simple"
    },
    {
      "text": "Task B - Implementation",
      "complexity": "moderate",
      "dependencies": [
        { "sourceId": "<id-of-task-A>", "type": "FS" }
      ]
    }
  ]
}
```

Update dependencies:

```json
{
  "updates": [
    {
      "id": "<action-item-id>",
      "dependencies": [
        { "sourceId": "<other-item-id>", "type": "FS" },
        { "sourceId": "<another-item-id>", "type": "SS" }
      ]
    }
  ]
}
```

Notes:
- `sourceId` must point to an existing action item.
- In MCP updates, setting `dependencies` replaces existing ones.
- Set `dependencies: []` to clear all dependencies.
