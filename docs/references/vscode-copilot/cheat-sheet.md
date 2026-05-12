---
source: https://code.visualstudio.com/docs/copilot/reference/copilot-vscode-features
retrieved: 2026-05-12
type: archived
---

# GitHub Copilot in VS Code cheat sheet

## Essential keyboard shortcuts

| Shortcut | Action |
|----------|--------|
| ⌃⌘I (Ctrl+Alt+I) | Open Chat view |
| ⌘I (Ctrl+I) | Start inline chat |
| ⌘N (Ctrl+N) | New chat session |
| ⇧⌘I (Ctrl+Shift+I) | Switch to agent mode |
| Tab | Accept suggestion / navigate to next edit |
| Escape | Dismiss suggestion |

## Access AI in VS Code

- **Chat view** (⌃⌘I): Ongoing conversations in the Secondary Side Bar
- **Inline chat** (⌘I): Chat in the editor or terminal
- **Quick Chat** (⇧⌥⌘L): Quick questions without leaving your task

## Chat experience

| Action | Description |
|--------|-------------|
| `⌃⌘I` | Open Chat view |
| `⌘I` | Start inline chat |
| `⌘N` | New chat session |
| `Add Context...` | Attach context to your prompt |
| `#-mention` | Reference tools and chat variables |
| `@-mention` | Reference chat participants |
| `/-command` | Use slash commands |

## Built-in chat tools

| Tool | Description |
|------|-------------|
| `#agent/runSubagent` | Delegate to a subagent |
| `#codebase` | Search the codebase (remote index) |
| `#file` | Reference a specific file |
| `#folder` | Reference a folder |
| `#git` | Reference git information |
| `#terminalLastCommand` | Reference last terminal command |
| `#vscodeAPI` | Reference VS Code API |

## Agents

- **Agent**: Autonomous planning and implementation across files
- **Plan**: Creates structured implementation plans
- **Ask**: Answers questions without making changes

## Editor AI features

- **Inline suggestions**: Ghost text as you type (Tab to accept)
- **Inline chat** (⌘I): Ask questions in the editor
- **Smart actions**: Generate commit messages, fix diagnostics, semantic search

## Source control

AI-powered commit message generation, PR title and description generation, and code review (experimental).
