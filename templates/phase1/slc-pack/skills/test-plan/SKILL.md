---
name: test-plan
description: Test planning skill for code changes and release verification. Activate when user says "test plan", "verification plan", "regression check", or "release testing".
license: "MIT"
metadata:
  version: "1.0"
  author: "slc-team"
  tags: ["testing", "test-plan", "regression", "verification"]
---

# Test Plan

Generate minimal but effective test plans for code changes.

## Use Cases

- User needs verification steps for a PR
- Pre-release regression testing needed
- Need risk-prioritized checklist
- Release validation planning

## Not Suitable For

- Direct execution of automated tests
- No change context to determine impact

## Core Capabilities

### Test Scope Definition
- Identify affected modules
- Determine test boundaries
- Define smoke, core, and edge test cases

### Risk-Based Prioritization
- Identify high-risk paths
- Focus on critical functionality
- Provide minimum viable test set

### Regression Planning
- Map dependencies
- Identify integration points
- Plan regression scope

## Usage

### Trigger
```
/test-plan --change "add user authentication"
/test-plan --scope full
```

### Input
- Change description or diff
- Affected modules
- Optional release window constraints

## Output Format Example

```markdown
## Test Plan: User Authentication Feature

### Scope
- Affected: auth-service, user-service
- Risk Level: Medium

### Smoke Tests
| ID | Test Case | Expected Result | Priority |
|----|-----------|-----------------|----------|
| SMOKE-001 | Login with valid credentials | Returns token | P0 |
| SMOKE-002 | Login with invalid credentials | Returns 401 | P0 |

### Core Regression Tests
| ID | Test Case | Expected Result | Priority |
|----|-----------|-----------------|----------|
| CORE-001 | Existing user login still works | No regression | P1 |
| CORE-002 | Password reset flow | Works as before | P1 |

### Edge Cases
| ID | Test Case | Expected Result | Priority |
|----|-----------|-----------------|----------|
| EDGE-001 | Concurrent login attempts | Handled gracefully | P2 |
| EDGE-002 | Session timeout | Redirects to login | P2 |

### Risk Notes
- Auth service is shared - coordinate with other teams
- Database migration may be required

### Verification Checklist
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] E2E smoke tests pass
- [ ] Performance benchmarks met
```

## Notes

- Provide minimum viable test set first
- Focus on new functionality and critical paths
- Identify dependencies and coordination needs
