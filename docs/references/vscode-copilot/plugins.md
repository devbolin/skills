---
source: https://code.visualstudio.com/docs/copilot/customization/agent-plugins
retrieved: 2026-05-12
type: archived
---

# Agent plugins in VS Code (Preview)

Agent plugins are prepackaged bundles of agent customizations that you can discover and install from plugin marketplaces. A single plugin can provide slash commands, agent skills, custom agents, hooks, and MCP servers.

> **Marketplace requirement**: Plugins published to a marketplace must include `"tags": ["copilot-plugin"]` in their manifest for discovery.

## What plugins provide

- **Slash commands**: invoke with `/` in chat
- **Skills**: agent skills with instructions, scripts, and resources
- **Agents**: custom agents with specialized personas
- **Hooks**: shell commands at agent lifecycle points
- **MCP servers**: external tool integrations

## Plugin manifest (plugin.json)

```json
{
  "name": "my-dev-tools",
  "description": "React development utilities",
  "version": "1.2.0",
  "author": { "name": "Jane Doe" },
  "skills": "skills/",
  "agents": "agents/",
  "hooks": "hooks.json",
  "mcpServers": ".mcp.json"
}
```

Required: `name` (kebab-case, max 64 chars, no slashes/colons/namespace prefixes).

Optional fields: `description` (max 1024), `version` (semver), `author` (`name` + `email` + `url`), `skills`, `agents`, `hooks`, `mcpServers`.

## Plugin types

VS Code supports multiple plugin architectures:

| Type | Description |
|------|-------------|
| **REST plugin** | Provides tools via HTTP API endpoints with authentication |
| **Command-line plugin** | Bundles shell commands, MCP servers, and hooks |
| **MCP-enhanced plugin** | Extends agents with MCP server tools |

### REST plugin endpoint structure

REST plugins define HTTP endpoints with auth in their configuration:

```json
{
  "endpoints": {
    "search-code": {
      "url": "https://api.example.com/search",
      "auth": {
        "type": "bearer",
        "token": "${input:api-key}"
      }
    }
  }
}
```

### manifest.json (legacy extension format)

```json
{
  "schema_version": "3.1",
  "api_version": "1.0.0",
  "plugin": {
    "name": "my-tools",
    "version": "1.0.0"
  },
  "interface": {
    "provider": "my-tools",
    "consumer": "copilot"
  }
}
```

### OpenPlugin format vs VS Code format

| Aspect | OpenPlugin | VS Code |
|--------|-----------|---------|
| Manifest path | `.plugin/plugin.json` | `plugin.json` (root) or `.github/plugin/plugin.json` |
| Hooks path | `hooks/hooks.json` | `hooks.json` (root) |
| Plugin root token | `${PLUGIN_ROOT}` | (not defined for Copilot format) |
| Claude format | — | `${CLAUDE_PLUGIN_ROOT}` |

## Directory structure

```
my-plugin/
  plugin.json           # Metadata and configuration
  skills/
    my-skill/SKILL.md   # Skill instructions
  agents/
    reviewer.agent.md   # Custom agent
  hooks/
    hooks.json          # Hook configuration
  scripts/
    format.sh           # Hook script
  .mcp.json             # MCP server definitions
```

## Plugin formats

VS Code auto-detects:
- **Copilot** (default): `plugin.json` at root, `hooks.json`
- **Claude**: `.claude-plugin/plugin.json`, `hooks/hooks.json`
- **OpenPlugin**: `.plugin/plugin.json`

## Hooks in plugins

Plugins can include hooks. File location depends on format:

| Format | Hook path |
|--------|-----------|
| Claude | `hooks/hooks.json` |
| Copilot | `hooks.json` (plugin root) |

Use `${CLAUDE_PLUGIN_ROOT}` to reference scripts within the plugin directory (Claude format only).

## MCP servers in plugins

Place `.mcp.json` at plugin root. The top-level key is `mcpServers` (not `servers`):

```json
{
  "mcpServers": {
    "plugin-database": {
      "command": "${CLAUDE_PLUGIN_ROOT}/servers/db-server",
      "args": ["--config", "${CLAUDE_PLUGIN_ROOT}/config.json"]
    }
  }
}
```

Plugin MCP servers are implicitly trusted when the plugin is installed.

## Discover and install

1. Enable `chat.plugins.enabled` setting
2. Browse marketplaces from the Extensions view (`@agentPlugins`)
3. Install from Extensions view, marketplace URLs, or **Chat: Install Plugin From Source**

## Configure marketplaces

Default marketplaces: `github/copilot-plugins`, `github/awesome-copilot`. Add more:

```json
"chat.plugins.marketplaces": ["anthropics/claude-code"]
```

## Local plugins

Register local plugins via `chat.pluginLocations`:

```json
"chat.pluginLocations": { "/path/to/my-plugin": true }
```

## Cross-tool compatibility

Plugin format shared between VS Code, Copilot CLI, and Claude Code. VS Code checks manifest locations in order:
1. `.plugin/plugin.json`
2. `plugin.json` (root)
3. `.github/plugin/plugin.json`
4. `.claude-plugin/plugin.json`

## Security

Plugins can include hooks and MCP servers that run code on your machine. Review plugin contents and publisher before installing.
