---
source: https://opencode.ai/docs/mcp-servers/
retrieved: 2026-05-12
type: archived
---

# MCP servers

Add local and remote MCP tools.

You can add external tools to OpenCode using the Model Context Protocol (MCP). OpenCode supports both local and remote servers.

---

## Enable

Define MCP servers in your config under `mcp`:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "name-of-mcp-server": {
      "enabled": true
    }
  }
}
```

---

## Local

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "my-local-mcp-server": {
      "type": "local",
      "command": ["npx", "-y", "my-mcp-command"],
      "enabled": true,
      "environment": {
        "MY_ENV_VAR": "my_env_var_value"
      }
    }
  }
}
```

---

## Remote

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "my-remote-mcp": {
      "type": "remote",
      "url": "https://my-mcp-server.com",
      "enabled": true,
      "headers": {
        "Authorization": "Bearer MY_API_KEY"
      }
    }
  }
}
```

---

## OAuth

OpenCode automatically handles OAuth authentication for remote MCP servers. Supports automatic and pre-registered client flows.

---

## Manage

MCP tools can be enabled/disabled globally or per-agent using glob patterns:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": { ... },
  "tools": {
    "my-mcp*": false
  },
  "agent": {
    "my-agent": {
      "tools": {
        "my-mcp*": true
      }
    }
  }
}
```

---

## Examples

- **Sentry**: Remote MCP at `https://mcp.sentry.dev/mcp` with OAuth
- **Context7**: Remote MCP at `https://mcp.context7.com/mcp` for docs search
- **Grep by Vercel**: Remote MCP at `https://mcp.grep.app` for GitHub code search
