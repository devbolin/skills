# Project Overview

This repository contains **design documents and specifications** for an AI Agent capability governance platform — building and governing AI capabilities (Skill, Subagent, Hook) with flexible integration methods (Plugin, Direct, etc.) for different Agent platforms.

**Note:** This repo does not contain implementation code. All content is design, documentation, and specification only.

---

## Branching Workflow

**Before starting ANY work:** Check CHANGELOG.md to see if it needs updating for the current or upcoming version.

All changes MUST follow this workflow:

0. **Check CHANGELOG.md** — Run `grep -n "v2\." CHANGELOG.md | head -3` to confirm current version state
1. Create a new branch from `main` before making changes
2. Commit changes to the feature branch
3. Update CHANGELOG.md with new version entry (if applicable)
4. Open a Pull Request to merge into `main`
5. Wait for CI checks and approval
6. Merge via PR — never push directly to `main`

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

---

## Commit Messages

### Format

```
type: Subject line (under 72 chars)

Optional body explaining WHY the change was made.
Keep lines under 80 characters.

Footer: Refs #issue-number, Closes #pr-number
```

### Type Prefixes

| Prefix | Use Case |
|--------|----------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation changes |
| `refactor:` | Code refactoring |
| `test:` | Test related |
| `chore:` | Build/tool changes |

### Example

```
docs: add CI validation for pack.yaml fields

Why: Phase 1 requires automated validation before merge.
This adds YAML lint and field completeness checks.

Refs: #1
```

---

## Pull Request Review

### Prerequisites for Merge

- [ ] All CI checks pass
- [ ] At least 1 approval
- [ ] No unresolved conversations
- [ ] Commit messages follow规范
- [ ] CHANGELOG.md updated

### Reviewer Responsibilities

1. Check content aligns with project goals
2. Verify documentation structure/format compliance
3. Confirm no security issues introduced
4. Ensure changes have corresponding CHANGELOG entry

### PR Description Requirements

- **Summary**: Change overview
- **Test plan**: Verification steps
- **Related issues**: Linked documentation

---

## Task 使用规范

### When to Create Tasks

- Tasks involving 3+ independent steps
- Tasks requiring progress tracking or dependencies
- When user explicitly asks to "track task"

### Status Flow

```
pending → in_progress → completed
```

Use `deleted` to permanently remove a task.

### Task Dependencies

Use `addBlockedBy` / `addBlocks` to establish dependencies between tasks.

### Example

```
创建: TaskCreate (subject, description, activeForm)
更新: TaskUpdate (taskId, status, owner)
依赖: TaskUpdate (taskId, addBlockedBy: ["task-id"])
```

---

## Memory 使用指南

### When to Save Memories

- User explicitly asks to remember something
- Discover user preferences or feedback (positive or negative)
- Project state changes (deadline, stakeholder, constraint)

### Memory Types

| Type | Purpose | Example |
|------|---------|---------|
| `user` | User role/preferences | "用户是数据科学家" |
| `feedback` | Team feedback/rules | "不要 mock 数据库" |
| `project` | Project state/decisions | "merge freeze 4/5" |
| `reference` | External resource pointers | "Linear 项目 INGEST" |

### File Location

- Memory directory: `~/.claude/projects/<project>/memory/`
- Must create `MEMORY.md` index
- Each memory as a separate `.md` file

### When to Use

- Check `MEMORY.md` at start of new conversation
- Review relevant memories when encountering ambiguous requirements
- Periodically clean up stale memories

---

## CHANGELOG 更新规范

### When to Update

CHANGELOG.md must be updated before each PR merge.

### Format (Keep a Changelog 1.0.0)

```markdown
## [version] - YYYY-MM-DD

### Added
- New features

### Changed
- Feature changes

### Deprecated
- Soon-to-be removed features

### Removed
- Removed features

### Fixed
- Bug fixes

### Security
- Security-related changes
```

### Version Numbering

| Version Type | When to Use |
|--------------|-------------|
| v1.0, v2.0 | Major structural changes |
| v1.1, v2.1 | New features or significant changes |
| v1.0.1 | Minor fixes/documentation updates |

### Pre-Merge Checklist

- [ ] CHANGELOG.md updated
- [ ] Change type correctly classified
- [ ] Date in YYYY-MM-DD format
