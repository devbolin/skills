# Validation Error Examples

Common validation errors and their fixes.

| Error | Suggestion |
|-------|------------|
| `name: Code-Review` (uppercase) | Change to `name: code-review` |
| `name` length 0 or > 64 chars | Adjust to between 1-64 characters |
| `name: -code-review` (leading hyphen) | Remove leading hyphen: `code-review` |
| `description` missing | Add a description (1-1024 chars) |
| `description` > 1024 chars | Shorten description to under 1024 characters |
| Directory name mismatch | Rename directory or fix `name` field |
| YAML parse error | Check for syntax issues in frontmatter (colons need quoting) |
