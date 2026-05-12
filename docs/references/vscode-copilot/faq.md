---
source: https://code.visualstudio.com/docs/copilot/faq
retrieved: 2026-05-12
type: archived
---

# GitHub Copilot frequently asked questions

## Subscription

**How can I get a Copilot subscription?**
- Individual: Set up Copilot Free or sign up for a paid plan.
- Organization/Enterprise member: Request access at https://github.com/settings/copilot.

**What's the advantage of signing in with a GitHub account?**
Increased chat limits, premium models, BYOK, remote indexing, code review, content exclusions, and cloud agent delegation.

**How can I monitor my usage?**
Use the Copilot status dashboard in the VS Code Status Bar. Shows inline suggestion %, chat messages %, premium requests %, and overage.

**I reached my limit**
Limits reset monthly. Free plan users can wait or upgrade. Paid plan users can still use included models or request additional premium requests.

## General

**How can I remove Copilot from VS Code?**
Set `chat.disableAIFeatures` to hide all AI features.

**Are there pre-release builds?**
Yes. Switch to Pre-Release Version from the extension's context menu.

**Network/firewall configuration?**
Ensure Copilot domain URLs are allowlisted. Troubleshoot VPN/proxy issues.

## Inline suggestions

**How do I enable/disable?**
Use Copilot menu in Status Bar, or set `github.copilot.enable`.

**Why aren't suggestions working?**
Check: latest VS Code, latest Copilot extensions, active subscription, not over limit, proper network connectivity.

## Chat

**What can I use chat for?**
Ask questions, get explanations, refactor, implement features, debug, generate tests, search codebase.

**How do I use agents?**
Select Agent from the agent dropdown in Chat view. Ensure `chat.agent.enabled` is set and your organization hasn't disabled agents.

**Are agents usage-limited?**
Yes. Agents use premium requests from your plan.

## Enterprise

**Can organizations manage Copilot?**
Yes. Admins can control agents, models, content exclusions, and trust boundaries through enterprise AI settings.
