---
source: https://code.visualstudio.com/docs/copilot/customization/mcp-servers
retrieved: 2026-05-12
type: archived
---

# Add and manage MCP servers in VS Code

Model Context Protocol (MCP) is an open standard for connecting AI models to external tools and services. MCP servers provide tools for file operations, databases, APIs, and more.

## Quickstart

1. Open Extensions view (⇧⌘X), search `@mcp playwright`
2. Install the Playwright MCP server
3. In chat, ask: "Go to code.visualstudio.com and take a screenshot"

## Configuration (mcp.json)

Two locations:
- **Workspace**: `.vscode/mcp.json` (share with team via source control)
- **User profile**: run `MCP: Open User Configuration`

### stdio servers

```json
{
  "servers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@microsoft/mcp-server-playwright"]
    }
  }
}
```

### HTTP/SSE servers

```json
{
  "servers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp"
    }
  }
}
```

### Input variables (avoid hardcoding secrets)

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "api-key",
      "description": "API Key",
      "password": true
    }
  ],
  "servers": {
    "my-server": {
      "command": "npx",
      "args": ["-y", "my-server"],
      "env": { "API_KEY": "${input:api-key}" }
    }
  }
}
```

### Sandbox (macOS/Linux)

Restrict server file system and network access:

```json
{
  "servers": {
    "myServer": {
      "command": "npx",
      "args": ["-y", "@example/mcp-server"],
      "sandboxEnabled": true,
      "sandbox": {
        "filesystem": {
          "allowWrite": ["${workspaceFolder}"],
          "denyRead": ["${userHome}/.ssh"]
        },
        "network": {
          "allowedDomains": ["api.example.com"]
        }
      }
    }
  }
}
```

## Dev Containers

Configure MCP servers in `devcontainer.json`:

```json
{
  "customizations": {
    "vscode": {
      "mcp": {
        "servers": {
          "playwright": {
            "command": "npx",
            "args": ["-y", "@microsoft/mcp-server-playwright"]
          }
        }
      }
    }
  }
}
```

## Auto-discovery

VS Code can automatically detect and reuse MCP server configurations from other applications (e.g., Claude Desktop). Enable with `chat.mcp.discovery.enabled` setting. Select one or more tools from which to discover their MCP configuration.

## Add MCP server from command line

Use the `--add-mcp` CLI option to add a server to your user profile:

```bash
code --add-mcp "{\"name\":\"my-server\",\"command\": \"uvx\",\"args\": [\"mcp-server-fetch\"]}"
```

## Management

| Method | Description |
|--------|-------------|
| Extensions view | Right-click server in MCP SERVERS section |
| Command Palette | `MCP: List Servers`, `MCP: Add Server` |
| Settings | `chat.mcp.access`, `chat.mcp.discovery.enabled`, `chat.mcp.autoStart` |

## Trust

VS Code shows a trust dialog when starting a new MCP server. Servers can run arbitrary code — only add from trusted sources.
