---
source: https://opencode.ai/docs/troubleshooting/
retrieved: 2026-05-12
type: archived
---

# Troubleshooting

Common issues and how to resolve them.

## Logs
- macOS/Linux: `~/.local/share/opencode/log/`
- Windows: `%USERPROFILE%\.local\share\opencode\log`
- Use `--log-level DEBUG` for detailed output

## Storage
Session data at `~/.local/share/opencode/` (auth.json, log/, project/)

## Desktop app issues
Quick checks: restart, reload webview, disable plugins, clear cache (`~/.cache/opencode`)

Platform-specific: Wayland/X11 on Linux, WebView2 on Windows, WSL recommended for Windows

## Common issues
- OpenCode won't start: check logs, try `--print-logs`
- Authentication issues: re-authenticate with `/connect`
- Model not available: verify model name format `<providerId>/<modelId>`
- ProviderInitError: clear `~/.local/share/opencode` and re-authenticate
- AI_APICallError: clear `~/.cache/opencode` to refresh provider packages
- Copy/paste on Linux: install xclip/xsel (X11) or wl-clipboard (Wayland)
