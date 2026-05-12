---
source: https://code.visualstudio.com/docs/copilot/ai-powered-suggestions
retrieved: 2026-05-12
type: archived
---

# Inline suggestions from GitHub Copilot in VS Code

GitHub Copilot provides AI-powered inline suggestions that complete your code, comments, tests, and more as you type.

## Two types of suggestions

### Ghost text suggestions

Dimmed ghost text appears at your cursor as you type. Press **Tab** to accept. Use ⌘→ (Ctrl+Right) to accept the next word/line.

### Next edit suggestions (NES)

Predicts both the location and content of your next edit based on current edits. Navigate with **Tab**. Enable with:
```
github.copilot.nextEditSuggestions.enabled
```

Use cases:
- Catching mistakes (typos, logic errors, inverted conditions)
- Changing intent (e.g., Point → Point3D, suggests adding `z`)
- Refactoring (rename a variable, suggests updates everywhere)
- Matching code (e.g., adding a parameter, suggests updating all callers)

## Enable or disable

Use the Copilot menu in the Status Bar, or set:
```json
"github.copilot.enable": { "*": true }
```

## Settings reference

| Setting | Description |
|---------|-------------|
| `github.copilot.enable` | Enable/disable inline completions per language |
| `github.copilot.nextEditSuggestions.enabled` | Enable Copilot NES |
| `editor.inlineSuggest.edits.renderSideBySide` | Show NES suggestions side-by-side |
| `github.copilot.nextEditSuggestions.fixes` | Enable NES based on diagnostics |
| `editor.inlineSuggest.minShowDelay` | Minimum delay before showing suggestions |
