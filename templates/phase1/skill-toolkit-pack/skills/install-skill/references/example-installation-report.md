# Installation Report Example

Example output after a successful plugin installation.

```markdown
## Installation Report

### Source
- **Type**: Plugin artifact
- **Pack**: devtools-pack
- **Version**: 1.0.0
- **Source**: catalog/index.json -> releases/devtools-pack-1.0.0-plugin.zip

### Target
- **Directory**: /opt/skills/plugins/devtools-pack/1.0.0

### Installed Skills

| Skill ID | Path | Status |
|----------|------|--------|
| code-review | skills/code-review/SKILL.md | ✅ |
| pr-summary | skills/pr-summary/SKILL.md | ✅ |
| test-plan | skills/test-plan/SKILL.md | ✅ |

### Installed Agents

| Agent ID | Path | Status |
|----------|------|--------|
| review-coordinator | agents/review-coordinator.md | ✅ |

### Verification
- [x] pack.yaml exists
- [x] All skill entries have valid SKILL.md
- [x] All agent entries have valid .md files
```
