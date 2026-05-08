---
name: validate-skill
description: Use this skill to validate Agent Skill format against the specification, checking frontmatter, directory structure, and naming conventions. Activate when the user needs to check skill compliance, inspect a SKILL.md, or verify a skill before committing — even if they just say "check this skill". Triggers: validate skill, check skill, verify skill, inspect skill, skill lint.
license: "MIT"
metadata:
  version: "1.0"
  author: "skill-toolkit-team"
  tags: ["validation", "lint", "compliance", "quality-check"]
compatibility: "Optional online validation requires network access and `skills-ref` CLI tool."
---

# Validate Skill

Check that a skill conforms to the Agent Skills specification (see `references/specification.md`), identifying issues and suggesting fixes.

## Use Cases

- Verifying a newly created skill before committing
- Checking an existing skill for spec compliance
- Identifying frontmatter errors (missing required fields, invalid formats)
- Ensuring `name` matches parent directory name
- Running automated quality gates in CI pipelines

## Not Suitable For

- Creating or updating skills (use create-skill or update-skill)
- Testing skill runtime behavior or output quality
- Validating agent files (validation is specific to SKILL.md format)

## Workflow

### Step 1: Locate Target Skill

Determine the skill to validate:
```bash
ls <skill-path>/SKILL.md
```

### Step 2: Run Local Validation

Check each item in order:

### Step 2: Run Local Validation

Check each item (agent knows how to verify these):
- SKILL.md exists
- YAML frontmatter is valid (no duplicate keys)
- `name`: 1-64 chars, lowercase, hyphens only, no consecutive `--`
- `description`: 1-1024 chars, non-empty
- `name` matches parent directory name
- Directory structure: SKILL.md required, scripts/ references/ optional

### Step 3: Run Online Validation (Optional)

If network access is available and `skills-ref` is installed:

```bash
skills-ref validate <skill-path>
```

Install `skills-ref`:
```bash
npm install -g @agentskills/skills-ref
```

### Step 4: Report Results

Output a structured validation report.

## Output Format Example

```markdown
## Validation Report

### Skill: /path/to/code-review

### Local Checks

| Check | Result | Details |
|-------|--------|---------|
| SKILL.md exists | ✅ | Found |
| YAML frontmatter | ✅ | Valid |
| name field | ✅ | "code-review" (10 chars, valid format) |
| name matches directory | ✅ | "code-review" == "code-review" |
| description field | ✅ | Present (120 chars, max 1024) |
| description non-empty | ✅ | "Code review for quality, security..." |

### Online Validation (optional)

| Check | Result |
|-------|--------|
| skills-ref validate | ⏭️ Skipped (no network) / ✅ Passed / ❌ Failed |

### Summary

- **Status**: ✅ PASS
- **Errors**: 0
- **Warnings**: 0
- **Suggestions**: 1 (consider adding `license` field)
```

### Error Examples

| Error | Suggestion |
|-------|------------|
| `name: Code-Review` (uppercase) | Change to `name: code-review` |
| `name` length 0 or > 64 chars | Adjust to between 1-64 characters |
| `name: -code-review` (leading hyphen) | Remove leading hyphen: `code-review` |
| `description` missing | Add a description (1-1024 chars) |
| `description` > 1024 chars | Shorten description to under 1024 characters |
| Directory name mismatch | Rename directory or fix `name` field |
| YAML parse error | Check for syntax issues in frontmatter |

## Gotchas

- The `name` must match the parent directory name. If the directory is `my-skill/`, `name: my-skill`. This is the most common validation failure.
- `description` with colons (e.g., "Use when: the user asks...") is technically invalid YAML. Prefer block scalars or quoting.
- Online validation with `skills-ref` is the authoritative check. Local checks catch most issues but may miss edge cases.
- `compatibility` field is rarely needed. Skip it unless the skill has specific environment requirements.
- `metadata` keys like `name` or `description` could conflict with future spec fields. Use unique key names.

## Workflow Checklist

- [ ] Step 1: Locate target skill directory
- [ ] Step 2: Check SKILL.md exists
- [ ] Step 3: Parse and validate YAML frontmatter
- [ ] Step 4: Check `name` format (1-64 chars, lowercase, hyphens)
- [ ] Step 5: Verify `name` matches parent directory
- [ ] Step 6: Check `description` (1-1024 chars, non-empty)
- [ ] Step 7: Report structured results with PASS/FAIL for each check
