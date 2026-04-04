# 阶段一 Agent 消费规范（Plugin-first）

## 1. 目标与范围
本文档定义 phase1 下 Agent 消费 Skill 的统一步骤。

约束：
- 默认消费 `plugin_ref`。
- `skill_ref` 为可选路径，仅在策略命中且引用存在时使用。
- Phase1 验收以 plugin-first 主路径为准。

## 2. 统一消费步骤
1. Agent/Runtime 根据 `skill_id + channel/version` 读取 catalog。
2. 检查并加载 `plugin_ref`（默认路径）。
3. 若策略要求且存在 `skill_ref`，可切换到 `skill_ref`。
4. 根据 `mode + entry` 定位入口并执行。
5. 返回结果、错误信息与基础指标。

## 3. Agent 消费总览图

```mermaid
flowchart TD
  A[Agent Request skill_id and channel_or_version] --> B[Agent or Runtime Read Catalog]
  B --> C{plugin_ref exists}
  C -- No --> X[Fail with catalog error]
  C -- Yes --> D[Load plugin_ref]
  D --> E{Policy allows skill_ref and skill_ref exists}
  E -- Yes --> F[Switch to skill_ref]
  E -- No --> G[Keep plugin_ref]
  F --> H[Route by mode and entry]
  G --> H
  H --> I[Execute]
  I --> J[Return result and metrics]
```

## 4. 配置映射（pack -> catalog -> runtime）

| 来源 | 字段 | 必需 | 说明 |
|---|---|---|---|
| `pack.yaml` | `skills[].id` | 是 | Skill 标识 |
| `pack.yaml` | `skills[].path` | 是 | Skill 路径 |
| `pack.yaml` | `skills[].mode` | 是 | 路由模式（Phase 1 仅 prompt） |
| `pack.yaml` | `skills[].entry` | 是 | 执行入口 |
| `pack.yaml` | `skills[].description` | 否 | 可发现性描述 |
| `pack.yaml` | `skills[].adapters` | 否 | 多工具路径映射（Phase 3） |
| `catalog index` | `plugin_ref` | 是 | 默认消费产物 |
| `catalog detail` | `skill_ref` | 否 | 可选单 Skill 产物 |
| `runtime` | `mode + entry` | 是 | 最终执行定位 |

## 5. 消费模式与路由

> 消费模式定义详见 [CONCEPTS.md](../CONCEPTS.md#agent-消费模式)

> Phase3 将扩展 Tool/MCP 模式支持。

### 5.1 Prompt 模式（Copilot）

最小配置（pack 侧）：
```yaml
skills:
  - id: code-review
    path: skills/code-review
    mode: prompt
    entry: skills/code-review/SKILL.md
```
路由规则：
- 解析 `mode=prompt`。
- 入口文件为 `SKILL.md`。
- 默认从 plugin 包内加载对应路径。

失败回退：
- `entry` 缺失或不可读：执行失败并返回入口错误。
- `skill_ref` 缺失：保持 plugin 默认路径，不切换。

## 6. 错误语义

| 场景 | 错误语义 | 默认处理 |
|---|---|---|
| `plugin_ref` 缺失 | `catalog_plugin_ref_missing` | 直接失败 |
| `mode/entry` 无法解析 | `entry_invalid` | 拒绝执行并记录错误 |
| `skill_ref` 不可用 | `skill_ref_unavailable` | 回退 `plugin_ref` |
| 权限冲突 | `permission_denied` | 拒绝执行并记录审计 |

## 7. 不要误解
- `pack.yaml` 不是 Agent 运行时配置文件。
- `pack.yaml` 负责发布与索引契约，Agent 运行时配置在各客户端环境完成。
- phase1 默认分发路径是 plugin，单 Skill 分发仅为可选能力。

## 8. 验收清单（文档级）
- [x] 明确 Phase1 必选主路径：`plugin_ref` + `mode/entry` 路由执行。
- [x] 默认 `plugin_ref` 与可选 `skill_ref` 行为可解释且一致。
- [x] 文档术语与 `docs/phase1` 其余文档一致（`pack.yaml`、plugin-first、`id/path/mode/entry`）。
