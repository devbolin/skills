---
name: complexity-estimator
description: Estimate implementation complexity and resource needs. Use when user says "estimate complexity", "how hard is this", or "resource assessment".
---

# Complexity Estimator

Estimates implementation complexity and required resources.

## Estimation Dimensions

### 1. Technical Complexity
- **Low**: Well-understood, similar solutions exist
- **Medium**: New patterns, some unknowns
- **High**: Cutting-edge, many uncertainties

### 2. Scope
- **S**: < 1 day
- **M**: 1-3 days
- **L**: 1 week
- **XL**: 2+ weeks

### 3. Dependencies
- External services
- Team expertise
- Infrastructure needs

## Output Format

```markdown
## Complexity Estimation

### Technical Complexity: [Low/Medium/High]
### Estimated Scope: [S/M/L/XL]

### Factors
**Increase Complexity:**
1. ...

**Reduce Complexity:**
1. ...

### Resource Needs
| Resource | Estimate |
|----------|----------|
| Time | X days |
| People | Y |
| Skills | z |

### Risks
| Risk | Likelihood | Impact |
|------|-------------|---------|
| ... | ... | ... |
```
