---
name: architecture-evaluator
description: Evaluate architecture design for technical feasibility and risks. Use when user says "evaluate architecture", "design review", or "assess feasibility".
---

# Architecture Evaluator

Evaluates architecture designs for technical soundness and potential issues.

## Evaluation Dimensions

### 1. Technical Feasibility
- Can it be implemented with current technology?
- Are dependencies achievable?
- What's the implementation complexity?

### 2. Scalability
- Will it handle expected load?
- What are the scaling characteristics?
- Any resource constraints?

### 3. Maintainability
- How easy is it to maintain?
- What's the operational complexity?
- Are there vendor lock-in risks?

### 4. Risk Assessment
- Technical risks
- Integration risks
- Operational risks

## Usage

Provide architecture description. Evaluation will include:

1. **Strengths** - What's good about this design
2. **Weaknesses** - Potential issues
3. **Risks** - Technical and operational risks
4. **Recommendations** - Suggested improvements

## Output Format

```markdown
## Architecture Evaluation

### Summary
[One paragraph overview]

### Technical Feasibility: [High/Medium/Low]
### Scalability: [High/Medium/Low]
### Maintainability: [High/Medium/Low]

### Strengths
1. ...

### Concerns
1. ...

### Risks
| Risk | Severity | Mitigation |
|------|----------|------------|

### Recommendations
1. ...
```
