---
name: requirements
description: Requirements analysis and specification skill. Activate when user says "analyze requirements", "write requirements", "User Story", "PRD", "SRS", or "gather requirements".
license: "MIT"
metadata:
  version: "1.0"
  author: "slc-team"
  tags: "requirements, analysis, user-story, PRD"
---

# Requirements

Transform ambiguous business requirements into clear functional specifications.

## Use Cases

- Project initiation, requirements gathering
- Requirements review meetings
- Requirement change requests, impact assessment
- User story breakdown, acceptance criteria definition

## Not Suitable For

- Implementation issues with clear technical solutions
- Emergency bug fixes (no requirements analysis needed)
- Code optimization (no requirement changes)

## Core Capabilities

### Requirements Gathering
Clarify ambiguous requirements through questioning, identify stakeholder goals.

**Method:**
1. Identify stakeholders
2. Use 5 Whys and SMART principles
3. Document assumptions and constraints

### Requirements Structuring
Break down into User Stories, functional lists, and non-functional requirements.

**Method:**
1. Prioritize using MoSCoW
2. Identify dependencies between requirements
3. Define testable acceptance criteria

### Acceptance Criteria Definition
Define testable acceptance criteria for each requirement.

## Usage

### Trigger
```
/requirements --action analyze
/requirements --action review
```

### Input
- Project background or business objectives
- Related documents or existing descriptions

## Output Format Example

```markdown
## Requirements Overview
[Project background and objectives]

## Functional Requirements

| ID | Requirement | Priority | Acceptance Criteria |
|----|-------------|----------|---------------------|
| REQ-001 | User login | P0 | Lock after 3 failures |

## Non-Functional Requirements
- Performance: Response time < 200ms
- Security: Password stored encrypted

## Dependencies & Constraints
[List technical, business, or external dependencies]

## Open Questions
- [ ] Question 1
- [ ] Question 2
```

## Notes

- Requirements should be independent, avoid interdependencies
- Acceptance criteria must be specific and measurable
- Prioritization should consider business value and implementation cost
