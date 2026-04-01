# 跨平台 AI Agent Skills 管理

> 版本：v2.1
> 更新：2026-04-01

以 **GitHub 为唯一控制面**，通过 **Domain Polyrepo** + **GitHub Actions** + **Release** + **Skill Catalog** 实现多 Agent 技能复用。

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

---

## 分阶段路线

| 阶段 | 目标 | 状态 |
|------|------|------|
| Phase 1 | GitHub-only + Plugin-first | 进行中 |
| Phase 2 | 自动化增强（可选单 Skill 分发） | 规划中 |
| Phase 3 | 引入轻量 Registry | 规划中 |
| Phase 4 | 运行治理与 MCP | 规划中 |
