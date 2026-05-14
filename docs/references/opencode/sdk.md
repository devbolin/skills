---
source: https://opencode.ai/docs/sdk/
retrieved: 2026-05-12
type: archived
---

# SDK

Type-safe JS client for opencode server.

The opencode JS/TS SDK provides a type-safe client for interacting with the server.

---

## Install

```bash
npm install @opencode-ai/sdk
```

---

## Create client

```ts
import { createOpencode } from "@opencode-ai/sdk"
const { client } = await createOpencode()
```

### Client only

Connect to an existing server:

```ts
import { createOpencodeClient } from "@opencode-ai/sdk"
const client = createOpencodeClient({
  baseUrl: "http://localhost:4096",
})
```

---

## Structured Output

Request structured JSON output from the model:

```ts
const result = await client.session.prompt({
  path: { id: sessionId },
  body: {
    parts: [{ type: "text", text: "Research Anthropic" }],
    format: {
      type: "json_schema",
      schema: {
        type: "object",
        properties: {
          company: { type: "string" },
          founded: { type: "number" },
        },
        required: ["company", "founded"],
      },
    },
  },
})
```

---

## APIs

The SDK exposes APIs for: Global (health), App (agents, logging), Project, Path, Config, Sessions (CRUD, prompt, shell, share, revert), Files (text search, file find, symbols, read, status), TUI (append prompt, toast, execute command), Auth, and Events (SSE stream).
