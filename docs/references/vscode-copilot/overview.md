---
source: https://code.visualstudio.com/docs/copilot/overview
retrieved: 2026-05-12
type: archived
---

# GitHub Copilot in VS Code

GitHub Copilot brings AI agents to Visual Studio Code. Describe what you want to build, and an agent plans the approach, writes the code, and verifies the result across your entire project. Choose from Copilot's built-in agents, third-party agents from providers like Anthropic and OpenAI, or your own custom agents, and run them locally, in the background, or in the cloud.

## Agents

An agent is an AI assistant that works autonomously to complete a coding task. Unlike traditional code completion, which suggests the next few lines, an agent takes a goal, breaks it into steps, edits files across your project, runs commands, and self-corrects when something goes wrong.

### Plan before you build

Use the built-in Plan agent to break a task into a structured implementation plan before writing any code. The Plan agent analyzes your codebase, asks clarifying questions, and produces a step-by-step plan. When the plan looks right, hand it off to an implementation agent to execute it.

### Run agents anywhere

Agents run where the work needs to happen. Run them locally in VS Code for interactive work, in the background for autonomous tasks, or in the cloud for team collaboration through pull requests. You can also use third-party agents from providers like Anthropic and OpenAI.

### Manage sessions from a central view

Run multiple agent sessions in parallel. The Sessions view in the Chat panel gives you a single place to monitor all active sessions, whether they run locally, in the background, or in the cloud.

## What you can build

- **Build a feature end-to-end**: Describe a feature in natural language and the agent scaffolds the project, implements logic across multiple files, and runs tests.
- **Debug and fix failing tests**: Point an agent at a failing test and it traces the root cause, applies a fix, and re-runs.
- **Refactor or migrate a codebase**: Ask an agent to plan a migration and it applies coordinated changes across files.
- **Test and interact with web apps** (Experimental): Ask an agent to open your web app in the integrated browser, verify features, check layout, or take screenshots.
- **Collaborate via pull requests**: Delegate to a cloud agent that creates a branch, implements changes, and opens a PR.

## Getting started

1. Hover over the Copilot icon in the Status Bar and select **Set up Copilot**.
2. Open the Chat view (⌃⌘I).
3. Enter a prompt describing what you want to build.
4. Type `/init` to configure your project for AI (creates custom instructions).

## AI assistance as you type

- **Inline suggestions**: Code suggestions as you type, from single-line completions to full functions.
- **Inline chat** (⌘I): Open a chat prompt directly in the editor for quick, focused edits.
- **Smart actions**: Predefined AI-powered actions for commit messages, renaming symbols, fixing errors, and semantic search.

## Customize AI for your workflow

Set up custom instructions. Define custom agents for specialized roles. Use prompt files for reusable prompts. Configure language models, MCP servers, and hooks. Install agent plugins from the Marketplace.

## Pricing

Copilot Free is available with monthly limits on inline suggestions and chat interactions. Paid plans (Pro, Pro+) offer more features and capacity. Starting April 20, 2026, new sign-ups for Copilot Pro, Pro+, and student plans are temporarily paused.
