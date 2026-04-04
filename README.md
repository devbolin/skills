
# 跨平台 AI Agent 能力管理

> 版本：v3.0
> 更新：2026-04-03

让 AI Agent 的能力可以被多个平台复用和共享。

基于 GitHub 管理能力生命周期，支持多种集成方式（Plugin、Direct 等），适配不同 Agent 平台。

## 当前规范（Normative）
- 阶段一以 `pack.yaml` 作为仓库级唯一清单文件。
- 默认分发模式为 **Plugin-first（整仓分发）**。
- 保留单 Skill 分发能力，但默认关闭，仅作为可选模式。
- `skill.yaml` 不作为手工维护真相源，仅可作为生成产物。
- **Polyrepo 目录模型保留**：`catalog/` 与多个 domain pack（如 `skills-*`）并存。

> 当前权威规范文档：`docs/phase1/`。

---

## 快速开始

- [阶段一设计](./docs/phase1/DESIGN.md) - Plugin-first 架构
- [阶段一流程](./docs/phase1/FLOW.md) - 开发到发布流程
- [阶段一配置](./docs/phase1/CONFIG.md) - pack.yaml 与发布配置
- [Agent 配置指南](./docs/guides/AGENT_CONFIGURATION.md) - Agent 侧 plugin 消费与配置步骤
- [技能编写指南](./docs/guides/SKILL_AUTHORING.md) - 如何编写 SKILL.md

---

## 核心概念

> 详细概念解释见 [docs/CONCEPTS.md](./docs/CONCEPTS.md)

### Skill（技能）
- 单个可复用的 AI 能力，如"代码审查"、"发票识别"
- Agent 可自动检测何时使用

### Subagent（任务代理）
- 专门处理特定任务的 AI 助手，运行在独立上下文
- 与主 Agent 协作，处理复杂子任务
- 在 Pack 内以 `agents/<id>.md` 形式声明，并由 `pack.yaml` 的 `agents[]` 索引

### Hook（自动化钩子）
- Agent 生命周期事件触发点，如任务开始、结束时的自动处理
- 可用于日志记录、上下文收集、结果格式化等

### Pack（能力包）
- 包含 Skill、Subagent、Hook 等多种能力的集合
- 打包发布后，多个 Agent 都能安装使用

### Polyrepo
- 以 domain pack 为边界拆分仓库与模板，不是单一 skill 仓库
- phase1 模板目录同时包含：catalog、多个 pack 示例

---

## 仓库结构

本仓库包含两类不同用途的 Skill：

### `.claude/skills/` - 本地能力（本文档仓库自用）

本文档仓库（skill-management）自身使用的 Claude Code 能力，无需 `pack.yaml` 或发布流程，直接供本仓库的 Claude Code 使用。

```text
.claude/skills/                # 本地 Skill（18 个）
├── architecture-evaluator/
├── complexity-estimator/
├── decision-recorder/
└── ...（共 18 个）
```

**用途**：本文档仓库的 Claude Code 运行时直接加载这些 SKILL.md，无需 catalog 注册。

---

### `templates/phase1/` - Pack 模板（供其他 Agent 使用）

阶段一验证用的完整 Pack 模板结构，用于创建可分发的能力包。

```text
templates/phase1/
├── .github/workflows/         # CI/Release
├── catalog/                   # 静态注册（本地记账）
├── skills-devtools/           # domain pack 示例 A
└── skills-<another-domain>/   # domain pack 示例 B
```

说明：
- `skills-*` 目录表示多个 domain pack，并非单一 skill。
- 每个 pack 内再维护各自的 `skills/`、`agents/` 与 `pack.yaml`。
- 发布与消费通过 `catalog` 聚合索引。

**Catalog 用途**：Phase 1 中 catalog 作为**静态注册**（仓库内记账），记录可用 Pack 及其 `plugin_ref`。Phase 2 的自托管 Registry 才是跨仓库分发机制。

---

## 文档结构

- [ARCHITECTURE.md](./ARCHITECTURE.md) - 架构详细说明
- [docs/guides/README.md](./docs/guides/README.md) - 操作指南总入口（Agent 配置 / Skill 编写 / Subagent）
- [templates/README.md](./templates/README.md) - 模板使用指南
- [docs/references/SKILL_BEST_PRACTICES.md](./docs/references/SKILL_BEST_PRACTICES.md) - SKILL.md 最佳实践
- [docs/references/AGENT_PLUGINS.md](./docs/references/AGENT_PLUGINS.md) - 各工具插件支持
- [docs/references/agent_capability_platform_report.md](./docs/references/agent_capability_platform_report.md) - 需求调研报告

---

## 分阶段路线

| 阶段 | 解决的问题 | 核心交付物 | 状态 |
|------|-----------|-----------|------|
| **Phase 1** | 验证：单个 Pack 的 e2e 能否跑通？ | 单 Pack e2e 流程：编写→发布→Agent 消费 | 规划中 |
| **Phase 2** | 如何支持多 Pack 管理？ | 多 Pack 管理、自托管 Registry | 规划中 |
| **Phase 3** | 如何支持多 Agent 平台的不同消费方式？ | 多模式适配（Prompt/Tool/MCP） | 规划中 |
| **Phase 4** | 如何实现企业级治理能力？ | 权限、审计、合规、回滚 | 规划中 |
