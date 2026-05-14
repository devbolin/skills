---
source: https://opencode.ai/docs/windows-wsl/
retrieved: 2026-05-12
type: archived
---

# Windows (WSL)

Run OpenCode on Windows using WSL for the best experience.

## Setup
1. Install WSL
2. Install OpenCode in WSL: `curl -fsSL https://opencode.ai/install | bash`
3. Access Windows files via `/mnt/c/`, `/mnt/d/`, etc.

## Desktop App + WSL Server
1. Start server in WSL: `opencode serve --hostname 0.0.0.0 --port 4096`
2. Connect Desktop app to `http://localhost:4096`

## Web Client + WSL
Run `opencode web --hostname 0.0.0.0` from WSL terminal, access from Windows browser.

## Tips
- Clone repos into WSL filesystem for best performance
- Use VS Code's WSL extension alongside OpenCode
