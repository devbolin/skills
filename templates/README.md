# 模板指南

本目录包含阶段一 Plugin-first 模板。

## 目录结构

```text
templates/phase1/                    # 单一仓库（Phase 1 验证用）
├── .github/workflows/               # CI/Release
├── catalog/                        # 发布索引（聚合所有 pack）
│   ├── index.json                 # catalog 入口
│   └── skills/                    # skill 版本详情
│       ├── code-review.json
│       ├── pr-summary.json
│       └── test-plan.json
└── skills-devtools/               # pack 示例
    ├── pack.yaml                   # 这个 pack 的真相源
    ├── agents/
    │   └── review-coordinator.md
    └── skills/
        ├── code-review/SKILL.md
        ├── pr-summary/SKILL.md
        └── test-plan/SKILL.md
```

## 使用方法

### 1. 创建领域仓库
```bash
cp -r templates/phase1/skills-devtools my-skills-<domain>
```

### 2. 维护 Pack 清单
编辑 `pack.yaml`：
- `distribution.default: plugin`
- `distribution.enable_skill_artifacts: false`
- `skills[]` 中的入口和适配器声明

### 3. 维护 Skill 文档
- 编辑 `skills/<skill-id>/SKILL.md`
- 按需维护 `adapters/` 与 `scripts/`

### 4. 发布
- 打 tag 触发 `release.yml`
- 默认仅生成 plugin artifact
- 可选开启单 Skill artifact

## 详细规范

- [阶段一设计文档](../docs/phase1/DESIGN.md) - 架构决策与边界
- [阶段一流程文档](../docs/phase1/FLOW.md) - 发布/调用/回滚流程
- [阶段一配置手册](../docs/phase1/CONFIG.md) - pack.yaml 字段详解
- [人类视角工作流](../docs/phase1/HUMAN_WORKFLOW.md) - 完整操作步骤
- [Agent 配置指南](../docs/guides/AGENT_CONFIGURATION.md) - 如何在 Agent 侧集成
