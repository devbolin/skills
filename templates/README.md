# 模板指南

本目录包含 Phase 1 Plugin-first 模板。

## Pack 概览

Phase 1 提供以下 Pack：

| Pack | 用途 | Skills | Agents |
|------|------|--------|--------|
| `skills-devtools` | 开发者工具集 | code-review, pr-summary, test-plan | review-coordinator |
| `slc-pack` | 软件开发生命周期 | requirements, architecture-design, code-implementation, code-review, test-plan, deployment, operations, documentation | requirements-analyst, architect, developer, reviewer, test-engineer, sre, tech-writer |
| `skill-toolkit-pack` | Skill/Agent 管理工具包 | create-skill, update-skill, validate-skill, install-skill, create-agent, update-agent | skill-manager |

## 目录结构

```text
templates/phase1/                          # 单一仓库（Phase 1 验证用）
├── .github/workflows/                     # CI/Release 配置
│   ├── ci.yml                           # PR/合并校验：YAML 语法、pack.yaml 结构、SKILL.md 入口
│   └── release.yml                       # Tag 触发：plugin artifact + manifest + checksum + catalog 更新
├── catalog/                              # 发布索引（聚合所有 pack）
│   ├── index.json                       # catalog 入口
│   └── skills/                          # skill 版本详情
├── skills-devtools/                      # pack 示例：开发者工具集
│   ├── pack.yaml                         # pack 真相源
│   ├── agents/                           # Subagent 声明
│   │   └── review-coordinator.md
│   └── skills/                           # Skill 定义
│       ├── code-review/SKILL.md
│       ├── pr-summary/SKILL.md
│       └── test-plan/SKILL.md
└── slc-pack/                             # pack：软件开发生命周期
    ├── pack.yaml                         # pack 真相源
    ├── agents/                           # Agent 声明
    │   ├── requirements-analyst.md
    │   ├── architect.md
    │   ├── developer.md
    │   ├── reviewer.md
    │   ├── test-engineer.md
    │   ├── sre.md
    │   └── tech-writer.md
    └── skills/                           # Skill 定义
        ├── requirements/SKILL.md
        ├── architecture-design/SKILL.md
        ├── code-implementation/SKILL.md
        ├── code-review/SKILL.md
        ├── test-plan/SKILL.md
        ├── deployment/SKILL.md
        ├── operations/SKILL.md
        └── documentation/SKILL.md
└── skill-toolkit-pack/                    # pack：Skill/Agent 管理工具
    ├── pack.yaml                          # pack 真相源
    ├── agents/                            # Agent 声明
    │   └── skill-manager.md
    └── skills/                            # Skill 定义
        ├── create-skill/SKILL.md
        ├── update-skill/SKILL.md
        ├── validate-skill/SKILL.md
        ├── install-skill/SKILL.md
        ├── create-agent/SKILL.md
        └── update-agent/SKILL.md
```

## 使用方法

### 1. 选择或创建 Pack

**使用现有 Pack**：
```bash
# 复制 skills-devtools
cp -r templates/phase1/skills-devtools my-devtools-pack

# 复制 slc-pack
cp -r templates/phase1/slc-pack my-slc-pack

# 复制 skill-toolkit-pack
cp -r templates/phase1/skill-toolkit-pack my-skill-toolkit
```

**创建新 Pack**：
参考现有 pack 结构创建新目录，包含 `pack.yaml`、`agents/` 和 `skills/`。

### 2. 维护 Pack 清单

编辑 `pack.yaml`：
- `id`: 唯一标识符
- `distribution.default`: plugin
- `distribution.enable_skill_artifacts`: false
- `skills[]`: 声明 pack 包含的 skill
- `agents[]`: 声明 pack 包含的 agent

### 3. 维护 Skill 文档

- 编辑 `skills/<skill-id>/SKILL.md`
- 每个 skill 独立目录，包含 SKILL.md 和可选的 `adapters/`、`scripts/`、`references/`

### 4. 维护 Agent 文档

- 编辑 `agents/<agent-id>.md`
- 使用 YAML frontmatter（`name`/`description`/`tools`/`model`）+ Markdown 正文格式

### 5. 发布

- 打 tag 触发 `release.yml`
- 默认仅生成 plugin artifact
- 可选开启单 Skill artifact

## 详细规范

- [Phase 1 设计文档](../docs/phase1/DESIGN.md) - 架构决策与边界
- [Phase 1 流程文档](../docs/phase1/FLOW.md) - 发布/调用/回滚流程
- [Phase 1 配置手册](../docs/phase1/CONFIG.md) - pack.yaml 字段详解
- [人类视角工作流](../docs/phase1/HUMAN_WORKFLOW.md) - 完整操作步骤
- [Agent 配置指南](../docs/guides/agent-configuration.md) - 如何在 Agent 侧集成
