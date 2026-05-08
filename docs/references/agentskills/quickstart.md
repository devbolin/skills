# Quickstart

> Create your first Agent Skill and see it work in VS Code.

## Prerequisites

- [VS Code](https://code.visualstudio.com/) with [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot)

## Create the skill

Create `.agents/skills/roll-dice/SKILL.md` in your project:

```markdown
---
name: roll-dice
description: Roll dice using a random number generator. Use when asked to roll a die (d6, d20, etc.), roll dice, or generate a random dice roll.
---

To roll a die, use the following command that generates a random number from 1
to the given number of sides:

```bash
echo $((RANDOM % <sides> + 1))
```

```powershell
Get-Random -Minimum 1 -Maximum (<sides> + 1)
```

Replace `<sides>` with the number of sides on the die (e.g., 6 for a standard
die, 20 for a d20).
```

## Try it out

1. Open your project in VS Code.
2. Open the Copilot Chat panel.
3. Select **Agent** mode.
4. Type `/skills` to confirm `roll-dice` appears.
5. Ask: **"Roll a d20"**

## How it works

1. **Discovery** — The agent scans skill directories and reads name + description.
2. **Activation** — The agent matches your question to the skill's description and loads the full `SKILL.md`.
3. **Execution** — The agent follows the instructions, adapting the command to your request.
