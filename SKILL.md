---
name: tensorpm-agentic-pm
description: Manage structured projects with long-term stable memory shared across humans and AI agents. Use this skill whenever the work needs a dedicated project manager (or project-manager agent) — including action items, kanban, sprint planning, decisions, history, and anything that benefits from persistent project context across sessions and agents. Read and write the project graph via MCP tools and the A2A protocol. Triggers: TensorPM, Context-Driven Project Management, CDPM, agentic project management, project memory, or any request to coordinate work with the local TensorPM desktop app. Local-first, MCP-native, BYOK, free. Requires TensorPM desktop app running on macOS, Windows, or Linux.
---

# TensorPM Skill

TensorPM is the project context layer: one project graph (goals, action items, decisions, history) shared by humans (desktop app) and AI agents (MCP + A2A). Use this skill to read and write that graph.

## Install

```bash
# macOS
brew install --cask neo552/tensorpm/tensorpm

# Windows
winget install --id Neo552.TensorPM --exact --accept-package-agreements --accept-source-agreements
```

Direct downloads (DMG / AppImage / Setup.exe) and version pinning instructions: see `references/install.md` if present, or [TensorPM Releases](https://github.com/Neo552/TensorPM-Releases/releases/latest). Prefer `gh release list -R Neo552/TensorPM-Releases` when only beta tags exist.

## Use TensorPM in Any MCP Client

Built-in installers for Claude Desktop, Claude Code, Codex, Continue, Antigravity, and Cursor live under **TensorPM → Settings → Integrations**. For any other MCP-capable client, point it at the bundled stdio server:

```json
{
  "mcpServers": {
    "tensorpm": {
      "command": "node",
      "args": ["<absolute-path-to>/dist/backend/mcp/server.js"]
    }
  }
}
```

The exact `server.js` path per OS is shown in **Settings → Integrations → Manual Setup**. No env vars or auth tokens go in the client config — the server reads its bridge token from `~/.tensorpm/mcp-bridge-token` (mode `0600`, auto-rotated). The desktop app must be running when the client invokes the server.

TOML form (Codex `~/.codex/config.toml`):

```toml
[mcp_servers.tensorpm]
command = "node"
args = ["<absolute-path-to>/dist/backend/mcp/server.js"]
```

YAML form (Continue `~/.continue/config.yaml`):

```yaml
mcpServers:
  - name: tensorpm
    command: node
    args:
      - <absolute-path-to>/dist/backend/mcp/server.js
```

## MCP vs A2A — Routing

| Task                                                                 | Use                                                                  |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| List/create/update action items                                      | MCP tools                                                            |
| Switch or list workspaces                                            | MCP tools                                                            |
| Set provider API keys                                                | MCP `set_api_key`                                                    |
| Bug report with diagnostic bundle                                    | MCP `submit_bug_report`                                              |
| Non-bug feedback (suggestion, partnership, licensing, collaboration) | MCP `submit_feedback`                                                |
| Account, billing, credits, donations                                 | MCP billing tools (return browser URLs only — never confirm payment) |
| Project-wide / contextual changes                                    | A2A `message/send` to the project agent                              |
| Multi-turn planning with conversation state                          | A2A with `contextId`                                                 |

Default: MCP for typed CRUD, A2A for intent and context-aware planning. Core project context (profile, budget, people, categories) can only be changed by the project agent — propose changes with `propose_updates` (human review required) or message the agent via A2A.

## Workflow

1. Confirm the TensorPM desktop app is running.
2. Pick MCP or A2A from the routing table.
3. Execute. Read back via `list_*` / `get_project` / A2A read endpoint to confirm state.
4. Summarize what changed.

## External MCPs Inside TensorPM

To give the in-app TensorPM agent access to other MCP servers, write a config file — TensorPM imports it on startup. Preferred locations (first match wins):

1. `~/.tensorpm/agent-mcps.json` (user-wide)
2. `.tensorpm/agent-mcps.json` (project-local)
3. `.tensorpm/agent-mcps.local.json` (local override)
4. `TENSORPM_AGENT_MCP_CONFIG_FILE` env var (one or more paths separated by the OS path delimiter)

Standard `mcpServers` and TensorPM-native `agentMcpServers` blocks are both accepted. The UI's own snapshot at `~/.tensorpm/agent-mcps.generated.json` is read-only — write user-managed config to `agent-mcps.json` instead.

## Conventions

- IDs are UUIDs. Dates use `YYYY-MM-DD`.
- A2A endpoint: `http://localhost:37850`. Verify with `GET /.well-known/agent.json`.
- `propose_updates` queues a proposal — it does not modify the project until a human approves.
- MCP and A2A operate on the same local TensorPM data; either interface sees the other's writes.

## References

- [MCP Tools](MCP-TOOLS.md) — tool catalog, usage boundaries, annotations.
- [A2A API](A2A-API.md) — discovery, JSON-RPC methods, REST endpoints, examples.
- [Action Items & Dependencies](ACTION-ITEMS.md) — schema, dependency types, payload examples.
- [Agent MCP Clients](AGENT-MCP-CLIENTS.md) — config schema for external MCPs inside TensorPM.
- [Releases](https://github.com/Neo552/TensorPM-Releases/releases/latest)
