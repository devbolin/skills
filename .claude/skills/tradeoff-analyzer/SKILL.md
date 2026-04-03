---
name: tradeoff-analyzer
description: Analyze trade-offs in design decisions. Use when user says "tradeoff analysis", "pros and cons", or "weigh options".
---

# Tradeoff Analyzer

Analyzes trade-offs in design decisions.

## Analysis Framework

### For Each Option, Consider

| Dimension | Questions |
|-----------|-----------|
| **Benefits** | What do we gain? |
| **Costs** | What do we pay? |
| **Risks** | What could go wrong? |
| **Complexity** | How much harder is this? |
| **Flexibility** | How adaptable is this? |

## Output Format

```markdown
## Tradeoff Analysis

### Options Considered

#### Option A: [Name]
| Dimension | Assessment |
|-----------|------------|
| Benefits | ... |
| Costs | ... |
| Risks | ... |
| Complexity | Low/Medium/High |
| Flexibility | ... |

#### Option B: [Name]
[Same structure]

### Comparison Summary

| Criteria | Option A | Option B |
|----------|---------|----------|
| Benefit | ... | ... |
| Cost | ... | ... |
| Risk | ... | ... |
| **Overall** | [Winner] | [Loser] |

### Recommendation
[Brief recommendation with rationale]
```

## Common Tradeoffs

| Decision | Tradeoff |
|----------|----------|
| Simplicity vs Flexibility | Simple solutions may not scale |
| Speed vs Quality | Fast delivery vs Maintainability |
| Standard vs Custom | Standards vs Optimization |
| Coupling vs Simplicity | Loose coupling adds complexity |
