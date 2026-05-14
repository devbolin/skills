---
source: https://opencode.ai/docs/web/
retrieved: 2026-05-12
type: archived
---

# Web

Using OpenCode in your browser.

Run `opencode web` to start a local server and open OpenCode in your browser.

## Configuration
- Port: `--port 4096`
- Hostname: `--hostname 0.0.0.0` for network access
- mDNS: `--mdns` for local network discovery
- CORS: `--cors https://example.com`
- Auth: `OPENCODE_SERVER_PASSWORD` env var

## Attaching a Terminal
`opencode attach http://localhost:4096` to attach TUI to running web server.

## Config File
Server settings in `opencode.json` under `server` key.
