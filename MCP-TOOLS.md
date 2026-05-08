# MCP Tools

## Tool Catalog

| Tool                               | Purpose                                                                                                                           |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `list_projects`                    | List project IDs and names                                                                                                        |
| `create_project`                   | Create project (`basic`, `fromPrompt`, `fromFile`)                                                                                |
| `get_project`                      | Read complete project data                                                                                                        |
| `list_action_items`                | Query/filter action items                                                                                                         |
| `submit_action_items`              | Create action items                                                                                                               |
| `update_action_items`              | Update existing action items                                                                                                      |
| `propose_updates`                  | Propose project updates for human review                                                                                          |
| `record_decision`                  | Capture a project decision/commitment as an append-only record (source: `stakeholder_commit`, `top_down`, `agent_recommendation`, `user_directive`, `derived`) |
| `supersede_decision`               | Atomically replace an active decision with a new one; previous record is marked `superseded` and chained via `supersedesId`       |
| `withdraw_decision`                | Mark an active decision as withdrawn (no longer applies, but stays in the audit chain). Prefer `supersede_decision` if replacing  |
| `list_decisions`                   | List decisions for a project, filterable by status (active/superseded/withdrawn), source, or linked entity                        |
| `link_decision`                    | Link a decision to an action item, risk, or milestone (idempotent)                                                                |
| `unlink_decision`                  | Remove a previously created decision link                                                                                         |
| `set_api_key`                      | Store AI provider API key (`openai`, `anthropic`, `google`, `mistral`)                                                            |
| `list_workspaces`                  | List workspaces and active workspace ID                                                                                           |
| `set_active_workspace`             | Switch active workspace                                                                                                           |
| `get_billing_status`               | Read account login state, billing capabilities, free-credit offer, and current credits when logged in                             |
| `get_available_billing_plans`      | List subscription plans, top-up packages, donation providers, donation amounts, and free-credit offer                             |
| `open_tensorpm_account`            | Return or open the browser-based TensorPM login/register/account URL; never accepts passwords                                     |
| `get_credit_balance`               | Read current credit balance for logged-in accounts                                                                                |
| `create_subscription_checkout`     | Create a Stripe subscription checkout URL for a logged-in account                                                                 |
| `create_credit_topup_checkout`     | Create a credit top-up checkout URL for logged-in Pro accounts only                                                               |
| `open_billing_portal`              | Open or return the billing portal URL for existing subscribers                                                                    |
| `create_support_donation_checkout` | Create a PayPal support donation URL; MCP never confirms or executes payment                                                      |
| `submit_bug_report`                | Submit a TensorPM bug report, defaulting to a support bundle without AI logs                                                      |
| `submit_feedback`                  | Submit non-bug feedback (suggestion, praise, question, partnership, licensing, collaboration, other) via the website contact form |
| `message_tensorpm_agent`           | Send a blocking project-level message to the TensorPM agent via the local A2A bridge                                              |

## Usage Boundary

- MCP is best for direct, structured CRUD workflows.
- Core project-context edits beyond explicit action-item fields should go through A2A `message/send`.
- Use `message_tensorpm_agent` only as the MCP adapter for blocking A2A `message/send`; use native A2A for streaming, task cancellation, or advanced session control.
- Billing tools only create or open browser URLs. Agents must not claim payment completion from MCP responses.
- `create_credit_topup_checkout` is available only for logged-in Pro accounts. `get_credit_balance` is available only when logged in.
- `submit_bug_report` includes telemetry and app logs by default, excludes AI logs by default, and skips oversized support bundles before JSON submission.
- `submit_feedback` is for non-bug input only — bugs must use `submit_bug_report` so diagnostic logs are attached. Feedback is one-way unless the caller passes an email, in which case the user receives an auto-reply confirming receipt.
- Use `list_projects` first if project IDs are unknown.
- Use `get_project` when you need category/person identifiers for valid action-item assignments or filters.
- Decisions are append-only. To change a previously recorded decision, call `supersede_decision` (creates a new record + chains the old one as `superseded`); never mutate or delete a decision in place. Use `withdraw_decision` only when no replacement applies.
- Call `list_decisions` before reasoning about a contentious or previously-debated topic so the audit trail informs your response.

## API-Key Setup Example

```text
set_api_key
  provider: "openai"
  api_key: "sk-..."
```

Behavior:

- Secure storage in TensorPM.
- Write-only; keys are not readable back.
