---
source: https://opencode.ai/docs/agents/
retrieved: 2026-05-12
type: archived
---

# Agents

Configure and use specialized agents.

## Types
- **Primary agents**: Build (default, all tools), Plan (restricted, planning only)
- **Subagents**: General (multi-step tasks), Explore (read-only codebase search), Scout (external docs research)
- **System agents**: Compaction, Title, Summary (hidden, automatic)

## Built-in agents
Build, Plan, General, Explore, Scout, Compaction, Title, Summary.

## Configure
Two ways: JSON in `opencode.json` or Markdown files in `~/.config/opencode/agents/`.

## Options
description, temperature, max steps, disable, prompt, model, permissions, mode (primary/subagent/all), hidden, task permissions, color, top_p, additional (provider-specific).

## Create agents
`opencode agent create` command with interactive setup.

## Examples
Documentation agent, Security auditor, Code reviewer.
