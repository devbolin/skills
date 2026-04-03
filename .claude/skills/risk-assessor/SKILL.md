---
name: risk-assessor
description: Identify and assess project risks. Use when user says "identify risks", "risk assessment", or "what could go wrong".
---

# Risk Assessor

Identifies and assesses project risks.

## Risk Categories

| Category | Examples |
|----------|----------|
| **Technical** | Complexity, integration, performance |
| **Resource** | Budget, team, time |
| **External** | Dependencies, market, regulations |
| **Operational** | Support, maintenance, adoption |

## Assessment Matrix

| Likelihood | Impact | Risk Level |
|------------|--------|------------|
| High | High | **Critical** |
| High | Low | Medium |
| Low | High | Medium |
| Low | Low | Low |

## Output Format

```markdown
## Risk Assessment

### Critical Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| ... | High | High | ... |

### Medium Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| ... | Medium | Medium | ... |

### Low Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| ... | Low | Low | Monitor |

### Risk Summary
- **Total Risks**: N
- **Critical**: N
- **Mitigation Plan**: [Summary]
```

## Risk Questions
1. What could fail?
2. How likely is it?
3. What's the impact?
4. How can we mitigate?
5. What's the contingency?
