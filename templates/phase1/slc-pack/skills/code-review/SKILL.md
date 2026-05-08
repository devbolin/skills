---
name: code-review
description: Code review skill for best practices, security, and performance. Activate when user says "code review", "review code", "PR review", or "analyze pull request".
license: "MIT"
metadata:
  version: "1.0"
  author: "slc-team"
  tags: "code-review, security, quality, best-practices"
---

# Code Review

Automatically review code quality, security, and best practices.

## Use Cases

- User submits PR and requests review
- User asks to check code for potential issues
- Pre-merge security scanning
- Code review for specific files or diff

## Not Suitable For

- Emergency hotfixes requiring quick merge (use exception process)
- Pure documentation changes (no technical review needed)
- Known issues already being tracked (avoid duplicate review)

## Core Capabilities

### Code Quality Review
- Readability and maintainability
- Naming conventions
- Function complexity

### Security Check
- SQL/NoSQL injection risks
- XSS/CSRF vulnerabilities
- Sensitive data exposure

### Performance Review
- Loop efficiency
- Database query optimization
- Cache usage

## Usage

### Trigger
```
/code-review --file src/auth.py
/code-review --diff HEAD~1
```

### Input
- Target file or diff context
- Review scope (full file or specific changes)

## Output Format Example

```markdown
## Review Summary

| File | Severity | Issue | Line | Suggestion |
|------|----------|-------|------|------------|
| src/auth.py | High | SQL injection risk | 42 | Use parameterized queries |
| src/auth.py | Medium | Missing error handling | 78 | Add try-catch block |
| src/auth.py | Low | Naming inconsistency | 15 | Rename to camelCase |

## Metrics
| Category | Count |
|----------|-------|
| Critical | 0 |
| High | 1 |
| Medium | 2 |
| Low | 3 |

## Recommendations
1. Fix high severity issues before merge
2. Address medium issues in follow-up PR
3. Consider refactoring long functions
```

## Notes

- Focus on actionable feedback
- Prioritize security and correctness over style
- Provide specific code suggestions when possible
