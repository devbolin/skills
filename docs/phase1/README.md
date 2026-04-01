# 阶段一实现文档

> 阶段一目标：在不引入独立 Registry 服务的前提下，基于 GitHub 完成 Skill 的开发、评审、发布、分发、调用闭环。

## 核心文档

| 文档 | 说明 |
|------|------|
| [DESIGN.md](./DESIGN.md) | 阶段一架构设计 |
| [FLOW.md](./FLOW.md) | 开发到发布流程 |
| [CONFIG.md](./CONFIG.md) | 配置指南 |
| [VALIDATION.md](./VALIDATION.md) | 模板验证报告 |

## 里程碑

| 周次 | 目标 |
|------|------|
| Week 1 | 结构与契约（schema + repo/skill manifest） |
| Week 2 | CI/Release 与 catalog 更新 |
| Week 3 | 至少 2 个 Agent 接入验证（Copilot + Tool Adapter） |

## 验收标准

- PR 能自动校验 schema/结构/测试
- 合并后自动发布 artifact 与 manifest
- catalog 自动指向最新 stable
- 至少两个 Agent 成功调用同一 skill

## 相关资源

- [模板目录](../../templates/phase1/) - Domain Polyrepo 模板
- [Schema 定义](../../templates/phase1/schemas/) - repo.yaml 和 skill.yaml 的 JSON Schema
