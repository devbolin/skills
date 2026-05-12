---
source: https://code.visualstudio.com/docs/copilot/concepts/overview
retrieved: 2026-05-12
type: archived
---

# AI features in VS Code

Visual Studio Code's built-in AI features are powered by GitHub Copilot and large language models (LLMs). These features span multiple surfaces, from inline suggestions as you type to autonomous agents that implement entire features.

## AI features at a glance

| Feature | Description |
|---------|-------------|
| **Agents** | Autonomous sessions following the full agent loop — reading files, executing coordinated changes across multiple files, running commands, iterating until complete. Handles multi-step tasks end-to-end. |
| **Chat** | Primary interface for interacting with agents and having multi-turn conversations. Switch between Agent, Ask, Plan, and custom agents. |
| **Inline chat** | Lightweight chat interface in the editor for quick, focused edits. |
| **Inline suggestions** | Ghost text code suggestions as you type. Uses specialized completion models (no agent loop). Next Edit Suggestions (NES) predict where your next edit should happen. |
| **Smart actions** | One-click AI actions like generating commit messages or fixing diagnostics errors. |

## Concepts

The following conceptual articles explain the architecture and building blocks:

| Concept | Description |
|---------|-------------|
| **Language models** | The AI models that power all features, including how to choose and configure them. |
| **Context** | How VS Code assembles information for the model — from your files to conversation history. |
| **Tools** | Mechanisms that let agents act on your development environment and connect to external services. |
| **Agents** | The agent loop, agent types, subagents, memory, and planning. |
| **Customization** | How to tailor AI behavior with instructions, prompt files, custom agents, skills, hooks, and plugins. |
| **Trust and safety** | Control mechanisms, AI limitations, and security considerations. |

## Related resources

- [Quickstart: Get started with AI in VS Code](https://code.visualstudio.com/docs/copilot/getting-started)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/copilot/guides/best-practices)
- [Using agents in VS Code](https://code.visualstudio.com/docs/copilot/agents/overview)
