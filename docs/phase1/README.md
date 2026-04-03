# 阶段一实现文档

> 阶段一目标：在不引入独立 Registry 服务的前提下，基于 GitHub 完成 Pack 的开发、评审、发布、分发、调用闭环，默认采用 Plugin-first 分发。

## 核心文档

| 文档 | 说明 |
|------|------|
| [HUMAN_WORKFLOW.md](./HUMAN_WORKFLOW.md) | 人类视角完整工作流程（开发→发布→Agent 集成→回滚） |
| [DESIGN.md](./DESIGN.md) | 阶段一架构设计与决策主文档 |
| [FLOW.md](./FLOW.md) | 发布/调用/回滚流程规范（系统视角） |
| [CONFIG.md](./CONFIG.md) | 配置字段操作手册 |
| [VALIDATION.md](./VALIDATION.md) | 模板验证报告与差异说明 |
| [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md) | Agent 消费规范与配置示例 |

## 图示索引

| 图示 | 位置 | 说明 |
|---|---|---|
| 人类活动总览图 | [HUMAN_WORKFLOW.md](./HUMAN_WORKFLOW.md) | 从 Author 到 Agent 集成的完整人类旅程 |
| Phase1 架构总览图 | [DESIGN.md](./DESIGN.md) | Author/CI/Release/Catalog/Runtime 关系 |
| 配置关系图 | [DESIGN.md](./DESIGN.md) | `pack.yaml`、`SKILL.md`、artifact、catalog 依赖 |
| 发布流程图 | [FLOW.md](./FLOW.md) | PR 到发布与 catalog 更新 |
| 调用流程图 | [FLOW.md](./FLOW.md) | Agent/Runtime 读取 catalog 与 plugin_ref 优先策略 |
| 回滚流程图 | [FLOW.md](./FLOW.md) | plugin 通道回退与可选 skill_ref 联动 |
| Agent 消费总览图 | [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md) | catalog 到 mode/entry 路由与执行 |

## 阅读顺序
1. [HUMAN_WORKFLOW.md](./HUMAN_WORKFLOW.md)：先看人类视角完整工作流程（推荐优先阅读）。
2. [DESIGN.md](./DESIGN.md)：确认架构决策与边界。
3. [CONFIG.md](./CONFIG.md)：确认字段和参数语义。
4. [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md)：确认各 Agent 消费配置与回退规则。
5. [FLOW.md](./FLOW.md)：再看发布、调用、回滚系统流程。
6. [VALIDATION.md](./VALIDATION.md)：确认验证现状与重跑项。

## Phase 边界说明
- Phase1：单 Pack e2e 闭环（编写→发布→catalog→Agent 通过 plugin 路径消费）。
- Phase3：多平台多模式适配（Prompt/Tool/MCP 的平台化强化与大规模验证）。
- 本目录中的 Tool/MCP 内容用于兼容映射说明，不代表 Phase1 必选验收门槛。

## 里程碑

| 周次 | Sprint | 目标 |
|------|--------|------|
| Week 1 | Sprint 1 | `pack.yaml` 契约与结构校验 |
| Week 2 | Sprint 1 | Plugin-first CI/Release 与 catalog `plugin_ref` 更新 |
| Week 3 | Sprint 2 | Agent plugin 路径消费验证（单 Pack e2e） |
| Week 4 | Sprint 2 | 文档收敛与回滚链路验证 |

## Sprint 划分

### Sprint 1（Week 1-2）：基础校验 + 发布闭环
**目标**：建立单 Pack 的发布闭环

**Week 1 交付物**：
- `pack.yaml` schema 校验集成到 CI
- YAML 语法校验（yamllint）
- 字段完整性校验（skills[].id/path/mode/entry）

**Week 2 交付物**：
- Plugin artifact 打包发布
- catalog `plugin_ref` 更新
- Catalog 一致性校验
- 发布产物校验（manifest + checksum）

### Sprint 2（Week 3-4）：Agent 消费验证 + 稳定化
**目标**：验证 Agent 能通过 plugin 主路径完成消费并可回滚

**Week 3 交付物**：
- plugin 包落盘与入口可读验证
- Agent plugin 主路径消费验证
- 失败分支处理验证（入口缺失、权限冲突）

**Week 4 交付物**：
- catalog 回滚链路验证
- 端到端链路测试（pack → release → catalog → Agent 发现）
- 文档与术语收敛

## 验收标准
- PR 能自动校验 pack 结构、入口与测试（YAML + Schema + 字段完整性）。
- 发布默认生成 plugin artifact 与 manifest。
- catalog 默认提供 `plugin_ref`。
- 单 Skill artifact 仅在显式开启时生成。
- **Agent 可通过 plugin 主路径成功调用 Skill**（`plugin_ref` + `mode/entry`）。
- **端到端链路可追溯**：pack → release → catalog → Agent 发现。
- Prompt/Tool/MCP 的平台化强约束验证属于 Phase3，不作为 Phase1 必选门槛。

## 相关资源
- [模板目录](../../templates/phase1/) - Domain Polyrepo 模板
- [Schema 定义](../../templates/phase1/schemas/) - 兼容/扩展校验（当前阶段一以 `pack.yaml` 为主，schema 作为可选增强）
