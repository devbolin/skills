---
source: https://opencode.ai/docs/providers/
retrieved: 2026-05-12
type: archived
---

# Providers

Using any LLM provider in OpenCode.

OpenCode uses the AI SDK and Models.dev to support 75+ LLM providers and local models.

## Credentials
API keys stored in `~/.local/share/opencode/auth.json` via `/connect` command.

## Base URL
Customize base URL for any provider via `baseURL` option.

## OpenCode Zen
Curated list of tested models. Run `/connect` and select OpenCode Zen.

## OpenCode Go
Low cost subscription ($5 first month, then $10/month) for open coding models.

## Supported providers (partial list)
302.AI, Amazon Bedrock, Anthropic, Atomic Chat, Azure OpenAI, Azure Cognitive Services, Baseten, Cerebras, Cloudflare AI Gateway, Cloudflare Workers AI, Cortecs, DeepSeek, Deep Infra, FrogBot, Fireworks AI, GitLab Duo, GitHub Copilot, Google Vertex AI, Groq, Hugging Face, Helicone, llama.cpp, IO.NET, LM Studio, Moonshot AI, MiniMax, NVIDIA, Nebius Token Factory, Ollama, Ollama Cloud, OpenAI, OpenRouter, LLM Gateway, SAP AI Core, STACKIT, OVHcloud AI Endpoints, Scaleway, Together AI, Venice AI, Vercel AI Gateway, xAI, Z.AI, ZenMux

Amazon Bedrock supports AWS-specific configuration (region, profile, endpoint, VPC endpoints) with multiple authentication methods.

## Custom provider
Configure any OpenAI-compatible endpoint via npm package `@ai-sdk/openai-compatible`.
