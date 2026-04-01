# 阶段一实现文档

> 阶段一目标：在不引入独立 Registry 服务的前提下，基于 GitHub 完成 Pack 的开发、评审、发布、分发、调用闭环，默认采用 Plugin-first 分发。

## 核心文档

| 文档 | 说明 |
|------|------|
| [DESIGN.md](./DESIGN.md) | 阶段一架构设计与决策主文档 |
| [FLOW.md](./FLOW.md) | 发布/调用/回滚流程规范 |
| [CONFIG.md](./CONFIG.md) | 配置字段操作手册 |
| [VALIDATION.md](./VALIDATION.md) | 模板验证报告与差异说明 |
| [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md) | Agent 消费规范与配置示例 |

## 图示索引

| 图示 | 位置 | 说明 |
|---|---|---|
| Phase1 架构总览图 | [DESIGN.md](./DESIGN.md) | Author/CI/Release/Catalog/Runtime 关系 |
| 配置关系图 | [DESIGN.md](./DESIGN.md) | `pack.yaml`、`SKILL.md`、artifact、catalog 依赖 |
| 发布流程图 | [FLOW.md](./FLOW.md) | PR 到发布与 catalog 更新 |
| 调用流程图 | [FLOW.md](./FLOW.md) | Agent/Runtime 读取 catalog 与 plugin_ref 优先策略 |
| 回滚流程图 | [FLOW.md](./FLOW.md) | plugin 通道回退与可选 skill_ref 联动 |
| Agent 消费总览图 | [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md) | catalog 到 mode/entry 路由与执行 |

## 阅读顺序
1. [DESIGN.md](./DESIGN.md)：先确认架构决策与边界。
2. [CONFIG.md](./CONFIG.md)：再确认字段和参数语义。
3. [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md)：确认各 Agent 消费配置与回退规则。
4. [FLOW.md](./FLOW.md)：最后看发布、调用、回滚流程。
5. [VALIDATION.md](./VALIDATION.md)：确认验证现状与重跑项。

## 里程碑

| 周次 | 目标 |
|------|------|
| Week 1 | `pack.yaml` 契约与结构校验 |
| Week 2 | Plugin-first CI/Release 与 catalog 更新 |
| Week 3 | 至少 2 个 Agent 接入验证（Copilot + Tool Adapter） |

## 验收标准
- PR 能自动校验 pack 结构、入口与测试。
- 发布默认生成 plugin artifact 与 manifest。
- catalog 默认提供 `plugin_ref`。
- 单 Skill artifact 仅在显式开启时生成。

## 相关资源
- [模板目录](../../templates/phase1/) - Domain Polyrepo 模板
- [Schema 定义](../../templates/phase1/schemas/) - 兼容/扩展校验（当前阶段一以 `pack.yaml` 为主，schema 作为可选增强）
