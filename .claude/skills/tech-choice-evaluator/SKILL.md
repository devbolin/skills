---
name: tech-choice-evaluator
description: Evaluate technology choices and compare solutions. Use when user says "evaluate tech choices", "compare solutions", or "which approach is better".
---

# Technology Choice Evaluator

Evaluates technology choices and compares alternative solutions.

## Evaluation Criteria

| Criteria | Weight | Description |
|----------|--------|-------------|
| **Maturity** | High | How proven is the technology? |
| **Fit** | High | Does it solve the problem well? |
| **Ecosystem** | Medium | Community, tools, documentation |
| **Maintenance** | High | Long-term support and updates |
| **Cost** | Medium | License, infrastructure, learning |

## Evaluation Matrix

```markdown
| Criteria | Weight | Option A | Option B | Option C |
|----------|--------|----------|----------|----------|
| Maturity | 30% | 8 | 6 | 5 |
| Fit | 30% | 7 | 8 | 6 |
| Ecosystem | 15% | 9 | 5 | 7 |
| Maintenance | 15% | 8 | 7 | 4 |
| Cost | 10% | 6 | 5 | 8 |
| **Total** | 100% | **7.6** | **6.7** | **5.7** |
```

## Output Format

```markdown
## Technology Evaluation

### Options Considered
1. **Option A** - Brief description
2. **Option B** - Brief description
3. **Option C** - Brief description

### Evaluation Matrix
[See matrix above]

### Recommended Choice: [Option]

### Rationale
[2-3 sentences explaining why]

### Trade-offs
| Choice | Pros | Cons |
|--------|------|------|
| Option A | ... | ... |
```

## When to Use
- Choosing between frameworks
- Selecting libraries
- Architecture decisions
- Platform selection
