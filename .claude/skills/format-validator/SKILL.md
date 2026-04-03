---
name: format-validator
description: Validate document format against standards. Use when user says "check format", "validate format", or "是否符合规范".
---

# Format Validator

Validates documents conform to project standards.

## Standards Reference

### File Naming
- Use kebab-case: `SKILL_AUTHORING.md`
- No spaces or underscores in filenames

### Markdown Format
- Code blocks with language specified
- Tables properly formatted
- Headers use `#` not `===` or `---`

### YAML Frontmatter
```yaml
---
name: skill-name
description: Clear description
---
```

## Validation Checklist

- [ ] Filename uses kebab-case
- [ ] Markdown syntax correct
- [ ] Code blocks have language tags
- [ ] Tables properly formatted
- [ ] YAML frontmatter valid (if present)
- [ ] No trailing whitespace
- [ ] Line endings consistent

## Output Format

```markdown
## Format Validation Report

### File: <filename>

| Check | Status |
|-------|--------|
| kebab-case | ✅ Pass |
| markdown-syntax | ❌ Fail |
| code-blocks | ⚠️ Warning |

### Issues
1. Line 15: Missing language tag on code block
2. Line 42: Malformed table
```
