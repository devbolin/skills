---
source: https://opencode.ai/docs/commands/
retrieved: 2026-05-12
type: archived
---

# Commands

Create custom commands for repetitive tasks.

Custom commands let you specify a prompt you want to run when that command is executed in the TUI.

```
/my-command
```

Custom commands are in addition to the built-in commands like `/init`, `/undo`, `/redo`, `/share`, `/help`.

---

## Create command files

Create markdown files in the `commands/` directory to define custom commands.

Create `.opencode/commands/test.md`:

```markdown
---
description: Run tests with coverage
agent: build
model: anthropic/claude-3-5-sonnet-20241022
---
Run the full test suite with coverage report and show any failures.
Focus on the failing tests and suggest fixes.
```

---

## Configure

You can add custom commands through the OpenCode config or by creating markdown files.

### JSON

Use the `command` option in your OpenCode config:

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "command": {
    "test": {
      "template": "Run the full test suite...",
      "description": "Run tests with coverage",
      "agent": "build",
      "model": "anthropic/claude-3-5-sonnet-20241022"
    }
  }
}
```

### Markdown

Place markdown files in `~/.config/opencode/commands/` or `.opencode/commands/`. The filename becomes the command name.

---

## Prompt config

### Arguments

Use `$ARGUMENTS`, `$1`, `$2`, etc. placeholders in your template.

### Shell output

Use `!`command`` to inject bash command output into your prompt.

### File references

Include files using `@` followed by the filename.

---

## Options

- **template** (required): The prompt sent to the LLM.
- **description**: Shown in the TUI.
- **agent**: Which agent should execute the command.
- **subtask**: Force subagent invocation.
- **model**: Override the default model.

---

## Built-in

OpenCode includes built-in commands like `/init`, `/undo`, `/redo`, `/share`, `/help`. Custom commands can override built-in ones.
