---
source: https://code.visualstudio.com/docs/copilot/customization/prompt-files
retrieved: 2026-05-12
type: archived
---

# Use prompt files in VS Code

Prompt files (slash commands) encode common tasks as standalone Markdown files invoked via `/` in chat.

## When to use

| Use Case | Example |
|----------|---------|
| Simplify prompting | Scaffold component, run tests, prepare PR |
| Override agent behavior | Create minimal implementation plan, generate mockups |

## Locations

| Scope | Default location |
|-------|-----------------|
| Workspace | `.github/prompts/` |
| User profile | User data (via Agent Customizations editor) |

## Format

Files use `.prompt.md` extension with optional YAML frontmatter:

```markdown
---
name: review-api
description: Perform a REST API security review
agent: ask
model: Claude Sonnet 4
tools: ['search/codebase', 'vscode/askQuestions']
---
```

Frontmatter fields:

| Field | Description |
|-------|-------------|
| `name` | Slash command name (defaults to filename) |
| `description` | Shown in `/` menu |
| `argument-hint` | Hint text in chat input |
| `agent` | Which agent to use: `ask`, `agent`, `plan`, or custom agent name |
| `model` | Language model for this prompt |
| `tools` | Allowed tool list |

Reference tools in body with `#tool:<tool-name>` syntax. Use `${input:variableName}` to prompt user for input.

## Tips

- Use prompt files for **lightweight, single-task** prompts
- Custom agents for persistent personas with tool restrictions
- Agent Skills for portable multi-file capabilities with scripts
