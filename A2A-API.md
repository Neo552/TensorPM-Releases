# A2A API

## Endpoint

- Base URL: `http://localhost:37850`
- Trust model: localhost-only.
- Optional auth: set `A2A_HTTP_AUTH_TOKEN` before starting TensorPM to require token validation.

## Agent Discovery

```bash
# Root agent card
curl http://localhost:37850/.well-known/agent.json

# List projects with per-project agent URLs
curl http://localhost:37850/projects

# Project agent card
curl http://localhost:37850/projects/{projectId}/.well-known/agent.json
```

## JSON-RPC Methods

| Method              | Purpose                     |
| ------------------- | --------------------------- |
| `message/send`      | Blocking request/response   |
| `message/stream`    | Streaming response via SSE  |
| `tasks/get`         | Fetch task by ID (+history) |
| `tasks/list`        | List tasks with filters     |
| `tasks/cancel`      | Cancel running task         |
| `tasks/resubscribe` | Resume streaming updates    |

Conversation continuity:

- Pass `contextId` in subsequent `message/send` requests.

## REST Endpoints

| Method  | Endpoint                                 | Purpose                                            |
| ------- | ---------------------------------------- | -------------------------------------------------- |
| `GET`   | `/.well-known/agent.json`                | Root agent card                                    |
| `GET`   | `/projects`                              | List projects with agent URLs                      |
| `POST`  | `/projects`                              | Create project                                     |
| `GET`   | `/projects/:id`                          | Read project data                                  |
| `GET`   | `/projects/:id/.well-known/agent.json`   | Project agent card                                 |
| `GET`   | `/projects/:id/contexts`                 | List conversations                                 |
| `GET`   | `/projects/:id/contexts/:ctxId/messages` | Read message history                               |
| `GET`   | `/projects/:id/action-items`             | List action items (filtered)                       |
| `POST`  | `/projects/:id/action-items`             | Create action items                                |
| `PATCH` | `/projects/:id/action-items/:itemId`     | Update one action item                             |
| `POST`  | `/projects/:id/a2a`                      | JSON-RPC endpoint                                  |
| `GET`   | `/workspaces`                            | List workspaces                                    |
| `POST`  | `/workspaces/:id/activate`               | Activate workspace                                 |
| `GET`   | `/integrations/mcp/status`               | Check MCP integration status and supported clients |
| `POST`  | `/integrations/mcp/install`              | Install MCP integration for a specific client      |

## Examples

```bash
# Send high-level planning request to project manager agent
curl -X POST http://localhost:37850/projects/{projectId}/a2a \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "method": "message/send",
    "id": "1",
    "params": {
      "message": {
        "role": "user",
        "parts": [{"kind": "text", "text": "List high-priority items"}]
      }
    }
  }'
```

```bash
# Create a project (basic)
curl -X POST http://localhost:37850/projects \
  -H "Content-Type: application/json" \
  -d '{"name":"New Project","description":"Optional description"}'
```

```bash
# Create a project from prompt (async)
curl -X POST http://localhost:37850/projects \
  -H "Content-Type: application/json" \
  -d '{"name":"Mobile App","mode":"fromPrompt","prompt":"Build a habit tracker with streaks"}'
```

```bash
# Create a project from file (async)
curl -X POST http://localhost:37850/projects \
  -H "Content-Type: application/json" \
  -d '{"name":"From Brief","mode":"fromFile","documentPath":"/path/to/brief.pdf"}'
```

```bash
# Check MCP integration status
curl http://localhost:37850/integrations/mcp/status
```

```bash
# Install Codex MCP integration for the current user
curl -X POST http://localhost:37850/integrations/mcp/install \
  -H "Content-Type: application/json" \
  -d '{"client":"codex"}'
```
