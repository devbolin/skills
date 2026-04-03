---
name: design-pattern-advisor
description: Recommend appropriate design patterns and architecture decisions. Use when user says "what design pattern", "architecture suggestion", or "best practice".
---

# Design Pattern Advisor

Recommends design patterns and best practices for given problems.

## Common Patterns

### For AI Agent Systems

| Pattern | Use Case | Description |
|---------|----------|-------------|
| **Skill Pattern** | Reusable capability | Define once, use multiple times |
| **Subagent Pattern** | Complex task decomposition | Delegate to specialized agents |
| **Adapter Pattern** | Multi-platform support | Translate between interfaces |
| **Catalog Pattern** | Service discovery | Index and locate capabilities |
| **Plugin Pattern** | Packaging & distribution | Bundle capabilities for sharing |

### For Architecture

| Pattern | Use Case |
|---------|----------|
| **Facade** | Simplify complex interfaces |
| **Strategy** | Swap algorithms at runtime |
| **Observer** | Event-driven communication |
| **Pipeline** | Sequential processing |
| **Gateway** | Unified API entry point |

## Usage

1. Understand the problem context
2. Identify constraints and requirements
3. Match to applicable patterns
4. Provide recommendations with rationale

## Output Format

```markdown
## Design Pattern Recommendation

### Context
[Problem description]

### Recommended Patterns
1. **[Pattern Name]**
   - Why: ...
   - How: ...
   - Example: ...

### Alternative Patterns
1. **[Alt Pattern]** - Brief why not

### Implementation Notes
- ...
```

## Anti-Patterns to Avoid
- Over-engineering
- Premature optimization
- God objects/services
- Tight coupling
