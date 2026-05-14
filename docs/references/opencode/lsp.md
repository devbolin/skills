---
source: https://opencode.ai/docs/lsp/
retrieved: 2026-05-12
type: archived
---

# LSP Servers

OpenCode integrates with your LSP servers.

OpenCode can integrate with your Language Server Protocol (LSP) to help the LLM interact with your codebase. It uses diagnostics to provide feedback to the LLM.

---

## Built-in

| LSP Server | Extensions | Requirements |
|---|---|---|
| astro | .astro | Auto-installs for Astro projects |
| bash | .sh, .bash, .zsh, .ksh | Auto-installs bash-language-server |
| clangd | .c, .cpp, .cc, .cxx, .h, .hpp | Auto-installs for C/C++ projects |
| csharp | .cs, .csx | .NET SDK installed |
| clojure-lsp | .clj, .cljs, .cljc, .edn | `clojure-lsp` available |
| dart | .dart | `dart` available |
| deno | .ts, .tsx, .js, .jsx, .mjs | `deno` available (auto-detects deno.json) |
| elixir-ls | .ex, .exs | `elixir` available |
| eslint | .ts, .tsx, .js, .jsx, .mjs, .cjs, .mts, .cts, .vue | `eslint` dependency |
| fsharp | .fs, .fsi, .fsx, .fsscript | .NET SDK installed |
| gleam | .gleam | `gleam` available |
| gopls | .go | `go` available |
| hls | .hs, .lhs | `haskell-language-server-wrapper` available |
| jdtls | .java | Java SDK (21+) installed |
| julials | .jl | `julia` and `LanguageServer.jl` |
| kotlin-ls | .kt, .kts | Auto-installs for Kotlin projects |
| lua-ls | .lua | Auto-installs for Lua projects |
| nixd | .nix | `nixd` available |
| ocaml-lsp | .ml, .mli | `ocamllsp` available |
| oxlint | .ts, .tsx, .js, .jsx, .mjs, .cjs, .mts, .cts, .vue, .astro, .svelte | `oxlint` dependency |
| php intelephense | .php | Auto-installs for PHP projects |
| prisma | .prisma | `prisma` available |
| pyright | .py, .pyi | `pyright` installed |
| razor | .razor, .cshtml | .NET SDK + VS Code C# extension |
| ruby-lsp | .rb, .rake, .gemspec, .ru | `ruby` and `gem` available |
| rust | .rs | `rust-analyzer` available |
| sourcekit-lsp | .swift, .objc, .objcpp | `swift` installed |
| svelte | .svelte | Auto-installs for Svelte projects |
| terraform | .tf, .tfvars | Auto-installs from GitHub releases |
| tinymist | .typ, .typc | Auto-installs from GitHub releases |
| typescript | .ts, .tsx, .js, .jsx, .mjs, .cjs, .mts, .cts | `typescript` dependency |
| vue | .vue | Auto-installs for Vue projects |
| yaml-ls | .yaml, .yml | Auto-installs yaml-language-server |
| zls | .zig, .zon | `zig` available |

---

## Configure

Enable all built-in LSP servers by setting `lsp` to `true`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": true
}
```

### Environment variables

```json
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": {
    "rust": {
      "command": ["rust-analyzer"],
      "env": { "RUST_LOG": "debug" }
    }
  }
}
```

### Custom LSP servers

```json
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": {
    "custom-lsp": {
      "command": ["custom-lsp-server", "--stdio"],
      "extensions": [".custom"]
    }
  }
}
```
