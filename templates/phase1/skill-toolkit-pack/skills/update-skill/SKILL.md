---
name: update-skill
description: Use this skill to update existing Agent Skills — frontmatter, body content, or resource files — while maintaining spec compliance. Activate when the user needs to modify a skill, revise instructions, bump version, or restructure skill content. Triggers: update skill, modify skill, edit skill, change skill, revise skill.
license: "MIT"
metadata:
  version: "1.0"
  author: "skill-toolkit-team"
  tags: ["skill-update", "maintenance", "modification"]
---

# Update Skill

Modify an existing skill's `SKILL.md` or its supporting files while ensuring continued compliance with the Agent Skills specification (see `references/specification.md`).

## Use Cases

- Updating a skill's `description` to improve agent activation accuracy
- Adding or removing use cases and boundaries
- Revising step-by-step instructions
- Updating metadata (version, author, tags, license)
- Adding or updating resource files (scripts, references, assets)
- Renaming a skill (updating directory name + frontmatter name)

## Not Suitable For

- Creating a brand new skill (use create-skill)
- Validating a skill without making changes (use validate-skill)
- Updating agent/subagent files (use update-agent)

## Workflow

### Step 1: Locate the Target Skill

Determine the skill to update:
- If a path is provided, use it directly
- If only a skill name is given, search for matching `skills/<id>/SKILL.md` in the current pack

Confirm:
```bash
ls <skill-path>/SKILL.md
```

### Step 2: Read Existing Content

Read the full `SKILL.md` to understand the current state:
```bash
cat <skill-path>/SKILL.md
```

### Step 3: Determine Changes

| Change Type | Actions |
|-------------|---------|
| Frontmatter | Update `name`, `description`, `license`, `metadata` (version/author/tags) |
| Body content | Add/remove/modify use cases, usage steps, notes |
| Resources | Add/modify/remove files in `scripts/`, `references/`, `assets/` |
| Reorganization | Restructure sections, split content into referenced files |

### Step 4: Apply Changes

For frontmatter updates:
- Edit the YAML block between `---` markers
- Ensure `name` continues to match the parent directory
- Validate updated `description` satisfies spec constraints (1-1024 chars)
- If `description` contains colons, use YAML block scalar or quoting

For body content:
- Keep the Progressive Disclosure principle in mind
- Move detailed reference material to `references/` files
- Keep the main `SKILL.md` under 500 lines

### Step 5: Validate After Update

Run validation checks before confirming:

1. `name` field is valid (1-64 chars, lowercase, hyphens, no consecutive `--`)
2. `description` is non-empty and under 1024 characters
3. YAML frontmatter parses correctly
4. `name` matches the parent directory name
5. All cross-references are valid

If validation fails, fix the issues and re-validate before proceeding.

### Step 6: Confirm

Summarize what was changed and ask for confirmation.

### Step 7: Re-validate (Optional)

For extra safety, run online validation:
```bash
skills-ref validate <skill-path>
```

## Output Format Example

```markdown
## Update Summary

### Skill: code-review

### Changes Made

| Field | Before | After |
|-------|--------|-------|
| description | Code review for quality | Code review for quality, security, and performance |
| metadata.version | "1.0" | "1.1" |
| metadata.tags | ["code-review"] | ["code-review", "security", "quality"] |

### Files Modified

- `skills/code-review/SKILL.md` - Updated frontmatter and added security review section

### Validation

- [x] name valid: code-review
- [x] description valid: 86 chars (max 1024)
- [x] YAML frontmatter: valid
- [x] name matches parent directory: OK
```

## Gotchas

- Updating the `name` field requires renaming the parent directory. Update both or validation will fail.
- Always use validate-skill after update. Even small frontmatter changes can introduce YAML errors.
- `description` with colons needs quoting or block scalar format in YAML.
- SKILL.md under 500 lines; file too large causes activation unreliability.

## Workflow Checklist

- [ ] Step 1: Locate target skill
- [ ] Step 2: Read current SKILL.md
- [ ] Step 3: Apply changes (frontmatter, body, or resources)
- [ ] Step 4: Validate name format and directory match
- [ ] Step 5: Validate YAML frontmatter parses correctly
- [ ] Step 6: Confirm changes with user
