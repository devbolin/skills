---
name: phase-planner
description: Assist with phase and milestone planning. Use when user says "plan phases", "create roadmap", or "phase breakdown".
---

# Phase Planner

Assists with planning phases and milestones.

## Planning Framework

### Phase Structure

```markdown
## Phase N: [Phase Name]

**Objective**: [What this phase achieves]

### Key Deliverables
- [ ] Deliverable 1
- [ ] Deliverable 2

### Success Criteria
- Criterion 1
- Criterion 2

### Dependencies
- Dependency on previous phase
- External dependencies

### Timeline
- Start: [Date]
- End: [Date]
```

## Phase Planning Questions

1. What is the primary goal?
2. What are the deliverables?
3. What are the dependencies?
4. What defines success?
5. What could go wrong?

## Usage

Provide:
1. High-level objective
2. Known constraints
3. Available resources
4. Timeline expectations

Get:
1. Suggested phase breakdown
2. Deliverable清单
3. Risk assessment
4. Success criteria

## Output Format

```markdown
## Phase Plan

### Phase 1: [Name]
- **Goal**: ...
- **Deliverables**: ...
- **Duration**: ...
- **Success Criteria**: ...

### Phase 2: [Name]
...

### Dependencies
| From | To | Type |
|------|----|------|
| ... | ... | ... |

### Risks & Mitigations
| Risk | Mitigation |
|------|------------|
| ... | ... |
```
