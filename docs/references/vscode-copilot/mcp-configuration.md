---
source: https://code.visualstudio.com/docs/copilot/reference/mcp-configuration
retrieved: 2026-05-12
type: archived
---

# MCP configuration reference

Reference for MCP server configuration file format, commands, and settings in VS Code.

## Configuration file (`mcp.json`)

Stored in workspace (`.vscode/mcp.json`) or user profile. VS Code provides IntelliSense.

### Structure

```json
{
  "inputs": [],
  "servers": {}
}
```

### stdio servers

| Field | Required | Description |
|-------|----------|-------------|
| `type` | Yes | `"stdio"` |
| `command` | Yes | Command to start server (e.g., `"npx"`, `"node"`, `"python"`, `"docker"`) |
| `args` | No | Array of arguments |
| `env` | No | Environment variables |
| `envFile` | No | Path to `.env` file |
| `sandboxEnabled` | No | Sandbox on macOS/Linux |
| `sandbox` | No | Filesystem/network access rules |

### HTTP/SSE servers

| Field | Required | Description |
|-------|----------|-------------|
| `type` | Yes | `"http"` or `"sse"` |
| `url` | Yes | Server URL |
| `headers` | No | HTTP headers |

Unix sockets and Windows named pipes: `unix:///path/to/server.sock` or `pipe:///pipe/named-pipe`.

### Input variables (avoid hardcoded secrets)

```json
{
  "inputs": [{
    "type": "promptString",
    "id": "api-key",
    "description": "API Key",
    "password": true
  }],
  "servers": {
    "my-server": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "my-server"],
      "env": { "API_KEY": "${input:api-key}" }
    }
  }
}
```

### Sandbox configuration

```json
{
  "servers": {
    "myServer": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@example/mcp-server"],
      "sandboxEnabled": true,
      "sandbox": {
        "filesystem": {
          "allowWrite": ["${workspaceFolder}"],
          "denyRead": ["${userHome}/.ssh"]
        },
        "network": {
          "allowedDomains": ["api.example.com", "*.cdn.example.com"]
        }
      }
    }
  }
}
```

When sandboxing is enabled, tool calls are auto-approved.

### Development mode

```json
{
  "servers": {
    "myServer": {
      "dev": {
        "watch": "**/*.ts",
        "debug": true
      }
    }
  }
}
```

- `watch`: file glob to watch for restarts
- `debug`: enables Node.js/Python debugger attachment

### Naming conventions

- camelCase server names
- No whitespace or special characters
- Unique, descriptive names

## Commands

| Command | Description |
|---------|-------------|
| `MCP: Add Server` | Add a server to workspace or user profile |
| `MCP: Browse MCP Servers` | Open server gallery |
| `MCP: Browse Resources` | Browse server resources |
| `MCP: Install Server from Manifest` | Install from manifest file |
| `MCP: List Servers` | List all servers, start/stop/restart |
| `MCP: Open User Configuration` | Open user `mcp.json` |
| `MCP: Open Workspace Folder MCP Configuration` | Open workspace `mcp.json` |
| `MCP: Reset Cached Tools` | Clear cached tool list |
| `MCP: Reset Trust` | Reset trust decisions |

## Settings

| Setting | Description |
|---------|-------------|
| `chat.mcp.access` | Manage which MCP servers can be used |
| `chat.mcp.discovery.enabled` | Auto-discover config from other apps |
| `chat.mcp.autostart` | (Experimental) Auto-start on config change |
| `chat.mcp.serverSampling` | Configure models exposed for sampling |
| `chat.mcp.apps.enabled` | (Experimental) Enable MCP Apps |
