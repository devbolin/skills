# Phase 2：多 Pack 管理与自托管 Registry

> 状态：规划中

## 目标

在 Phase 1 单 Pack e2e 闭环的基础上，扩展到多 Pack 场景，并引入自托管 Registry 替代纯 GitHub catalog。

## 核心能力

| 能力 | 说明 |
|------|------|
| 多 Pack 聚合管理 | 支持多个相关 Pack 在同一仓库或跨仓库协作 |
| 自托管 Registry | 独立的 Registry API，支持 Pack 的注册、发现、版本管理 |
| Cross-Pack 引用 | 一个 Pack 可声明对其他 Pack 的依赖 |
| Pack 组合与继承 | 支持 Pack 间的技能共享与组合 |

## 架构变化

```
Phase 1: GitHub-only + catalog（静态）
Phase 2: Self-hosted Registry + 动态发现
```

## 关键设计点

### Registry API（待设计）

- `POST /packs` - 注册新 Pack
- `GET /packs/{id}` - 获取 Pack 元数据
- `GET /packs/{id}/versions` - 列出可用版本
- `GET /packs/{id}/skills` - 列出 Pack 包含的 Skills
- `DELETE /packs/{id}` - 删除 Pack（需权限）

### Catalog vs Registry

| 概念 | Phase 1 | Phase 2 |
|------|---------|---------|
| 存储位置 | GitHub 仓库内（静态） | 自托管 Registry（动态） |
| 更新方式 | Release workflow 手动更新 | Registry API 自动注册 |
| 可见性 | 仅仓库内可见 | 跨组织可见 |
| 版本管理 | Git tag | Registry 版本管理 |

## 里程碑（待定）

- Sprint 1: Registry 核心 API设计与实现
- Sprint 2: 多 Pack 依赖管理
- Sprint 3: 与 Phase 1 现有 workflow 集成

## 相关文档

- Phase 1: [../phase1/README.md](../phase1/README.md) - 单 Pack e2e 闭环
- Phase 3: [../phase3/README.md](../phase3/README.md) - 多平台适配
