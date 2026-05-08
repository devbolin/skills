---
name: update-agent
description: Use this skill to update existing subagent declaration files — frontmatter, system prompt, boundaries, or output format. Activate when the user needs to revise an agent's behavior, adjust tools/permissions, or rename a subagent. Triggers: update agent, modify agent, edit agent, change agent, revise agent.
license: "MIT"
metadata:
  version: "1.0"
  author: "skill-toolkit-team"
  tags: ["agent-update", "maintenance", "modification"]
---

# Update Agent

Modify an existing subagent declaration file (`agents/<id>.md`) while maintaining consistency with the pack structure and frontmatter conventions.

## Use Cases

- Updating an agent's `description` to improve activation accuracy
- Revising the system prompt to refine behavior
- Adjusting delegation boundaries (input, output, prohibited actions, fallback)
- Adding or removing tools from the agent's tool list
- Changing the model assignment
- Renaming an agent (requires updating both file and pack.yaml)

## Not Suitable For

- Creating a brand new agent (use create-agent)
- Creating or updating skills (use create-skill or update-skill)
- Validating agent files without making changes

## Workflow

### Step 1: Locate the Target Agent

Determine the agent to update:

```bash
ls agents/<agent-id>.md
```

Read the current file:
```bash
cat agents/<agent-id>.md
```

Also check `pack.yaml` for the agent declaration:
```bash
grep -A2 "<agent-id>" pack.yaml
```

### Step 2: Determine Changes

| Change Type | Actions |
|-------------|---------|
| Frontmatter | Update `name`, `description`, `tools`, `model` |
| Role | Revise role description |
| When to Activate | Add/remove trigger scenarios |
| System Prompt | Update MUST/MUST NOT rules |
| Output Format | Change output structure or fields |
| Boundaries | Update input/output/fallback definitions |

### Step 3: Apply Changes

For frontmatter updates:
- Edit the YAML block between `---` markers
- Ensure tools listed are appropriate (principle of least privilege)
- Keep `description` focused on activation triggers

For body content:
- Keep the standard section structure (Role, When to Activate, System Prompt, Output Format)
- Add Delegation Boundaries section if not present

For renames:
- Change the `name` field in frontmatter
- Rename the file: `agents/<old-id>.md` → `agents/<new-id>.md`
- Update `pack.yaml` agents entry: `path: agents/<new-id>.md`



### Step 4: Validate

- Frontmatter is valid YAML
- `name` matches the file name (without extension)
- `description` is non-empty
- Tools listed are valid for the runtime
- If renamed, `pack.yaml` reflects the new path
- All sections are coherent and consistent

If validation fails, fix and re-validate before confirming.

### Step 5: Confirm

## Output Format Example

```markdown
## Update Summary

### Agent: reviewer

### Changes Made

| Field | Before | After |
|-------|--------|-------|
| description | Code reviewer that checks quality | Code reviewer that identifies issues, security risks, and provides constructive feedback |
| tools | Read, Glob, Grep | Read, Glob, Grep, Bash |
| System Prompt | Added MUST NOT section for deadline pressure | Added "Not rush reviews for deadline pressure" |

### Files Modified

- `agents/reviewer.md` - Updated description, tools, and system prompt
- `pack.yaml` - No changes needed (file name unchanged)

### Validation

- [x] Frontmatter YAML: valid
- [x] name matches file name: reviewer == reviewer.md
- [x] description: non-empty (115 chars)
- [x] pack.yaml entry: valid
```

## Gotchas

- Renaming an agent requires updating both the file name and `pack.yaml` — missing either breaks the agent.
- The `name` frontmatter field must match the file name (without `.md` extension).
- Keep tool assignments minimal. Adding unnecessary tools increases security risk and token usage.
- System Prompt MUST/MUST NOT sections should be specific and actionable. "MUST NOT rush reviews" is actionable; "MUST be good" is not.
- After updating, verify that `pack.yaml` still parses correctly and all paths are valid.

## Workflow Checklist

- [ ] Step 1: Locate target agent file and pack.yaml entry
- [ ] Step 2: Read current agent file
- [ ] Step 3: Apply changes (frontmatter, body, or rename)
- [ ] Step 4: If renamed, update both file and pack.yaml
- [ ] Step 5: Validate frontmatter YAML and name-file match
- [ ] Step 6: Confirm changes with user
