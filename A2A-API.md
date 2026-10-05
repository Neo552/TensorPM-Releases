# A2A API

## Endpoint

- Base URL: `http://localhost:37850`
- Trust model: localhost-only. Every request, discovery included, must carry a loopback `Host`
  header with the bridge's port (`localhost:37850`, `127.0.0.1:37850` or `[::1]:37850`). An
  `Origin` header, when present, must be the same loopback origin (e.g. `http://localhost:37850`).
  Anything else gets HTTP 403, including requests through Docker's `host.docker.internal`, a port
  forward to another port, or a reverse proxy that passes on a non-loopback `Host` (for example
  the client's original one). Non-browser clients (no `Origin` header) calling
  `http://localhost:37850` are unaffected.
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
| `message/send`      | Blocks until the turn ends  |
| `message/stream`    | Streaming response via SSE  |
| `tasks/get`         | Fetch task by ID (+history) |
| `tasks/list`        | List tasks with filters     |
| `tasks/cancel`      | Cancel running task         |
| `tasks/resubscribe` | Resume streaming updates    |

Conversation continuity:

- Pass `contextId` in subsequent `message/send` requests.

Long turns and cancellation:

- `message/send` answers only after the agent turn has ended, which can take several minutes.
- Closing the connection cancels the running turn. An HTTP client that gives up closes it too:
  Node's built-in `fetch`, for example, gives up after 300 s without response headers. Raise the
  client's timeout, or use `message/stream`: it sends its headers and first event at once, and
  that event already carries the task id for `tasks/cancel`. It sends no keep-alive events (token
  events pause during tool calls), so also allow long gaps between events.
- `tasks/cancel` with the task's id also cancels the turn. Pass your own `params.contextId` on the
  first `message/send` (an unknown id starts a new conversation); while the call blocks, you can
  then find the id with `tasks/list`, filtered by that `contextId` and `states: ["working"]`.
- Changes the agent already applied are kept.

## Agent self-scheduling

The TensorPM project agent can schedule its own future runs (reminders, follow-ups, time-bound checks). External agents trigger this via `message/send` describing the future intent — there is no dedicated method, the project agent decides whether the request warrants a scheduled invocation. Scheduled runs are persisted on the project (`scheduled_invocations`) and fire at the requested time with the original prompt + full project context.

```bash
# Ask the project agent to schedule a future check-in
curl -X POST http://localhost:37850/projects/{projectId}/a2a \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "method": "message/send",
    "id": "1",
    "params": {
      "message": {
        "role": "user",
        "parts": [{"kind": "text", "text": "Remind me next Tuesday to review the Q3 budget burn-down."}]
      }
    }
  }'
```

There is no MCP tool for self-scheduling — scheduling lives with the agent so it has full context when it fires.

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
