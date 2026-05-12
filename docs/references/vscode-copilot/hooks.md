---
source: https://code.visualstudio.com/docs/copilot/customization/hooks
retrieved: 2026-05-12
type: archived
---

# Agent hooks in VS Code (Preview)

Hooks execute custom shell commands at key lifecycle points during agent sessions. Use them to automate workflows, enforce security, and integrate with external tools.

## Hook lifecycle events

| Event | When it fires | Common uses |
|-------|---------------|-------------|
| `SessionStart` | First prompt of a new session | Initialize resources, log session |
| `UserPromptSubmit` | User submits a prompt | Audit requests, inject context |
| `PreToolUse` | Before agent invokes any tool | Block dangerous ops, require approval |
| `PostToolUse` | After tool completes | Run formatters, log results |
| `PreCompact` | Before context is compacted | Export state before truncation |
| `SubagentStart` | Subagent is spawned | Track nested agent usage |
| `SubagentStop` | Subagent completes | Aggregate results |
| `Stop` | Agent session ends | Cleanup, send notifications |

## Hook file locations

| Scope | Default location |
|-------|-----------------|
| Workspace | `.github/hooks/*.json` |
| Workspace (Claude) | `.claude/settings.json`, `.claude/settings.local.json` |
| User | `~/.copilot/hooks`, `~/.claude/settings.json` |
| Custom agent | `hooks` field in `.agent.md` frontmatter |
| Plugin | `hooks.json` or `hooks/hooks.json` |

Configure via `chat.hookFilesLocations` setting.

## Hook configuration format

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "type": "command",
        "command": "./scripts/validate-tool.sh",
        "timeout": 15
      }
    ],
    "PostToolUse": [
      {
        "type": "command",
        "command": "npx prettier --write \"$TOOL_INPUT_FILE_PATH\""
      }
    ]
  }
}
```

### Command properties

| Property | Type | Description |
|----------|------|-------------|
| `type` | string | Must be `"command"` |
| `command` | string | Default command (cross-platform) |
| `windows` | string | Windows-specific override |
| `linux` | string | Linux-specific override |
| `osx` | string | macOS-specific override |
| `cwd` | string | Working directory (relative to repo root) |
| `env` | object | Additional environment variables |
| `timeout` | number | Timeout in seconds (default: 30) |

### Matcher format (Claude compatibility)

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [{ "type": "command", "command": "npx prettier --write \"$TOOL_INPUT_FILE_PATH\"" }]
      }
    ]
  }
}
```

VS Code parses `matcher` for compatibility but currently ignores matcher values — hooks run on every matching event regardless of the tool name.

## Input/Output

Each hook receives structured JSON via stdin:

```json
{
  "timestamp": "2026-02-09T10:30:00.000Z",
  "cwd": "/path/to/workspace",
  "sessionId": "session-identifier",
  "hookEventName": "PreToolUse",
  "transcript_path": "/path/to/transcript.json"
}
```

Hooks return JSON via stdout:

```json
{
  "continue": true,
  "stopReason": "Security policy violation",
  "systemMessage": "Unit tests failed"
}
```

### Exit codes

| Code | Behavior |
|------|----------|
| `0` | Success: parse stdout as JSON |
| `2` | Blocking error: stop processing, show error to model |
| Other | Non-blocking warning: show warning, continue processing |

### PreToolUse hook-specific output

```json
{
  "hookSpecificOutput": {
    "permissionDecision": "deny",
    "permissionDecisionReason": "Destructive command blocked",
    "updatedInput": { "files": ["src/safe.ts"] },
    "additionalContext": "Read-only access to production files"
  }
}
```

`permissionDecision`: `"allow"` | `"deny"` | `"ask"` (most restrictive across hooks wins)

### Stop hook

The `Stop` hook receives `stop_hook_active` field. When `true`, the agent is already continuing from a previous stop — check this to prevent infinite loops. Use `decision: "block"` with a `reason` to prevent the agent from stopping.

## Event sequence

The expected order of hook events during a typical agent session:

1. `SessionStart` → agent session begins
2. `UserPromptSubmit` → user sends a message
3. `PreToolUse` → before each tool invocation
4. `PostToolUse` → after each tool completes
5. `SubagentStart` → when a subagent spawns
6. `SubagentStop` → when a subagent finishes
7. `PreCompact` → before context compaction
8. `Stop` → session ends

## Agent-scoped hooks

Define hooks in a custom agent's YAML frontmatter (requires `chat.useCustomAgentHooks: true`):

```markdown
---
name: "Strict Formatter"
description: "Agent that auto-formats code after every edit"
hooks:
  PostToolUse:
    - type: command
      command: "./scripts/format-changed-files.sh"
---
```

Only run when that agent is active.

## Supported command launching tools

Hooks can be triggered by any tool that runs commands or edits files. Common tools that fire `PreToolUse`/`PostToolUse`:

- `runTerminalCommand` — running shell commands
- `editFiles` / `replace_string_in_file` / `createFile` — file modifications
- `deleteFile` — file deletion
- `pushToGitHub` — git operations
- `readFile` — read-only (triggers PostToolUse but typically not blocked)

## Use cases

- Enforce policies: block `rm -rf` or `DROP TABLE`
- Auto-format: run formatters after file edits
- Audit trails: log every tool invocation
- Inject context: add project-specific information
- Control approvals: auto-approve safe operations

## Safety

If the agent has access to edit hook scripts, it can modify them mid-run. Use `chat.tools.edits.autoApprove` to disallow the agent from editing hook scripts without manual approval.

## Troubleshooting

View loaded hooks: **Output** panel → **GitHub Copilot Chat Hooks** channel. Check for "Load Hooks" entries to verify which files were loaded.

Common issues:
- **Hook not executing**: verify `.json` extension and `type: "command"`
- **Permission denied**: ensure scripts have execute permissions (`chmod +x`)
- **Timeout**: increase `timeout` value (default 30s)
- **JSON parse errors**: verify hook outputs valid JSON to stdout
