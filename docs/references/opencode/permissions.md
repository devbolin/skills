---
source: https://opencode.ai/docs/permissions/
retrieved: 2026-05-12
type: archived
---

# Permissions

Control which actions require approval to run.

OpenCode uses the `permission` config to decide whether a given action should run automatically, prompt you, or be blocked.

---

## Actions

Each permission rule resolves to one of: `"allow"`, `"ask"`, `"deny"`.

---

## Configuration

Set permissions globally with `*` and override specific tools:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "*": "ask",
    "bash": "allow",
    "edit": "deny"
  }
}
```

---

## Granular Rules (Object Syntax)

Use an object to apply different actions based on tool input:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "bash": {
      "*": "ask",
      "git *": "allow",
      "npm *": "allow",
      "rm *": "deny",
      "grep *": "allow"
    }
  }
}
```

### Wildcards

- `*` matches zero or more of any character
- `?` matches exactly one character

### External Directories

Use `external_directory` to allow tool calls that touch paths outside the working directory.

---

## Available Permissions

- `read`, `edit` (covers edit/write/patch), `glob`, `grep`, `bash`, `task`, `skill`, `lsp`, `question`, `webfetch`, `websearch`, `external_directory`, `doom_loop`

---

## Defaults

Most permissions default to `"allow"`. `doom_loop` and `external_directory` default to `"ask"`. `.env` files are denied by default for `read`.

---

## Agents

You can override permissions per agent. Agent permissions are merged with the global config, and agent rules take precedence.
