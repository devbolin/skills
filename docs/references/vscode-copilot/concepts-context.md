---
source: https://code.visualstudio.com/docs/copilot/concepts/context
retrieved: 2026-05-12
type: archived
---

# Context

Context is everything the model can see when generating a response — conversation history, file contents, tool outputs, custom instructions, and explicit references.

## How VS Code assembles context

1. **System instructions**: built-in agent behavior guidelines
2. **Customizations**: agents, skills, custom instructions
3. **User message**: current prompt
4. **Conversation history**: messages in the current session
5. **Implicit context**: active file, selection, errors, git state
6. **Explicit references**: `#file`, `#codebase`, `#fetch` mentions
7. **Tool outputs**: results from file reads, terminal commands, codebase search

## Workspace indexing

| Index type | Description |
|------------|-------------|
| Remote index | GitHub-hosted, fast for large repos |
| Local index | Advanced semantic index on your machine |
| Basic index | Fallback when local indexing unavailable |

## Implicit context

- Active editor selection
- Current file/notebook name
- Ask agent: active file auto-included
- Agent: decides autonomously if active file is needed

## conversation.md format

VS Code supports a `conversation.md` format for exporting and sharing chat sessions. The format captures:
- **Messages**: user prompts and agent responses with timestamps
- **Context**: files referenced, tool outputs, and explicit mentions
- **Metadata**: model used, agent type, session ID

## Participants and contextValue

Different context participants contribute different types of information:

| Participant | `contextValue` | Description |
|-------------|---------------|-------------|
| `#file` | `file` | Specific file contents |
| `#codebase` | `codebase` | Workspace index search results |
| `#fetch` | `web` | Web page content |
| `#editor` | `editor` | Active editor state |
| `#terminal` | `terminal` | Terminal last command output |
| `#git` | `git` | Git status/diff |

The `contextValue` distinguishes participants in LM API calls and tool invocations, allowing agents to handle different context types appropriately.

## Effective context tips

- Start new sessions for new tasks
- Reference specific files with `#file` instead of entire codebase
- Use custom instructions for persistent rules
- Use `#fetch` to provide current web documentation
