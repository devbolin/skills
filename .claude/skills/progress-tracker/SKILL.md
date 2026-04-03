---
name: progress-tracker
description: Track phase and sprint completion status. Use when user says "track progress", "update status", or "completion status".
---

# Progress Tracker

Tracks progress of phases, milestones, and deliverables.

## Tracking Dimensions

| Dimension | Description |
|-----------|-------------|
| **Completion** | % of deliverables complete |
| **Quality** | Meets acceptance criteria? |
| **Timeline** | On schedule? |
| **Resources** | Budget/time spent |

## Status Levels

| Status | Meaning |
|--------|---------|
| **Not Started** | 0% - Work has not begun |
| **In Progress** | 1-99% - Work started |
| **Complete** | 100% - Delivered |
| **Blocked** | 0% - Cannot proceed |
| **At Risk** | Likely to miss deadline |

## Output Format

```markdown
## Progress Report

### Phase 1: [Name]
| Deliverable | Status | Completion | Notes |
|-------------|--------|------------|-------|
| Deliverable 1 | ✅ Complete | 100% | - |
| Deliverable 2 | 🔄 In Progress | 60% | - |
| Deliverable 3 | ⚠️ At Risk | 40% | Blocked on X |

### Overall Progress: [XX%]

### Timeline Status
| Milestone | Target | Actual | Status |
|-----------|--------|--------|--------|
| M1 | Date | Date | ✅ On Track |

### Blockers
1. [Blocker description] - Impact: [High/Med/Low]

### Upcoming
- [ ] Next deliverable
- [ ] Upcoming milestone
```

## Update Format

When updating status, include:
1. What changed
2. What's the new status
3. What's the impact
4. Any new blockers
