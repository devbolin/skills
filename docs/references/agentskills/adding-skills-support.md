---
source: https://agentskills.io/docs/adding-skills-support
retrieved: 2026-05
type: standard
---

# How to add skills support to your agent

> A guide for adding Agent Skills support to an AI agent or development tool.

## The core principle: progressive disclosure

| Tier            | What's loaded               | When                                 | Token cost                  |
| --------------- | --------------------------- | ------------------------------------ | --------------------------- |
| 1. Catalog      | Name + description          | Session start                        | ~50-100 tokens per skill   |
| 2. Instructions | Full `SKILL.md` body        | When the skill is activated          | <5000 tokens (recommended) |
| 3. Resources    | Scripts, references, assets | When the instructions reference them | Varies                      |

## Step 1: Discover skills

### Where to scan

| Scope   | Path                               | Purpose                       |
| ------- | ---------------------------------- | ----------------------------- |
| Project | `<project>/.<your-client>/skills/` | Your client's native location |
| Project | `<project>/.agents/skills/`        | Cross-client interoperability |
| User    | `~/.<your-client>/skills/`         | Your client's native location |
| User    | `~/.agents/skills/`                | Cross-client interoperability |

### What to scan for

Look for subdirectories containing a file named exactly `SKILL.md`.

### Handling name collisions

Project-level skills override user-level skills.

### Trust considerations

Gate project-level skill loading on a trust check.

## Step 2: Parse `SKILL.md` files

Extract YAML frontmatter and body content. Handle malformed YAML gracefully. Warn on issues but load when possible.

## Step 3: Disclose available skills

Tell the model what skills exist without loading their full content. Include name, description, and optionally location.

Place the catalog in the system prompt or in a dedicated tool description. Include behavioral instructions telling the model how to use skills.

## Step 4: Activate skills

Two patterns:

- **File-read activation**: The model reads the `SKILL.md` path from the catalog.
- **Dedicated tool activation**: Register a tool (e.g., `activate_skill`) that takes a skill name and returns the content.

Also support user-explicit activation via slash command or mention syntax.

## Step 5: Manage skill context over time

- Protect skill content from context compaction.
- Deduplicate activations.
- Optionally support subagent delegation.
