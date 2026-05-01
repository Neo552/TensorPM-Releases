# Agent MCP Clients

TensorPM can import external MCP client configuration on startup so its own AI chat can call tools from other MCP servers.

## Config Locations

TensorPM imports these files in order:

1. `~/.tensorpm/agent-mcps.json`
2. `.tensorpm/agent-mcps.json`
3. `.tensorpm/agent-mcps.local.json`

Set `TENSORPM_AGENT_MCP_CONFIG_FILE` to override discovery. Multiple paths can be separated with the OS path delimiter (`:` on macOS/Linux, `;` on Windows).

The connector UI writes the current Agent MCP client list to `~/.tensorpm/agent-mcps.generated.json` whenever an MCP client is added, updated, or removed. Use the **MCP config** button in the connector overview to reveal that generated file. TensorPM does not import the generated file on startup; it is an export/sync artifact for inspection and external agents.

Secret values from UI fields are not written as plaintext. TensorPM writes `${TENSORPM_MCP_...}` placeholders in the JSON file and keeps the real values in encrypted local storage.

## Standard MCP Config

Use `mcpServers` for compatibility with Codex, Claude Code, and other MCP clients:

```json
{
  "mcpServers": {
    "context7": {
      "displayName": "Context7",
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"],
      "toolPrefix": "context7",
      "allowWriteToolsWithoutApproval": false
    }
  }
}
```

In TensorPM's connector overview, use `+ -> Context7 MCP` to add this example without editing files.

TensorPM extensions:

- `displayName`: UI label; falls back to the map key.
- `toolPrefix`: prefix for TensorPM tool names, e.g. `mcp_context7__resolve_library_id`.
- `allowWriteToolsWithoutApproval`: set `false` unless the user explicitly wants auto-approval.
- `scope`: `global` or `project`.
- `projectId`: required when `scope` is `project`.

## HTTP MCP Config

Use `agentMcpServers` when the server is HTTP-first or needs explicit TensorPM fields:

```json
{
  "agentMcpServers": [
    {
      "name": "Linear",
      "transport": "streamable-http",
      "url": "https://mcp.linear.app/mcp",
      "toolPrefix": "linear",
      "enabled": true
    }
  ]
}
```

Bearer-token example:

```json
{
  "agentMcpServers": [
    {
      "name": "Example MCP",
      "transport": "streamable-http",
      "url": "https://example.com/mcp",
      "headers": {
        "Authorization": "Bearer ${EXAMPLE_MCP_TOKEN}"
      },
      "toolPrefix": "example"
    }
  ]
}
```

Environment placeholders use `${NAME}` and are expanded from TensorPM's process environment.
If an import references an environment variable that is not set, TensorPM skips that server and leaves any existing saved configuration unchanged.

## Local Test Server

For local development, this repo includes a dummy stdio MCP:

```json
{
  "mcpServers": {
    "dummy": {
      "displayName": "Dummy MCP",
      "command": "node",
      "args": ["scripts/dummy-mcps/dummy-mcp.mjs"],
      "toolPrefix": "dummy"
    }
  }
}
```

The dummy exposes read-only, unannotated, destructive, slow, and failing tools so approvals and chat tool-call handling can be tested.

## Safety

- Default write-like tools to approval-required.
- Do not put secrets directly into committed config files; use `${ENV_VAR}` placeholders.
- HTTP MCP URLs targeting localhost or private IPs are blocked unless `TENSORPM_ALLOW_PRIVATE_MCP=1` is set for trusted local testing.
- Restart TensorPM after writing config so startup import runs.
