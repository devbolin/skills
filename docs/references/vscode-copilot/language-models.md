---
source: https://code.visualstudio.com/docs/copilot/customization/language-models
retrieved: 2026-05-12
type: archived
---

# AI language models in VS Code

VS Code offers built-in language models optimized for different tasks. You can also bring your own API key for models from other providers.

## Model selection

- **Fast models**: quick edits, simple questions
- **Reasoning models**: complex refactoring, architectural decisions, multi-step tasks
- **Auto selection**: VS Code automatically selects optimal model (Claude Sonnet 4, GPT-5, etc.). Detects degraded performance and switches.

## Change chat model

Use the model picker in the chat input field. For paid plans, shows premium request multiplier. Auto model selection applies multiplier discounts.

## Configure thinking effort

Reasoning models support configurable thinking effort (Low/Medium/High). Adaptive reasoning lets the model determine effort dynamically. Configure via the model picker submenu (arrow next to reasoning model name).

## Manage language models

Open via model picker → **Manage Models**, or run **Chat: Manage Language Models**. Browse, filter by provider/capability/visibility, show/hide models in picker.

## Bring Your Own Key (BYOK)

Add models from providers like OpenAI, Anthropic, Google, Ollama, etc. Manage keys in the Language Models editor.

### Supported API types

BYOK supports two API types:
- **completions**: for inline suggestions (ghost text)
- **chat**: for chat conversations and agents

### Provider configuration

When adding a model from a built-in provider, configure:

| Parameter | Description |
|-----------|-------------|
| `providerId` | Provider identifier (e.g., `openai`, `anthropic`, `ollama`) |
| `model` | Model name/identifier |
| `apiKey` | API key for authentication |
| `endpoint` | Optional custom endpoint URL |

### Considerations

- BYOK only affects chat — inline suggestions use built-in models
- Capabilities are model-dependent (tool calling, vision, thinking may differ)
- Copilot service still used for embeddings, indexing, intent detection
- No guarantee of responsible AI filtering on BYOK output
- Requires a Copilot plan (e.g., Free) and internet connection

### Locally hosted models

VS Code supports local models via:
- Built-in providers (e.g., Ollama)
- Extensions (e.g., AI Toolkit for VS Code with Foundry Local)
- Custom OpenAI-compatible endpoints (Insiders, setting `github.copilot.chat.customOAIModels`)

## LM API call example

```typescript
const [model] = await vscode.lm.selectChatModels({
  vendor: 'copilot',
  family: 'gpt-4o'
});

const messages = [
  new vscode.LanguageModelChatSystemMessage('You are a helpful assistant.'),
  new vscode.LanguageModelChatUserMessage('Explain async/await in JavaScript.')
];

const response = await model.sendRequest(messages, {});
for await (const chunk of response.text) {
  console.log(chunk);
}
```

### Message roles

| Role | Class | Description |
|------|-------|-------------|
| System | `LanguageModelChatSystemMessage` | System instructions for the model |
| User | `LanguageModelChatUserMessage` | User input/query |
| Assistant | `LanguageModelChatAssistantMessage` | Model responses |

## FAQ

**Q: How do I enable BYOK for Copilot Business/Enterprise?**
A: Admin must enable "Bring Your Own Language Model Key in VS Code" policy in Copilot policy settings on GitHub.com.

**Q: Can I use locally hosted models?**
A: Yes, via BYOK with a local provider (Ollama, AI Toolkit, etc.). Still requires a Copilot plan and internet for Copilot service tasks.

**Q: Can I use a local model without internet?**
A: No. Currently requires Copilot service access. May change in future.

**Q: Can I use a local model without a Copilot plan?**
A: No. Requires at least Copilot Free. May change in future.

## Related settings

| Setting | Description |
|---------|-------------|
| `github.copilot.chat.anthropic.thinking.effort` | (Deprecated) Use model picker instead |
| `github.copilot.chat.responsesApiReasoningEffort` | (Deprecated) Use model picker instead |
| `inlineChat.defaultModel` | Default model for inline chat |
