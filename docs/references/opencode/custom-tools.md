---
source: https://opencode.ai/docs/custom-tools/
retrieved: 2026-05-12
type: archived
---

# Custom Tools

Create tools the LLM can call in opencode.

Custom tools are functions you create that the LLM can call during conversations. They work alongside opencode's built-in tools like `read`, `write`, and `bash`.

---

## Creating a tool

Tools are defined as TypeScript or JavaScript files.

### Location

- Locally: `.opencode/tools/` in your project
- Globally: `~/.config/opencode/tools/`

### Structure

```ts
import { tool } from "@opencode-ai/plugin"

export default tool({
  description: "Query the project database",
  args: {
    query: tool.schema.string().describe("SQL query to execute"),
  },
  async execute(args) {
    return `Executed query: ${args.query}`
  },
})
```

Multiple tools per file using named exports:

```ts
export const add = tool({ ... })
export const multiply = tool({ ... })
```

This creates `math_add` and `math_multiply` tools.

Custom tools can override built-in tools if they share the same name.

### Arguments

Use `tool.schema` (Zod) to define argument types.

### Context

Tools receive context about the current session:

```ts
async execute(args, context) {
  const { agent, sessionID, messageID, directory, worktree } = context
}
```

---

## Examples

Write tool logic in any language. Define the tool in TypeScript, then call external scripts via `Bun.$` or similar.
