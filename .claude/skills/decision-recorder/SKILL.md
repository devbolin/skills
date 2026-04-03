---
name: decision-recorder
description: Record architecture decisions and alternatives. Use when user says "record decision", "create ADR", or "document decision".
---

# Decision Recorder

Records architecture decisions in ADR (Architecture Decision Record) format.

## ADR Format

```markdown
# ADR-XXX: [Title]

## Status
[Proposed | Accepted | Deprecated | Superseded by ADR-YYY]

## Context
[What is the issue that is being decided?]

## Decision
[What is the decision that was made?]

## Alternatives Considered
1. **[Option]** - Brief description
   - Pros: ...
   - Cons: ...

2. **[Option]** - Brief description
   - Pros: ...
   - Cons: ...

## Consequences
### Positive
- ...

### Negative
- ...

## Related Decisions
- ADR-XXX: [Related decision]
```

## When to Create ADR
- Significant design decisions
- Technology choices
- Architecture changes
- Trade-offs with major impact

## Usage

1. Identify the decision to record
2. Gather context and rationale
3. Document alternatives considered
4. Note consequences
5. Assign ADR number

## Numbering Convention
- Start with ADR-001
- Increment sequentially
- Never reuse numbers
