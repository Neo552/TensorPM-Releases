# TensorPM Releases

Public distribution repository for TensorPM desktop builds and agent integration metadata.

- Main application repository: [Neo552/TensorPM](https://github.com/Neo552/TensorPM) (private)
- Website: [tensorpm.com](https://tensorpm.com)
- Latest releases: [GitHub Releases](https://github.com/Neo552/TensorPM-Releases/releases)

TensorPM is free for local use.

## Download (Latest)

- Windows: [`TensorPM-Setup.exe`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Setup.exe)
- macOS: [`TensorPM-macOS.dmg`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-macOS.dmg)
- Linux AppImage: [`TensorPM-Linux.AppImage`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.AppImage)
- Linux DEB: [`TensorPM-Linux.deb`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.deb)
- Linux RPM: [`TensorPM-Linux.rpm`](https://github.com/Neo552/TensorPM-Releases/releases/latest/download/TensorPM-Linux.rpm)

## Agent Docs (Always Latest)

- [`SKILL.md`](./SKILL.md)
- [`MCP-TOOLS.md`](./MCP-TOOLS.md)
- [`A2A-API.md`](./A2A-API.md)
- [`ACTION-ITEMS.md`](./ACTION-ITEMS.md)

Raw URLs:

- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/SKILL.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/MCP-TOOLS.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/A2A-API.md>
- <https://raw.githubusercontent.com/Neo552/TensorPM-Releases/main/ACTION-ITEMS.md>

## Claude Code Marketplace

```bash
/plugin marketplace add Neo552/TensorPM-Releases
/plugin install tensorpm@tensorpm-marketplace
```

## Sync Policy

On each tagged TensorPM release, CI syncs this repository with:

- release binaries and GitHub release assets
- `.claude-plugin/marketplace.json`
- `plugins/tensorpm/.claude-plugin/plugin.json`
- `plugins/tensorpm/skills/tensorpm/SKILL.md`
- root agent docs (`SKILL.md`, `MCP-TOOLS.md`, `A2A-API.md`, `ACTION-ITEMS.md`)
