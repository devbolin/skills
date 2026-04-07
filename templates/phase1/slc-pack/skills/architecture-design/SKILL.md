---
name: architecture-design
description: Architecture design and technical decision skill. Activate when user says "architecture design", "system design", "technical proposal", "ADR", "architecture review", or "design review".
version: "1.0"
author: "slc-team"
license: "MIT"
tags: ["architecture", "design", "ADR", "technical-decision"]
---

# Architecture Design

System architecture design and technical decision-making, balancing short-term requirements and long-term maintainability.

## Use Cases

- New project initiation, system architecture design
- Technology selection evaluation, comparing options
- Architecture review, preparing review materials
- Major refactoring decisions, impact assessment

## Not Suitable For

- Simple feature development with clear architecture
- Emergency bug fixes
- Code style or small-scale refactoring

## Core Capabilities

### Architecture Design
Design clear, scalable system architecture including component division and data flow.

**Method:**
1. Understand business requirements and constraints
2. Determine architectural style (layered, microservices, etc.)
3. Define components and responsibilities
4. Design data flow and interfaces

### Technology Selection
Evaluate trade-offs of different technical options and provide recommendations.

**Method:**
1. List candidate solutions
2. Evaluate from feasibility, cost, risk perspectives
3. Consider team capabilities
4. Provide clear recommendations

### ADR Authoring
Record key architectural decisions and their rationale to form a decision log.

**ADR Format:**
```
## ADR-001: [Decision Title]

**Status**: Accepted

**Context**: [Problem description]

**Decision**: [Chosen solution]

**Consequences**: [Positive/Negative]
```

## Usage

### Trigger
```
/architecture-design --action design
/architecture-design --action evaluate
/architecture-design --action adr
```

### Input
- Business requirements or functional specifications
- Constraints (performance, security, cost, etc.)

## Output Format Example

```markdown
## Architecture Overview
[High-level description of system architecture]

## Technology Stack

| Component | Choice | Rationale |
|-----------|--------|------------|
| Frontend | React | Team expertise |
| Backend | FastAPI | Performance |

## System Components

## ADR

### ADR-001: Use PostgreSQL instead of MongoDB
**Status**: Accepted
**Context**: Need strong consistency
**Decision**: Use PostgreSQL
**Consequences**: Need to manage schema migrations

## Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| Scalability bottleneck | High |预留分片设计 |
```
