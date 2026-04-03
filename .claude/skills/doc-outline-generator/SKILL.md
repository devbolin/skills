---
name: doc-outline-generator
description: Generate standardized document outlines. Use when user says "help me write a doc", "generate document structure", or "how to organize this document".
---

# Document Outline Generator

Generates standardized document structures based on document type and purpose.

## Supported Document Types

### SKILL.md - Skill Definition
```markdown
# Structure
---
name: skill-name
description: Clear description of when to activate
---

# Skill Name

## Use Cases
## Non-Use Cases
## Usage
## Notes
```

### Design Document - Architecture, Solution Design
```markdown
# Structure
## Background & Problem
## Design Goals
## Solution Description
## Tradeoff Analysis
## Implementation Plan
## Acceptance Criteria
```

### Flow Document - Workflow, Operation Steps
```markdown
# Structure
## Overview
## Prerequisites
## Steps
## Verification
## Rollback
```

### Specification Document - Coding Standards, Commit Standards
```markdown
# Structure
## Purpose
## Scope
## Specifications
## Examples
## Checklist
```

## Usage

User provides:
1. Document type
2. Document purpose
3. Key content points

Generate corresponding outline structure.
