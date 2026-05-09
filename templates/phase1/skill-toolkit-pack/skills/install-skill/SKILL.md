---
name: install-skill
description: Use this skill to install Agent Skills from a catalog, registry, or direct download URL. Activate when the user needs to deploy a skill pack, download plugin artifacts, or set up skills from a remote source. Triggers: install skill, download skill, add skill from catalog, install pack, deploy skill.
license: "MIT"
metadata:
  version: "1.0"
  author: "skill-toolkit-team"
  tags: "installation, catalog, registry, download"
compatibility: "Requires network access to download artifacts from GitHub Releases or a registry."
---

# Install Skill

Install a skill or pack from a catalog, registry, or direct source by resolving `plugin_ref` or `skill_ref` and deploying to a target directory.

## Use Cases

- Installing a skill from a local or remote catalog
- Downloading and setting up a plugin artifact
- Installing a single skill artifact (when `enable_skill_artifacts` is enabled)
- Verifying the installation is complete and valid
- Listing available skills in a catalog

## Not Suitable For

- Creating new skills (use create-skill)
- Validating installed skills (use validate-skill)
- Removing or uninstalling skills

## Workflow

### Step 1: Determine Source

Identify where to find the skill:

| Source | How |
|--------|-----|
| Local catalog | `catalog/index.json` in the current repository |
| Remote catalog | URL to a `catalog/index.json` |
| Direct artifact | URL or path to a plugin zip or SKILL.md |
| GitHub Release | `https://github.com/<org>/<repo>/releases/download/<tag>/<artifact>` |

### Step 2: Look Up the Skill

For catalog-based installation, read `catalog/index.json` and find the target by `skill_id` or `pack_id`.
Decide installation mode:
- Plugin (default): use `plugin_ref` for full pack
- Single Skill: use `skill_ref` when available and requested

### Step 3: Download Artifact

For plugin installation:
```bash
# Download plugin artifact
curl -L -o pack.zip <plugin_ref>

# Or use a direct GitHub Release URL
curl -L -o pack.zip https://github.com/<org>/<repo>/releases/download/v1.0.0/<pack-id>-1.0.0-plugin.zip
```

### Step 4: Extract to Target Directory

```bash
# Create target directory
mkdir -p /opt/skills/plugins/<pack-id>/<version>

# Extract plugin
unzip pack.zip -d /opt/skills/plugins/<pack-id>/<version>

# Verify structure
ls /opt/skills/plugins/<pack-id>/<version>/pack.yaml
ls /opt/skills/plugins/<pack-id>/<version>/skills/
```

### Step 5: Verify Installation

```bash
# Check required files exist
ls <target-dir>/pack.yaml
ls <target-dir>/skills/<skill-id>/SKILL.md

# Verify catalog entry points to valid artifact
cat <target-dir>/pack.yaml
```

If verification fails, fix the issues and re-verify before proceeding.

### Step 6: Confirm

Summarize what was installed and ask for confirmation.

### Source
- **Type**: Plugin artifact
- **Pack**: devtools-pack
- **Version**: 1.0.0
- **Source**: catalog/index.json -> releases/devtools-pack-1.0.0-plugin.zip

### Target
- **Directory**: /opt/skills/plugins/devtools-pack/1.0.0

### Installed Skills
| Skill ID | Path | Status |
|----------|------|--------|
| code-review | skills/code-review/SKILL.md | ✅ |
| pr-summary | skills/pr-summary/SKILL.md | ✅ |
| test-plan | skills/test-plan/SKILL.md | ✅ |

### Installed Agents
| Agent ID | Path | Status |
|----------|------|--------|
| review-coordinator | agents/review-coordinator.md | ✅ |

### Verification
- [x] pack.yaml exists
- [x] All skill entries have valid SKILL.md
- [x] All agent entries have valid .md files
```

## Gotchas

- Plugin installation is the default mode; single-skill installation is only available when the catalog explicitly provides `skill_ref`.
- Network access is required for remote installations. Check `compatibility` field in frontmatter for network requirements.
- Always verify the downloaded artifact has a valid structure (SKILL.md, pack.yaml) before extracting.
- Keep versioned directories to support rollback. Overwriting an existing version loses the previous one.
- If `checksums` are provided in the catalog, verify them after download.

## Workflow Checklist

- [ ] Step 1: Determine source (catalog, direct URL, GitHub Release)
- [ ] Step 2: Look up skill/pack in catalog
- [ ] Step 3: Download artifact
- [ ] Step 4: Verify artifact integrity (checksum if available)
- [ ] Step 5: Extract to target directory
- [ ] Step 6: Verify installed structure (pack.yaml, SKILL.md)
- [ ] Step 7: Confirm installation with report
