---
source: https://opencode.ai/docs/plugins/
retrieved: 2026-05-12
type: archived
---

# Plugins

Write your own plugins to extend OpenCode.

## Use a plugin
From local files (`.opencode/plugins/` or `~/.config/opencode/plugins/`) or from npm via `plugin` config option.

## Create a plugin
JavaScript/TypeScript module exporting plugin functions. Each receives a context object and returns a hooks object.

## Events
Command events, File events, Installation events, LSP events, Message events, Permission events, Server events, Session events, Todo events, Shell events, Tool events, TUI events.

## Examples
Send notifications, .env protection, Inject environment variables, Custom tools, Logging, Compaction hooks.
