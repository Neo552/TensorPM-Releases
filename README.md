<div align="center">
  <h1>TensorPM Releases</h1>
  <p><strong>Your Project OS for You, Your Agents, and Your Team.</strong></p>
  <p>Local-first. AI-powered. Context-driven.</p>
</div>

> Experience agentic, context-driven project management with TensorPM.
>
> Other tools store tasks. TensorPM stores evolving intent.

Public distribution repository for TensorPM desktop binaries, marketplace metadata, and agent docs.

TensorPM itself is free for local-first use.  
For AI capabilities outside MCP (A2A), use BYOK (your own API key) or an account.

- Website: [tensorpm.com](https://tensorpm.com)
- Canonical app repository: [Neo552/TensorPM](https://github.com/Neo552/TensorPM) (private)
- Latest releases: [GitHub Releases](https://github.com/Neo552/TensorPM-Releases/releases)

## Why TensorPM

| Traditional tools + blind AI | TensorPM |
| --- | --- |
| You re-explain project context repeatedly | Context compounds over time across agents and humans |
| PM data is fragmented across tools | MCP + A2A provide a unified, live project surface |
| High-level updates lose intent | Project manager agent keeps context continuity |

## Agentic Architecture (At a Glance)

| Layer | Role |
| --- | --- |
| **Project Manager Agent** | Per-project AI agent via A2A (`message/send`) with full project context; coordinates high-level context changes. |
| **Internal Subagents** | Specialized subagents for action-item generation, splitting, and re-evaluation workflows. |
| **Copilot Agent (Optional)** | GitHub Copilot sub-agent bridge for repo-aware code/issue/PR/commit queries (when Copilot CLI is available and enabled). |
| **External Agents** | MCP tools for structured CRUD + A2A REST/JSON-RPC for conversational, context-aware operations. |

## Install TensorPM

### Website / Direct Downloads

- Website installer: <https://tensorpm.com>
- Windows: [`TensorPM-Setup.exe`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Setup.exe)
- macOS: [`TensorPM-macOS.dmg`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-macOS.dmg)
- Linux AppImage: [`TensorPM-Linux.AppImage`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.AppImage)
- Linux DEB: [`TensorPM-Linux.deb`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.deb)
- Linux RPM: [`TensorPM-Linux.rpm`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.rpm)

### CLI (All Platforms)

```bash
# macOS
brew install --cask neo552/tensorpm/tensorpm
```

```powershell
# Windows (PowerShell)
winget install --id Neo552.TensorPM --exact --accept-package-agreements --accept-source-agreements
```

```bash
# macOS / Linux fallback installer script
curl -fsSL https://raw.githubusercontent.com/Neo552/TensorPM/main/scripts/install.sh | bash
```

```powershell
# Windows fallback installer script
irm https://raw.githubusercontent.com/Neo552/TensorPM/main/scripts/install.ps1 | iex
```

## Connect Your Agent

1. Start the TensorPM desktop app.
2. Install MCP client integration once:
   - UI: `Settings -> Integrations`
   - API: `POST /integrations/mcp/install`
3. Use MCP for structured CRUD and A2A for high-level, context-aware project-manager-agent requests.
4. For A2A, use `http://localhost:37850` (optional token auth via `A2A_HTTP_AUTH_TOKEN`).

## Claude Code Marketplace

```bash
/plugin marketplace add Neo552/TensorPM-Releases
/plugin install tensorpm@tensorpm-marketplace
```

## Agent Docs

Browsable:

- [`SKILL.md`](./SKILL.md)
- [`MCP-TOOLS.md`](./MCP-TOOLS.md)
- [`A2A-API.md`](./A2A-API.md)
- [`ACTION-ITEMS.md`](./ACTION-ITEMS.md)

Raw:

- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/SKILL.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/MCP-TOOLS.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/A2A-API.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/ACTION-ITEMS.md>

## What This Repo Publishes

- Desktop release artifacts (Windows, macOS, Linux)
- Claude marketplace metadata:
  - `.claude-plugin/marketplace.json`
  - `plugins/tensorpm/.claude-plugin/plugin.json`
  - `plugins/tensorpm/skills/tensorpm/SKILL.md`
- Root agent docs:
  - `SKILL.md`
  - `MCP-TOOLS.md`
  - `A2A-API.md`
  - `ACTION-ITEMS.md`

## Release Sync

On each tagged TensorPM release, CI syncs this repository automatically (binaries, marketplace metadata, and docs).

## Support

- Product docs: <https://tensorpm.com/en/docs>
- Issues related to release artifacts or marketplace metadata: open an issue in this repository.
