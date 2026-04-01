# 跨平台 AI Agent Pack/Plugin 管理

> 版本：v2.1
> 更新：2026-04-01

以 **GitHub 为唯一控制面**，通过 **Domain Polyrepo** + **GitHub Actions** + **Release** + **Skill Catalog** 实现 Pack 的 plugin-first 分发与多 Agent 消费。

## 当前规范（Normative）
- 阶段一以 `pack.yaml` 作为仓库级唯一清单文件。
- 默认分发模式为 **Plugin-first（整仓分发）**。
- 保留单 Skill 分发能力，但默认关闭，仅作为可选模式。
- `skill.yaml` 不作为手工维护真相源，仅可作为生成产物。

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

### Pack
- 仓库级能力包，由 `pack.yaml` 描述。
- 是维护、发布、回滚的最小治理边界。

### Skill
- Agent 可复用能力单元。
- 模型消费入口为 `SKILL.md`。
- 工具消费入口由 `pack.yaml` 的 `skills[*].entry/adapters` 声明。

### Adapter
- Skill 对特定工具的适配层。
- 支持 `prompt/tool/workflow/mcp`。

### Agent
- Pack/Skill 的消费者。
- 默认通过 catalog 的 `plugin_ref` 消费 plugin artifact。
- 仅在显式开启并可用时使用 `skill_ref`。

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

| 阶段 | 目标 | 状态 |
|------|------|------|
| Phase 1 | GitHub-only + Plugin-first | 进行中 |
| Phase 2 | 自动化增强（可选单 Skill 分发） | 规划中 |
| Phase 3 | 引入轻量 Registry | 规划中 |
| Phase 4 | 运行治理与 MCP | 规划中 |
