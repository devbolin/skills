---
name: link-checker
description: Verify internal links and external references. Use when user says "check links", "verify references", or "find broken links".
---

# Link Checker

Verifies internal links and external references in documentation.

## Link Types

### Internal Links
- Relative paths: `./docs/...`, `../guides/...`
- Markdown links to other docs
- Cross-references within repo

### External Links
- URLs to external resources
- Reference links to official documentation

## Usage

1. Parse markdown for all links
2. Categorize by type (internal/external)
3. Verify each link
4. Report status

## Validation Rules

### Internal Links
- File must exist at path
- Fragment (#anchor) targets must exist in file
- Case-sensitive path matching

### External Links
- URL must be reachable
- Redirects followed
- Timeout after 10 seconds

## Output Format

```markdown
## Link Check Report

### Internal Links
| Status | Path | Target |
|--------|------|--------|
| ✅ | ./docs/README.md | exists |
| ❌ | ./docs/MISSING.md | file not found |

### External Links
| Status | URL | Status Code |
|--------|-----|-------------|
| ✅ | https://example.com | 200 |
| ⚠️ | https://unreachable.com | timeout |
```
