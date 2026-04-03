---
name: dependency-mapper
description: Map and analyze dependencies between modules. Use when user says "map dependencies", "dependency analysis", or "what depends on what".
---

# Dependency Mapper

Maps and analyzes dependencies between modules and components.

## Dependency Types

| Type | Description |
|------|-------------|
| **Direct** | A directly uses B |
| **Indirect** | A uses B, B uses C |
| **Circular** | A uses B, B uses A |
| **Optional** | A may use B if available |
| **Temporal** | A must complete before B |

## Output Format

```markdown
## Dependency Analysis

### Dependency Graph
```
[Visual representation]
```

### Key Dependencies
| Component | Dependencies | Dependents |
|----------|--------------|------------|
| A | B, C | D, E |
| B | C | A |

### Critical Path
[Longest dependency chain]

### Problematic Dependencies
| Issue | Components | Recommendation |
|-------|------------|----------------|
| Circular | A ↔ B | Break cycle |
| High Coupling | A → B → C → D | Abstract interfaces |

### Recommendations
1. ...
```

## Issues to Flag
- **Circular dependencies** - A uses B, B uses A
- **Deep nesting** - Long dependency chains
- **Hub dependencies** - One component everyone depends on
- **Orphan components** - Nothing depends on them

## Analysis Questions
1. What does this component depend on?
2. What depends on this component?
3. What's the longest dependency chain?
4. Are there any circular dependencies?
5. What are the riskiest dependencies?
