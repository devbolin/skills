---
source: https://opencode.ai/docs/
retrieved: 2026-05-12
type: archived
---

# Intro

Get started with OpenCode.

[**OpenCode**](/) is an open source AI coding agent. It's available as a terminal-based interface, desktop app, or IDE extension.

## Prerequisites

1. A modern terminal emulator (WezTerm, Alacritty, Ghostty, Kitty)
2. API keys for the LLM providers you want to use.

## Install

The easiest way to install OpenCode is through the install script:

```bash
curl -fsSL https://opencode.ai/install | bash
```

Also available via npm, Homebrew, Chocolatey, Scoop, and Docker.

## Configure

With OpenCode you can use any LLM provider by configuring their API keys. Run `/connect` in the TUI, select opencode, and head to opencode.ai/auth.

## Initialize

Run `opencode` in your project directory, then run `/init` to create an `AGENTS.md` file.

## Usage

- **Ask questions**: "How is authentication handled in @file.ts"
- **Add features**: Use Plan mode (Tab key) first, then Build mode
- **Make changes**: "We need to add authentication to the /settings route"
- **Undo changes**: Use `/undo` command

## Share

Run `/share` to create a shareable link to the current conversation.

## Customize

Pick a theme, customize keybinds, configure formatters, create custom commands.
