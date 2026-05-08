---
name: create-skill
description: Use this skill to create new Agent Skills with valid frontmatter and standard directory structure. Activate when the user needs to scaffold a skill, generate SKILL.md, or create a new agent capability — even if they don't explicitly say "create skill". Triggers: create skill, new skill, scaffold, generate skill, add capability.
license: "MIT"
metadata:
  version: "1.0"
  author: "skill-toolkit-team"
  tags: ["skill-creation", "scaffold", "generation"]
---

# Create Skill

Create a new Agent Skill with a valid `SKILL.md` and standard directory structure. See the specification in `references/specification.md` for all format rules.

## Use Cases

- Starting a new skill from scratch
- Scaffolding a skill directory with all required and recommended files
- Generating a valid `SKILL.md` with proper YAML frontmatter
- Setting up the progressive disclosure structure (L1/L2/L3)

## Not Suitable For

- Updating an existing skill (use update-skill)
- Validating an already-created skill (use validate-skill)
- Creating agent/subagent files (use create-agent)

## Workflow

### Step 1: Gather Input

Ask the user for or determine the following:

| Field | Required | Notes |
|-------|----------|-------|
| `name` | Yes | 1-64 chars, lowercase letters, numbers, hyphens only. Must not start/end with `-`. Must match parent directory name. |
| `description` | Yes | 1-1024 chars. Describe what the skill does and when to use it. Include trigger keywords. |
| `license` | No | e.g. "MIT", "Apache-2.0", or "Proprietary" |
| `metadata.version` | No | Semantic version string |
| `metadata.author` | No | Author or team identifier |
| `metadata.tags` | No | Comma-separated list of tags |

### Step 2: Validate Name

```
name: code-review         # valid
name: pdf-processing      # valid
name: PDF-Processing      # invalid: uppercase
name: -pdf                # invalid: starts with hyphen
name: pdf--processing     # invalid: consecutive hyphens
```

### Step 3: Generate Directory Structure

```
<skill-name>/
├── SKILL.md                  # Skill definition (frontmatter + instructions)
├── scripts/                  # Optional: executable code
├── references/               # Optional: reference documentation
├── assets/                   # Optional: templates, resources
└── adapters/                 # Optional: multi-platform adapters
    └── prompt/               # Prompt mode (default)
```

### Step 4: Generate SKILL.md

Write the SKILL.md with the gathered metadata. Required structure:

```markdown
---
name: <skill-name>
description: <description>    # imperative: "Use this skill when..."
license: <license>              # if provided
metadata:                       # if any metadata
  version: "1.0"
  author: "<author>"
---

# <Skill Name>

Summary of what this skill does.

## Use Cases
## Not Suitable For
## Workflow       # step-by-step instructions
## Output Format   # template if applicable
## Notes            # gotchas, limitations
```

Key rules:
- `description`: imperative phrasing, include trigger keywords
- Keep under 500 lines; move depth to `references/`
- See `references/example-invoice-extractor.md` for a complete example

### Step 5: Verify

Run validation after creation:

1. Check `name` matches the parent directory name
2. Verify YAML frontmatter is valid
3. Confirm `description` is non-empty and includes trigger keywords
4. Ensure all required sections are present

If validation fails, fix the issues and re-verify before proceeding.

## Output Format Example:

See `references/example-invoice-extractor.md` for a complete skill example.

## Gotchas

- The `name` field must match the parent directory name exactly. If you name the directory `my-skill`, the frontmatter `name` must also be `my-skill`.
- After creating a skill, always run validate-skill to catch frontmatter errors before committing.
- The `description` is the only field that determines agent activation — keep it focused on user intent, not implementation.
- SKILL.md files over 500 lines hurt activation reliability. Move depth to `references/` files.
- Directory names with uppercase letters will fail validation. Always use lowercase with hyphens.

## Workflow Checklist

- [ ] Step 1: Determine skill name, description, optional metadata
- [ ] Step 2: Validate name format (lowercase, hyphens, no consecutive `--`)
- [ ] Step 3: Create directory: `<name>/`
- [ ] Step 4: Generate `SKILL.md` with valid frontmatter and body
- [ ] Step 5: Create optional directories (scripts/, references/, assets/)
- [ ] Step 6: Verify name matches directory name
- [ ] Step 7: Open inline for human review
