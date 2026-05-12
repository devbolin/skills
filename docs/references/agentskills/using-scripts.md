---
source: https://agentskills.io/docs/using-scripts
retrieved: 2026-05
type: standard
---

# Using scripts in skills

> How to run commands and bundle executable scripts in your skills.

## One-off commands

When an existing package already does what you need, reference it directly in your `SKILL.md` instructions without a `scripts/` directory.

- **uvx**: Runs Python packages in isolated environments. Ships with [uv](https://docs.astral.sh/uv/).
- **pipx**: Alternative to uvx. Available via OS package managers.
- **npx**: Runs npm packages. Ships with npm/Node.js.
- **bunx**: Bun's equivalent of npx.
- **deno run**: Runs scripts directly from URLs or specifiers.
- **go run**: Compiles and runs Go packages directly.

Tips:

- Pin versions (e.g., `npx eslint@9.0.0`) for reproducibility.
- State prerequisites in your `SKILL.md`.
- Move complex commands into scripts.

## Referencing scripts from `SKILL.md`

Use relative paths from the skill directory root:

```markdown
## Available scripts

- **`scripts/validate.sh`** — Validates configuration files
- **`scripts/process.py`** — Processes input data
```

```markdown
## Workflow

1. Run the validation script:
   ```bash
   bash scripts/validate.sh "$INPUT_FILE"
   ```
```

## Self-contained scripts

Several languages support inline dependency declarations:

- **Python (PEP 723)**: Declare dependencies in a TOML block inside `# ///` markers. Run with `uv run`.
- **Deno**: Use `npm:` and `jsr:` import specifiers.
- **Bun**: Auto-installs missing packages at runtime.
- **Ruby**: Use `bundler/inline` to declare gems.

## Designing scripts for agentic use

### Avoid interactive prompts

Accept all input via command-line flags, environment variables, or stdin.

### Document usage with `--help`

Include a brief description, available flags, and usage examples.

### Write helpful error messages

Say what went wrong, what was expected, and what to try.

### Use structured output

Prefer JSON, CSV, TSV over free-form text. Send data to stdout and diagnostics to stderr.

### Further considerations

- **Idempotency**: "Create if not exists" is safer than "create and fail on duplicate."
- **Dry-run support**: A `--dry-run` flag lets the agent preview what will happen.
- **Meaningful exit codes**: Use distinct exit codes for different failure types.
- **Safe defaults**: Consider whether destructive operations should require explicit confirmation flags.
- **Predictable output size**: Default to a summary or reasonable limit; support `--offset` for pagination.
