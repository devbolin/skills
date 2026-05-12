---
source: https://code.visualstudio.com/docs/copilot/agents/overview
retrieved: 2026-05-12
type: archived
---

# Using agents in Visual Studio Code

An agent is an AI assistant that works autonomously to complete a coding task. Give it a high-level goal, and it breaks the goal into steps, edits files across your project, runs commands, and self-corrects when something goes wrong.

## Types of agents

| Type | Description |
|------|-------------|
| **Local** | Runs interactively in the editor with full access to your workspace, tools, and models. |
| **Copilot CLI** | Runs in the background on your machine, optionally using Git worktrees for isolation. |
| **Cloud** | Runs remotely on GitHub Copilot infrastructure. Integrates with GitHub pull requests for team collaboration. |
| **Third-party** | Uses the third-party agent harness and SDK from Anthropic and OpenAI. |

## Which agent to use

| I want to... | Use |
|-------------|------|
| Brainstorm, explore, or iterate interactively | Local agent |
| Get answers about my codebase | Local agent (Ask) |
| Create a structured implementation plan | Local agent (Plan) |
| Fix issues needing editor context | Local agent |
| Build and test web apps with integrated browser (Experimental) | Local agent |
| Use VS Code extension tools or MCP servers | Local agent |
| Implement a well-defined task while I keep working | Copilot CLI or Cloud agent |
| Explore multiple variants or proof of concepts | Copilot CLI or Cloud agent |
| Create a PR for team review | Cloud agent |
| Assign a GitHub issue to an agent | Cloud agent |
| Use a specific AI provider (Anthropic, OpenAI) | Third-party agent |

## Built-in agents

- **Agent**: Autonomously plans and implements changes across files, runs terminal commands, and invokes tools.
- **Plan**: Creates a structured, step-by-step implementation plan before writing code. Hands off to an implementation agent.
- **Ask**: Answers questions about coding concepts, your codebase, or VS Code itself without making file changes.

## Permission levels

| Level | Description |
|-------|-------------|
| **Default Approvals** | Uses approvals as specified in VS Code settings. Only read-only and safe tools bypass approval. |
| **Bypass Approvals** | Auto-approves all tool calls. The agent may ask clarifying questions. |
| **Autopilot (Preview)** | Auto-approves all tool calls, auto-responds to questions. Agent continues autonomously until the task is complete. |

## Hand off sessions

You can hand off a task from one agent to another to leverage their unique strengths. For example: create a plan with a local agent, hand off to Copilot CLI for proof of concepts, then continue with a cloud agent to submit a PR.
