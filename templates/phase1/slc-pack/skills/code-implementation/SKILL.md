---
name: code-implementation
description: Code implementation and programming skill. Activate when user says "write code", "implement feature", "generate code", "code completion", or "refactor code".
version: "1.0"
author: "slc-team"
license: "MIT"
tags: ["code", "implementation", "refactor", "programming"]
---

# Code Implementation

Code implementation and refactoring, transforming designs into working code.

## Use Cases

- Implementing feature code based on requirements
- Code refactoring (with clear refactoring goals)
- Generating boilerplate code or templates
- Code completion and optimization suggestions

## Not Suitable For

- Pure concept discussion or algorithm design (before implementation stage)
- Large-scale architecture changes (should follow architecture design process)
- Code review (should use code-review skill)

## Core Capabilities

### Feature Implementation
Implement feature code based on requirements and design.

**Method:**
1. Understand requirement specifications
2. Choose appropriate code location
3. Follow coding standards
4. Write necessary comments

### Code Refactoring
Identify code smells and provide refactoring solutions.

**Method:**
1. Identify refactoring points (duplication, complexity, coupling)
2. Write tests to protect behavior
3. Make small, incremental changes
4. Verify tests pass

### Unit Testing
Write unit tests for implemented functionality.

## Usage

### Trigger
```
/code-implementation --file path/to/file --action implement
/code-implementation --function function-name --action refactor
```

### Input
- Functional requirements or design specifications
- Target file or module path

## Output Format Example

```markdown
## Implementation Summary
[Brief description of implemented functionality]

## Code Changes

### src/auth.py
```python
# New function
def authenticate(username: str, password: str) -> User:
    """Validate user credentials"""
    # Implementation
    pass
```

## Dependency Changes
```toml
# New dependency
package = "bcrypt"
```

## Test Plan
| Test Case | Input | Expected Output | Status |
|-----------|-------|----------------|--------|
| test_auth_success | Valid credentials | Returns user | ✅ |
| test_auth_failure | Wrong password | Raises exception | ✅ |
```

## Notes

- Follow project coding standards
- Include necessary comments in code
- Ensure code is testable
