---
source: https://code.visualstudio.com/docs/copilot/customization/custom-agents
retrieved: 2026-05-12
type: archived
---

# Custom agents in VS Code

Custom agents configure the AI to adopt different personas tailored to specific development roles and tasks. Each persona can have its own behavior, available tools, and instructions.

## File format

Custom agents are defined in `.agent.md` files with YAML frontmatter:

```markdown
---
name: security-reviewer
description: Review code for security vulnerabilities
tools: ['search/codebase', 'search/web']
model: GPT-5
handoffs:
  - label: Start Implementation
    agent: implementation
    prompt: Now implement the security fixes outlined above.
    send: false
---
```

Frontmatter fields:

| Field | Description |
|-------|-------------|
| `name` | Agent name (shown in dropdown) |
| `description` | Shown as placeholder in chat input |
| `tools` | List of available tools/tool sets |
| `model` | Language model preference |
| `handoffs` | Guided workflow transitions to other agents |

## Locations

| Scope | Default location |
|-------|-----------------|
| Workspace | `.github/agents/` |
| Workspace (Claude) | `.claude/agents/` |
| User profile | `~/.copilot/agents/` |

## agentify — auto-generated agent IDs

When VS Code automatically generates a custom agent (via `/create-agent` or "Generate Agent" in the editor), it uses the **agentify** process:

- The agent's `name` is derived from the file name by stripping the `.agent.md` extension
- If no `name` field is provided in frontmatter, the file name (without extension) is used as the display name
- IDs are automatically kebab-cased from the file name
- Agent files in `.github/agents/` are detected by their `.md` extension — no explicit registration needed

## Handoffs

Handoffs enable guided sequential workflows between agents. Each handoff specifies:
- **Target agent**: which agent to switch to
- **Button label**: displayed to user
- **Prompt**: pre-filled message (auto-send if `send: true`)
- **Model**: optional model override for target

Example workflow: Plan → Implementation → Code Review

## When to use

- Custom agents for **persistent persona** with tool/model restrictions
- Prompt files for **one-off tasks** without tool restrictions
- Agent Skills for **portable capabilities** with scripts and resources
