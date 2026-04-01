# 阶段一实现文档

> 阶段一目标：在不引入独立 Registry 服务的前提下，基于 GitHub 完成 Pack 的开发、评审、发布、分发、调用闭环，默认采用 Plugin-first 分发。

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
| Week 1 | `pack.yaml` 契约与结构校验 |
| Week 2 | Plugin-first CI/Release 与 catalog 更新 |
| Week 3 | 至少 2 个 Agent 接入验证（Copilot + Tool Adapter） |

## 验收标准

- PR 能自动校验 pack 结构、入口与测试
- 发布默认生成 plugin artifact 与 manifest
- catalog 默认提供 `plugin_ref`
- 单 Skill artifact 仅在显式开启时生成

## 相关资源

- [模板目录](../../templates/phase1/) - Domain Polyrepo 模板
- [Schema 定义](../../templates/phase1/schemas/) - 兼容/扩展校验（当前阶段一以 `pack.yaml` 为主，schema 作为可选增强）
