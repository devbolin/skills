---
source: https://code.visualstudio.com/docs/copilot/concepts/agents
retrieved: 2026-05-12
type: archived
---

# Agents (Conceptual)

An agent is an AI system that autonomously plans and executes coding tasks. This page explains the core architecture.

## Agent loop

1. **Understand**: reads files, searches codebase, looks up documentation
2. **Act**: modifies code, runs terminal commands, installs dependencies, calls external services
3. **Validate**: runs tests, checks compiler errors, reviews own changes

The agent chains actions together until the task is complete. Behind the scenes, VS Code assembles context into a prompt, sends it to the LLM, processes tool calls, and repeats.

### Customize the agent loop

- **Custom agents**: different personas with their own instructions, tools, models, handoffs
- **Agent Skills**: teach new capabilities for specific domains
- **Hooks**: run custom commands at lifecycle points

## Agents Application (Insiders)

The [Agents Application](https://code.visualstudio.com/docs/copilot/agents-app) provides a dedicated standalone window for interacting with agents. It gives direct access to all AI customizations (agents, instructions, prompts, MCP servers) from a single sidebar panel.

## Agent types

| Type | Environment | Interaction |
|------|-------------|-------------|
| Local | Your machine | Interactive (editor) |
| Copilot CLI | Your machine | Background (terminal) |
| Cloud | GitHub infrastructure | Autonomous (PRs, issues) |
| Third-party | Anthropic/OpenAI harness | Local or cloud |

## Subagents

Independent AI agents that perform focused work and report results back to the main agent.

Key characteristics:
- **Context isolation**: each subagent runs in its own context window
- **Synchronous**: main agent waits for results
- **Parallel**: multiple subagents can run simultaneously (VS Code can spawn multiple subagents in parallel for tasks like analyzing security, performance, and accessibility simultaneously)
- **Focused results**: only final result returned to main agent

The built-in Plan agent uses subagents for research and analysis before creating implementation plans.

## Memory

Two complementary systems:

| System | Scope | Persistence |
|--------|-------|-------------|
| Memory tool (local) | User (/memories/), Repo (/memories/repo/), Session (/memories/session/) | Cross-session or per-session |
| Copilot Memory (GitHub) | Repository-specific insights | Shared across Copilot surfaces |

## Planning

The Plan agent researches the task and creates a detailed implementation plan before any code changes. Ensures requirements are understood and edge cases are addressed.
