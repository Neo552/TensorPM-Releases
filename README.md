# TensorPM Releases

Public distribution repository for TensorPM desktop binaries and agent integration metadata.

TensorPM is free for local use.  
For AI capabilities outside MCP (A2A), use BYOK (your own API key) or an account.

- Website: [tensorpm.com](https://tensorpm.com)
- Canonical app repository: [Neo552/TensorPM](https://github.com/Neo552/TensorPM) (private)
- Latest release page: [GitHub Releases](https://github.com/Neo552/TensorPM-Releases/releases)

## What This Repo Publishes

- Desktop release artifacts (Windows, macOS, Linux)
- Claude marketplace metadata:
  - `.claude-plugin/marketplace.json`
  - `plugins/tensorpm/.claude-plugin/plugin.json`
  - `plugins/tensorpm/skills/tensorpm/SKILL.md`
- Agent docs at repository root:
  - `SKILL.md`
  - `MCP-TOOLS.md`
  - `A2A-API.md`
  - `ACTION-ITEMS.md`

## Quick Install

### Desktop App

- Website installer: <https://tensorpm.com>
- Latest direct binaries:
  - Windows: [`TensorPM-Setup.exe`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Setup.exe)
  - macOS: [`TensorPM-macOS.dmg`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-macOS.dmg)
  - Linux AppImage: [`TensorPM-Linux.AppImage`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.AppImage)
  - Linux DEB: [`TensorPM-Linux.deb`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.deb)
  - Linux RPM: [`TensorPM-Linux.rpm`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.rpm)

### Claude Code Marketplace

```bash
/plugin marketplace add Neo552/TensorPM-Releases
/plugin install tensorpm@tensorpm-marketplace
```

## Agent Docs

Browsable docs:

- [`SKILL.md`](./SKILL.md)
- [`MCP-TOOLS.md`](./MCP-TOOLS.md)
- [`A2A-API.md`](./A2A-API.md)
- [`ACTION-ITEMS.md`](./ACTION-ITEMS.md)

Raw URLs:

- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/SKILL.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/MCP-TOOLS.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/A2A-API.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/ACTION-ITEMS.md>

## Release Sync

On each tagged TensorPM release, CI updates this repository automatically (binaries + marketplace + docs).

## Support

- Product documentation: <https://tensorpm.com/en/docs>
- Issues related to public release artifacts or marketplace metadata: open an issue in this repository.
