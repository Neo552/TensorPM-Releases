---
name: tensorpm-agentic-pm
description: 'Agentic Project Memory | AI Project Management Software. The dedicated project management subagent for Claude Code, Codex, OpenClaw and any MCP/A2A agent — manages shared project context (goals, action items, decisions, history) via MCP tools and the A2A protocol. Local-first, free.'
compatibility: Requires the TensorPM desktop app v0.8.0+ to be running for MCP tool access and A2A communication. Available on macOS, Windows, and Linux.
---

# TensorPM Skill

Use this skill for AI-powered, context-driven project management inside a running TensorPM desktop app.
TensorPM itself is free. For AI capabilities outside MCP (A2A), use your own API key (BYOK) or create an account.

## When To Use

- You need to list, create, or update TensorPM projects or action items.
- You need to switch/list workspaces.
- You need to set AI provider keys through TensorPM (`set_api_key`).
- You need conversational project-level changes via A2A (`message/send`).
- You need to make external MCP servers available inside TensorPM's AI chat.

## When Not To Use

- The request is only about website/account/billing pages.

## Installation (Agent CLI)

Preferred package-manager install:

```bash
# macOS
brew install --cask neo552/tensorpm/tensorpm
```

```powershell
# Windows (PowerShell)
winget install --id Neo552.TensorPM --exact --accept-package-agreements --accept-source-agreements
```

Direct-download fallback (works on every platform, no installer script required):

```bash
# macOS DMG
curl -fL -o /tmp/TensorPM-macOS.dmg \
  https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-macOS.dmg
hdiutil attach /tmp/TensorPM-macOS.dmg -nobrowse -quiet
cp -R "/Volumes/$(ls /Volumes | grep -i TensorPM)/TensorPM.app" /Applications/
hdiutil detach "/Volumes/$(ls /Volumes | grep -i TensorPM)" -quiet
```

```bash
# Linux AppImage
curl -fL -o ~/TensorPM.AppImage \
  https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.AppImage
chmod +x ~/TensorPM.AppImage
```

```powershell
# Windows installer
Invoke-WebRequest -Uri https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Setup.exe -OutFile $env:TEMP\TensorPM-Setup.exe
Start-Process $env:TEMP\TensorPM-Setup.exe
```

If the agent needs to verify or pin a version, list releases via `gh release list -R Neo552/TensorPM-Releases` (the GitHub `/releases/latest` endpoint excludes pre-releases, so prefer `gh release list` when only beta tags exist).

## Runtime Prerequisites

1. Start TensorPM desktop app.
2. For MCP usage with external AI clients: ensure client integration is installed once (via **Settings -> Integrations** or A2A `POST /integrations/mcp/install`).
3. For A2A usage: verify local endpoint `http://localhost:37850`.

## External MCPs Inside TensorPM

When a user asks to give TensorPM access to external MCP servers, write an MCP config file and let TensorPM import it on startup.

Preferred paths:

1. User-wide: `~/.tensorpm/agent-mcps.json`
2. Project-local: `.tensorpm/agent-mcps.json`
3. Local override: `.tensorpm/agent-mcps.local.json`
4. Explicit override: set `TENSORPM_AGENT_MCP_CONFIG_FILE` to one or more paths separated by the OS path delimiter.

TensorPM accepts standard `mcpServers` config blocks and TensorPM-native `agentMcpServers`. Use `references/agent-mcp-clients.md` for schema, examples, and safety notes.
The UI exports its own generated snapshot to `~/.tensorpm/agent-mcps.generated.json`; agents should write user-managed config to `~/.tensorpm/agent-mcps.json`.

## MCP vs A2A Routing

| Task                                        | Use                  | Why                              |
| ------------------------------------------- | -------------------- | -------------------------------- |
| Structured action-item CRUD                 | MCP tools            | Direct typed operations          |
| Set provider API keys                       | MCP `set_api_key`    | Dedicated secure write-only tool |
| Project-wide/contextual changes             | A2A `message/send`   | Managed by project manager agent |
| HTTP-based automation/client integration    | A2A REST/JSON-RPC    | Endpoint-first integration path  |
| Multi-turn planning with conversation state | A2A with `contextId` | Native conversation continuity   |

Rule of thumb:

- Prefer MCP for explicit CRUD operations.
- Prefer A2A for high-level intent and context-aware planning.

## Minimal Workflow

1. Verify TensorPM is running.
2. Choose MCP vs A2A via the routing table above.
3. Execute operation.
4. Read back result (`list_*`, `get_project`, or A2A read endpoint) to confirm state.
5. Summarize applied changes and any follow-up action.

## References

- [MCP Tools](MCP-TOOLS.md): tool catalog and usage boundaries.
- [Agent MCP Clients](MCP-TOOLS.md): configure external MCP servers for TensorPM's AI chat.
- [A2A API](A2A-API.md): discovery, JSON-RPC methods, REST endpoints, examples.
- [Action Items & Dependencies](ACTION-ITEMS.md): fields, dependency types, payload examples.

## Notes

- IDs are UUIDs.
- Dates use ISO format (`YYYY-MM-DD`).
- `propose_updates` requires human approval before apply.
- MCP and A2A operate on the same local TensorPM data.
- Release notes: <https://github.com/Neo552/TensorPM-Releases/releases/latest>
