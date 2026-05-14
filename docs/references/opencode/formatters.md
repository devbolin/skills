---
source: https://opencode.ai/docs/formatters/
retrieved: 2026-05-12
type: archived
---

# Formatters

OpenCode uses language specific formatters.

OpenCode can format files after they are written or edited using language-specific formatters. Formatters are disabled by default; enable them in your config before OpenCode will run them.

---

## Built-in

| Formatter | Extensions | Requirements |
|---|---|---|
| air | .R | `air` command available |
| biome | .js, .jsx, .ts, .tsx, .html, .css, .md, .json, .yaml | `biome.json(c)` config file |
| cargofmt | .rs | `cargo fmt` command available |
| clang-format | .c, .cpp, .h, .hpp, .ino | `.clang-format` config file |
| cljfmt | .clj, .cljs, .cljc, .edn | `cljfmt` command available |
| dart | .dart | `dart` command available |
| dfmt | .d | `dfmt` command available |
| gleam | .gleam | `gleam` command available |
| gofmt | .go | `gofmt` command available |
| htmlbeautifier | .erb, .html.erb | `htmlbeautifier` command available |
| ktlint | .kt, .kts | `ktlint` command available |
| mix | .ex, .exs, .eex, .heex, .leex, .neex, .sface | `mix` command available |
| nixfmt | .nix | `nixfmt` command available |
| ocamlformat | .ml, .mli | `ocamlformat` command and `.ocamlformat` config |
| ormolu | .hs | `ormolu` command available |
| oxfmt (Experimental) | .js, .jsx, .ts, .tsx | `oxfmt` dependency in `package.json` |
| pint | .php | `laravel/pint` in `composer.json` |
| prettier | .js, .jsx, .ts, .tsx, .html, .css, .md, .json, .yaml | `prettier` in `package.json` |
| rubocop | .rb, .rake, .gemspec, .ru | `rubocop` command available |
| ruff | .py, .pyi | `ruff` command available with config |
| rustfmt | .rs | `rustfmt` command available |
| shfmt | .sh, .bash | `shfmt` command available |
| standardrb | .rb, .rake, .gemspec, .ru | `standardrb` command available |
| terraform | .tf, .tfvars | `terraform` command available |
| uv | .py, .pyi | `uv` command available |
| zig | .zig, .zon | `zig` command available |

---

## How it works

When OpenCode writes or edits a file and formatters are enabled, it checks the file extension, runs the appropriate formatter, and applies formatting changes.

---

## Configure

Enable formatters by setting `formatter` to `true`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": true
}
```

### Disabling formatters

Set `formatter` to `false` to disable all, or set `disabled: true` on a specific formatter.

### Custom formatters

```json
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": {
    "custom-markdown-formatter": {
      "command": ["deno", "fmt", "$FILE"],
      "extensions": [".md"]
    }
  }
}
```

The `$FILE` placeholder is replaced with the file path.
