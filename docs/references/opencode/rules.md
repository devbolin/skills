---
source: https://opencode.ai/docs/rules/
retrieved: 2026-05-12
type: archived
---

# Rules

Set custom instructions for opencode.

Create `AGENTS.md` file in your project root. Run `/init` to generate one automatically.

## Types
- **Project**: `AGENTS.md` in project root
- **Global**: `~/.config/opencode/AGENTS.md`
- **Claude Code compat**: `CLAUDE.md` (fallback if no AGENTS.md)

## Precedence
Local files (traversing up) → Global → Claude Code file

## Custom Instructions
Specify instruction files in `opencode.json`:
```json
{
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md", ".cursor/rules/*.md"]
}
```

Supports glob patterns and remote URLs.

## Referencing External Files
Use `instructions` field in opencode.json for modular rules. Manual instructions in AGENTS.md with @file references.
