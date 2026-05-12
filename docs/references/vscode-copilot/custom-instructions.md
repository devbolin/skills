---
source: https://code.visualstudio.com/docs/copilot/customization/custom-instructions
retrieved: 2026-05-12
type: archived
---

# Use custom instructions in VS Code

Custom instructions define common guidelines that automatically influence how AI generates code. They apply to all chat requests or specific files only.

## Types of instruction files

### Always-on instructions

| File | Description |
|------|-------------|
| `.github/copilot-instructions.md` | Project-wide coding standards, applied to all chat |
| `AGENTS.md` | Multi-agent workspace instructions |
| `CLAUDE.md` | Claude Code compatibility |
| Organization-level | GitHub org-level shared instructions |

### File-based instructions

`*.instructions.md` files with `applyTo` glob patterns, conditionally applied based on file type or task matching.

## Instruction locations

| Scope | Default location |
|-------|-----------------|
| Workspace | `.github/copilot-instructions.md`, `.github/instructions/` |
| Workspace (Claude) | `.claude/rules/` |
| User profile | `~/.copilot/instructions/` |

## Example

```markdown
---
applyTo: "**"
---
# Coding Standards

## Naming
- PascalCase for components, interfaces, type aliases
- camelCase for variables, functions, methods
- ALL_CAPS for constants

## Error Handling
- Use try/catch for async operations
- Implement error boundaries in React components
```

## Instruction priority

When multiple types of custom instructions exist, they are all provided to the AI. Higher-priority instructions take precedence when conflicts occur:

1. **Personal instructions** (user-level, highest priority)
2. **Repository instructions** (`.github/copilot-instructions.md` or `AGENTS.md`)
3. **Organization instructions** (lowest priority)

## Tips

- Start with a single `.github/copilot-instructions.md` file
- Add `*.instructions.md` files for different file types or frameworks
- Use `AGENTS.md` when working with multiple AI agents
- Instructions do NOT apply to inline suggestions (ghost text)
