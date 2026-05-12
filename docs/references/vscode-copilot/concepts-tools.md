---
source: https://code.visualstudio.com/docs/copilot/concepts/tools
retrieved: 2026-05-12
type: archived
---

# Tools

Tools are the mechanism that lets the model act on your development environment. Without tools, a language model can only generate text. With tools, an agent can read files, write code, run terminal commands, search your codebase, and connect to external services.

## Types of tools

| Type | Description |
|------|-------------|
| **Built-in tools** | Ship with VS Code: read/write files, terminal commands, codebase search, editor navigation |
| **MCP tools** | From MCP servers: databases, APIs, external services |
| **Extension tools** | From VS Code extensions via Language Model Tools API |

## How tools work

During the agent loop, the model examines available tools and decides which to call. You can explicitly reference tools with `#` followed by the tool name in prompts.

## Control tool availability

Use the **Configure Tools** button in chat input to enable/disable tools per request. Benefits:
- **Preserve context**: fewer unnecessary tool calls
- **More relevant results**: agent focuses on appropriate tools
- **Improve performance**: smaller decision space for the model

Control tools via prompt files (`tools` field) and custom agents (tool restrictions).

## Tool approval and trust

- **Approval prompts**: side-effect tools show confirmation dialogs
- **URL approval**: two-step verify request and response
- **Permission levels**: from manual approval to fully autonomous

See [Trust and safety](https://code.visualstudio.com/docs/copilot/concepts/trust-and-safety) for details.
