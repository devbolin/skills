---
source: https://code.visualstudio.com/docs/copilot/setup
retrieved: 2026-05-12
type: archived
---

# Set up GitHub Copilot in VS Code

## Getting started

1. Hover over the Copilot icon in the Status Bar and select **Use AI Features**.
2. Choose a sign-in method and follow the prompts.
3. If you don't have a subscription, you'll be signed up for Copilot Free.

After setup, start using Copilot and type `/init` in a chat session to configure your project for AI.

## Use Copilot with a GHE account

1. In the sign in dialog, choose **Continue with GHE.com**.
2. Provide your GHE instance URL and credentials.

## Use a different GitHub account per workspace/profile

Configure via **Accounts > Manage Extension Account Preferences > GitHub Copilot Chat**.

For GHE accounts, add to `settings.json`:
```json
"github.copilot.advanced": {
    "authProvider": "github-enterprise"
}
```

## Remove AI features from VS Code

Set `chat.disableAIFeatures` to disable and hide all AI features in VS Code.

## Disable AI features for a workspace

Set `chat.disableAIFeatures` in workspace `settings.json`.
