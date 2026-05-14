---
source: https://opencode.ai/docs/acp/
retrieved: 2026-05-12
type: archived
---

# ACP Support

Use OpenCode in any ACP-compatible editor.

OpenCode supports the Agent Client Protocol (ACP), allowing you to use it directly in compatible editors and IDEs.

---

## Configure

To use OpenCode via ACP, configure your editor to run the `opencode acp` command.

### Zed

Add to `~/.config/zed/settings.json`:

```json
{
  "agent_servers": {
    "OpenCode": {
      "command": "opencode",
      "args": ["acp"]
    }
  }
}
```

### JetBrains IDEs

Add to `acp.json`:

```json
{
  "agent_servers": {
    "OpenCode": {
      "command": "/absolute/path/bin/opencode",
      "args": ["acp"]
    }
  }
}
```

### Avante.nvim

```lua
acp_providers = {
  ["opencode"] = {
    command = "opencode",
    args = { "acp" }
  }
}
```

### CodeCompanion.nvim

```lua
require("codecompanion").setup({
  interactions = {
    chat = {
      adapter = {
        name = "opencode",
        model = "claude-sonnet-4",
      },
    },
  },
})
```

---

## Support

All OpenCode features work via ACP (built-in tools, custom tools, MCP servers, rules, formatters, agents, permissions). Some built-in slash commands like `/undo` and `/redo` are currently unsupported.
