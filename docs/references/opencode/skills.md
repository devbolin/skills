---
source: https://opencode.ai/docs/skills/
retrieved: 2026-05-12
type: archived
---

# Agent Skills

Define reusable behavior via SKILL.md definitions.

## Place files
Create one folder per skill name with SKILL.md inside. Locations:
- `.opencode/skills/<name>/SKILL.md`
- `~/.config/opencode/skills/<name>/SKILL.md`
- `.claude/skills/<name>/SKILL.md` (compat)
- `~/.claude/skills/<name>/SKILL.md` (compat)
- `.agents/skills/<name>/SKILL.md` (compat)
- `~/.agents/skills/<name>/SKILL.md` (compat)

## Write frontmatter
Required: name (1-64 chars, lowercase alphanumeric with hyphens), description (1-1024 chars). Optional: license, compatibility, metadata.

## Configure permissions
Pattern-based in opencode.json: `{"permission": {"skill": {"*": "allow", "internal-*": "deny"}}}`

## Override per agent
In agent frontmatter or in opencode.json under agent config.

## Disable skill tool
`"tools": {"skill": false}` in agent config.
