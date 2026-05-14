---
source: https://opencode.ai/docs/config/
retrieved: 2026-05-12
type: archived
---

# Config

Using the OpenCode JSON config.

You can configure OpenCode using a JSON config file. Supports both JSON and JSONC formats.

## Format

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "autoupdate": true,
  "server": {
    "port": 4096
  }
}
```

## Locations

Config files are merged together. Later configs override earlier ones only for conflicting keys.

**Precedence order**: Remote config → Global config → Custom config → Project config → `.opencode` directories → Inline config → Managed settings → macOS managed preferences

### Remote
Organizations can provide default configuration via the `.well-known/opencode` endpoint, fetched automatically when authenticated.

### Global
Place in `~/.config/opencode/opencode.json`. For TUI-specific settings, use `~/.config/opencode/tui.json`.

### Per project
Add `opencode.json` in your project root. Traverses up to nearest Git directory.

### Custom path
Use `OPENCODE_CONFIG` env var to specify a custom config file path.

### Custom directory
Use `OPENCODE_CONFIG_DIR` env var to specify a custom config directory.

### Managed settings
Organizations can enforce configuration via file-based managed config or macOS managed preferences (.mobileconfig via MDM).

## Schema

Server/runtime config schema at opencode.ai/config.json. TUI config at opencode.ai/tui.json.

### TUI
Dedicated `tui.json` for TUI-specific settings (scroll_speed, diff_style, mouse, etc.).

### Server
Configure server settings for `opencode serve` and `opencode web` commands (port, hostname, mdns, cors).

### Shell
Configure the shell used for interactive terminal (e.g., `"pwsh"`).

### Tools
Manage which tools an LLM can use (e.g., `{"write": false, "bash": false}`).

### Models
Configure providers and models via `provider`, `model` and `small_model` options.

### Themes
Set UI theme in `tui.json`.

### Agents
Configure specialized agents via `agent` option or markdown files in `~/.config/opencode/agents/`.

### Default agent
Set the default agent using `default_agent` option.

### Sharing
Configure the share feature: `"manual"`, `"auto"`, or `"disabled"`.

### Commands
Configure custom commands for repetitive tasks via `command` option or markdown files.

### Keybinds
Customize TUI keyboard shortcuts in `tui.json`.

### Snapshot
Track file changes for undo. Can be disabled.

### Autoupdate
Control automatic updates.

### Formatters
Enable and configure code formatters.

### LSP Servers
Enable and configure LSP servers.

### Permissions
Control which operations require approval: `"allow"`, `"ask"`, `"deny"`.

### Compaction
Control context compaction behavior (auto, prune, reserved tokens).

### Watcher
Configure file watcher ignore patterns.

### MCP servers
Configure MCP servers via `mcp` option.

### Plugins
Load plugins from npm via `plugin` option.

### Instructions
Configure instruction files via `instructions` option (array of paths and glob patterns).

### Disabled providers
Prevent certain providers from loading even if their credentials are available.

### Enabled providers
Specify an allowlist of providers.

### Experimental
Options under active development.

## Variables

Use `{env:VARIABLE_NAME}` for environment variables and `{file:path/to/file}` for file contents.
