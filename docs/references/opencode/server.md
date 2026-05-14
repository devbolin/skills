---
source: https://opencode.ai/docs/server/
retrieved: 2026-05-12
type: archived
---

# Server

Interact with opencode server over HTTP.

The `opencode serve` command runs a headless HTTP server that exposes an OpenAPI endpoint.

---

## Usage

```bash
opencode serve [--port <number>] [--hostname <string>] [--cors <origin>]
```

| Flag | Description | Default |
|---|---|---|
| `--port` | Port to listen on | 4096 |
| `--hostname` | Hostname to listen on | 127.0.0.1 |
| `--mdns` | Enable mDNS discovery | false |
| `--cors` | Additional browser origins | [] |

### Authentication

Set `OPENCODE_SERVER_PASSWORD` for HTTP basic auth. Username defaults to `opencode`.

---

## How it works

When you run `opencode` it starts a TUI and a server. The server exposes an OpenAPI 3.1 spec at `http://<hostname>:<port>/doc`.

---

## APIs

The server exposes APIs for: Global (health, events), Project (list, current), Path & VCS, Config (get, update, providers), Provider (list, auth, OAuth), Sessions (CRUD, init, fork, abort, share, revert, permissions), Messages (send, list, command, shell), Files (find text/files/symbols, read, status), Tools (experimental), LSP/Formatters/MCP status, Agents, Logging, TUI (append prompt, toast, execute command), Auth, Events (SSE), and Docs (OpenAPI spec).
