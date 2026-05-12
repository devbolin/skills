---
source: https://code.visualstudio.com/docs/copilot/customization/overview
retrieved: 2026-05-12
type: archived
---

# Customize AI in VS Code

VS Code provides several ways to teach the AI about your codebase, coding standards, and workflows.

## Customization scenarios

| Scenario | Solution |
|----------|----------|
| Define coding standards | Custom instructions (`copilot-instructions.md`, `*.instructions.md`) |
| Automate repetitive tasks | Prompt files (`.prompt.md` slash commands) |
| Complex multi-step workflows | Agent Skills (with scripts and resources) |
| Specialized AI personas | Custom Agents (`.agent.md`) |
| Connect external tools | MCP servers + Hooks |
| Pre-packaged bundles | Agent Plugins |

## Get started incrementally

1. **Initialize**: type `/init` in chat to generate `.github/copilot-instructions.md`
2. **Add targeted rules**: create `*.instructions.md` for specific file types
3. **Automate tasks**: create prompt files and add MCP servers
4. **Create workflows**: build custom agents and package reusable capabilities as skills

Generate customizations with AI: `/create-prompt`, `/create-instruction`, `/create-skill`, `/create-agent`, `/create-hook`.

## Agent Customizations editor (Preview)

Centralized UI for creating and managing all customizations. Open via **Configure Chat** (gear icon) in the Chat view, or run `Chat: Open Customizations` from the Command Palette.

## Parent repository discovery (monorepo)

Enable `chat.useCustomizationsInParentRepositories` to discover customizations from parent repository folders when working in subdirectories.

## Customization types

| Type | File format | Location | Scope |
|------|-------------|----------|-------|
| Custom Instructions | `.github/copilot-instructions.md`, `*.instructions.md`, `AGENTS.md`, `CLAUDE.md` | `.github/`, `.github/instructions/`, `.claude/rules/` | Always-on or file-based |
| Prompt Files | `.prompt.md` | `.github/prompts/` | Manual invocation via `/` |
| Custom Agents | `.agent.md` | `.github/agents/`, `.claude/agents/` | Agent persona switch |
| Agent Skills | `SKILL.md` + scripts | `.github/skills/`, `.claude/skills/`, `.agents/skills/` | On-demand loading |
| MCP Servers | `mcp.json` | `.vscode/mcp.json`, user profile | Tool extension |
| Hooks | `hooks.json` | `.github/hooks/`, `.claude/settings.json` | Lifecycle automation |
| Plugins | `plugin.json` | Marketplace or local | Bundled customizations |
