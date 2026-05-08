---
name: create-agent
description: Use this skill to create new subagent declaration files with proper frontmatter, role definition, system prompt, and output format. Activate when the user needs to add a subagent, scaffold an agent file, or define a new specialized agent role. Triggers: create agent, new agent, add agent, create subagent, scaffold agent.
license: "MIT"
metadata:
  version: "1.0"
  author: "skill-toolkit-team"
  tags: "agent-creation, subagent, scaffold, generation"
---

# Create Agent

Create a new subagent declaration file (`agents/<id>.md`) with proper frontmatter, role, boundary definitions, and structured output format.

## Use Cases

- Starting a new subagent from scratch
- Scaffolding an agent file with all required sections
- Generating a valid frontmatter block with `name`, `description`, `tools`, `model`
- Setting up delegation boundaries (input, output, prohibited actions, fallback)

## Not Suitable For

- Updating an existing agent (use update-agent)
- Creating a skill SKILL.md (use create-skill)
- Installing agents from a catalog (use install-skill)

## Workflow

### Step 1: Gather Input

Ask the user for or determine the following:

| Field | Required | Notes |
|-------|----------|-------|
| `name` | Yes | Agent ID, lowercase, hyphens allowed |
| `description` | Yes | What triggers this agent, non-empty |
| `tools` | No | Comma-separated tool list (e.g. `Read, Glob, Grep, Bash`) |
| `model` | No | Model selection (e.g. `inherit`, `claude-sonnet-4-20250514`) |
| Role | Yes | Brief role description |
| When to Activate | Yes | List of trigger scenarios |
| System Prompt | Yes | Core instructions including MUST/MUST NOT |
| Output Format | Yes | Structured output template |

### Step 2: Validate Input

- `name`: lowercase, letters, numbers, hyphens only
- `description`: non-empty, describes when to activate
- `tools`: valid tool names for the target runtime

### Step 3: Generate Agent File

Create `agents/<id>.md` with the standard structure:

```markdown
---
name: <agent-name>
description: <description>
tools: <tool-list>
model: <model>
---

# <Agent Name>

## Role
## When to Activate
## System Prompt
## Output Format
## Delegation Boundaries
```

Key rules:
- `name`: file name (without `.md`) must match frontmatter `name`
- `description`: imperative phrasing, trigger keywords
- Keep sections focused; move depth to `references/`

See `references/example-code-reviewer.md` for a complete example.

### Step 4: Register in pack.yaml

Add the agent entry to `pack.yaml`:

```yaml
agents:
  - id: <agent-name>
    path: agents/<agent-name>.md
```

### Step 5: Verify

Before finishing:

1. Verify frontmatter is valid YAML
2. Confirm all recommended sections are present
3. Check tool list contains valid tools for the target runtime
4. Confirm agent file exists at the path declared in `pack.yaml`
5. If anything fails, fix and re-verify

## Output Format Example:

See `references/example-code-reviewer.md` for a complete agent example.

## Gotchas

- The agent file name (without `.md` extension) must match the `name` field in frontmatter. `name: code-reviewer` means file must be `agents/code-reviewer.md`.
- After creating the agent file, always register it in `pack.yaml` under `agents:` or the agent won't be discoverable.
- Tools should follow the principle of least privilege. Don't give Write/Edit tools to read-only agents.
- `model: inherit` uses the parent agent's model. Specify a model only if this agent needs different capabilities.
- The `description` field is what triggers the agent — keep it focused on when to activate, not what the agent does internally.

## Workflow Checklist

- [ ] Step 1: Determine agent name, description, tools, model
- [ ] Step 2: Validate name format (lowercase, hyphens only)
- [ ] Step 3: Create `agents/<name>.md` with all sections
- [ ] Step 4: Add entry in `pack.yaml` under `agents:`
- [ ] Step 5: Verify frontmatter YAML is valid
- [ ] Step 6: Verify file name matches frontmatter name
