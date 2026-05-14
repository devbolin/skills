---
source: https://opencode.ai/docs/network/
retrieved: 2026-05-12
type: archived
---

# Network

Configure proxies and custom certificates.

## Proxy
OpenCode respects standard proxy environment variables:
- `HTTPS_PROXY` - HTTPS proxy (recommended)
- `HTTP_PROXY` - HTTP proxy
- `NO_PROXY` - Bypass proxy (must include localhost,127.0.0.1)

### Authenticate
Include credentials in URL: `http://username:password@proxy.example.com:8080`

## Custom certificates
Use `NODE_EXTRA_CA_CERTS` env var to point to custom CA certificate file.
