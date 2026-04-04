
# Project Overview

This repository contains **design documents and specifications** for an AI Agent capability governance platform — building and governing AI capabilities (Skill, Subagent, Hook) with flexible integration methods (Plugin, Direct, etc.) for different Agent platforms.

**Note:** This repo does not contain implementation code. All content is design, documentation, and specification only.

---

## Branching Workflow

All changes MUST follow this workflow:

1. Create a new branch from `main` before making changes
2. Commit changes to the feature branch
3. Open a Pull Request to merge into `main`
4. Wait for CI checks and approval
5. Merge via PR — never push directly to `main`

**Never:**
- commit directly to `main`
- push directly to `main`
- merge PRs without CI passing

GitHub Branch Protection rules enforce this on the repository.

---

## Writing Conventions

### File Naming
- Use kebab-case for documentation files (e.g., `SKILL_AUTHORING.md`, `AGENT_CONFIGURATION.md`)
- Skill definition files use `SKILL.md` (uppercase)
- Subagent declarations use `agents/<id>.md`

### Documentation Structure
- Keep documentation modular — related content in the same doc
- Use tables for field/metadata reference
- Use mermaid diagrams for flow visualization
- Add Chinese translations for non-English content

### Terminology
- Use consistent terminology as defined in `docs/CONCEPTS.md`
- When in doubt, refer to `docs/CONCEPTS.md` before using alternatives

### Commit Messages
- Prefix with type: `feat:`, `fix:`, `docs:`, `refactor:`, `test:`, `chore:`
- Keep subject line under 72 characters
- Explain **why** the change was made, not just **what** changed