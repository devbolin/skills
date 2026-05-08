# 阶段一配置文档（Plugin-first）

本文档对应 `templates/phase1/` 下的模板文件。

## 1. `pack.yaml` 字段手册

| 字段 | 必需 | 默认值 | 影响流程节点 | 说明 |
|---|---|---|---|---|
| `id` | 是 | 无 | 发布、catalog、审计 | Pack 稳定标识 |
| `name` | 是 | 无 | 文档、发布元数据 | Pack 可读名称 |
| `version` | 是 | 无 | tag/release/catalog | 版本标识 |
| `owners` | 是 | 无 | 审核、责任归属 | 维护者列表 |
| `distribution.default` | 是 | `plugin` | 发布、调用 | 默认分发模式 |
| `distribution.enable_skill_artifacts` | 是 | `false` | 发布、catalog、调用 | 是否生成单 Skill 产物 |
| `defaults.permissions` | 是 | 最小权限 | 执行、审计 | 默认权限边界 |
| `skills[].id` | 是 | 无 | catalog、执行 | Skill 标识 |
| `skills[].path` | 是 | 无 | CI、打包 | Skill 目录 |
| `skills[].mode` | 是 | 无 | 执行路由 | prompt |
| `skills[].entry` | 是 | 无 | 执行入口 | 入口文件路径 |
| `skills[].description` | 否 | 无 | discoverability | 可读描述 |
| `skills[].adapters` | 否 | 无 | 多工具接入（Phase 3） | 适配器路径映射 |
| `agents[].id` | 否 | 无 | 委托、审计 | Subagent 标识 |
| `agents[].path` | 否 | 无 | CI、打包、运行时委托 | Subagent 声明文件，固定为 `agents/<id>.md` |

最小必需字段口径：`skills[]` 仅要求 `id/path/mode/entry`。
如 Pack 包含 Subagent，则 `agents[]` 最小必需字段为 `id/path`。

## 2. `SKILL.md` 关键字段

### 必需字段（frontmatter）
| 字段 | 说明 |
|------|------|
| `name` | Skill 名称，必须与父目录名一致 |
| `description` | 激活描述与触发关键词 |

### 可选字段（frontmatter）
| 字段 | 说明 |
|------|------|
| `license` | 许可证 |
| `compatibility` | 环境依赖说明 |
| `metadata` | 附加元数据（如 `version`、`author`、`tags`） |

> 依据 [Agent Skills 规范](https://agentskills.io/specification)，仅 `name` 和 `description` 为必需字段。`version`、`author`、`tags` 建议放在 `metadata` 映射中。

### 正文内容（建议包含）
- `## 使用场景`：适用场景列表
- `## 不适用场景`：边界说明
- `## 使用方法`：调用方式说明

> frontmatter 为 Agent/Runtime 解析元数据，正文内容为模型消费指令。

## 2.1 `agents/<id>.md` 约定

Subagent 以单文件形式保存在 Pack 内：

```text
agents/
  <id>.md
```

建议至少包含：
- 职责说明
- 适用场景与不适用场景
- 输入约束
- 输出约定
- 失败回退策略

## 3. catalog 字段手册

### pack.yaml vs catalog/ 架构关系

| 文件 | 位置 | 作用域 | 职责 |
|------|------|--------|------|
| `pack.yaml` | 每个 pack 内 | 单个 pack | 真相源：声明这个 pack 有哪些 Skill |
| `catalog/` | 外层（组织级别） | 所有 pack | 发布索引：聚合所有 pack 的 artifact 引用 |

**为什么分离？**
- `pack.yaml` 是源码清单，供开发者维护
- `catalog/` 是运行时索引，供 Agent/Runtime 读取
- Phase 2 Registry 跨仓库聚合，替代这个单一 catalog

### 3.1 `index.json`

| 字段 | 必需 | 说明 |
|---|---|---|
| `version` | 是 | catalog 版本 |
| `generated_at` | 是 | 生成时间 |
| `distribution_default` | 是 | 默认分发模式（应为 `plugin`） |
| `skills[].skill_id` | 是 | Skill 标识 |
| `skills[].pack_id` | 是 | 归属 Pack |
| `skills[].repo` | 建议 | GitHub 仓库路径（org/repo） |
| `skills[].path` | 是 | Skill 相对于仓库根的路径 |
| `skills[].channels` | 是 | 通道到版本映射 |
| `skills[].catalog_entry` | 是 | skill 明细索引路径 |
| `skills[].plugin_ref` | 是 | 默认消费产物（releases 路径） |

### 3.2 `skills/<skill-id>.json`

| 字段 | 必需 | 说明 |
|---|---|---|
| `skill_id` | 是 | Skill 标识 |
| `name` | 是 | Skill 可读名称（供 Agent 展示） |
| `pack_id` | 建议 | 归属 Pack（推荐与 index 一致） |
| `versions.<ver>.channel` | 是 | 所属通道 |
| `versions.<ver>.plugin_artifact` | 是 | Plugin artifact 引用 |
| `versions.<ver>.skill_ref` | 否 | 单 Skill 产物引用（仅当 `ENABLE_SKILL_ARTIFACTS=true` 时生成） |
| `versions.<ver>.compatibility` | 建议 | Agent 兼容信息 |
| `versions.<ver>.permissions` | 建议 | 运行权限声明 |

## 4. workflow 参数说明

| 参数 | 位置 | 默认值 | 影响 |
|---|---|---|---|
| `ENABLE_SKILL_ARTIFACTS` | `release.yml` env | `false` | 是否额外生成单 Skill artifact |
| Tag 触发规则 | `release.yml` on.push.tags | `v*.*.*` | 控制发布触发时机 |
| CI 分支触发 | `ci.yml` on.push.branches | `main` | 控制结构校验执行 |

## 5. 参数到行为映射

| 条件 | 行为 |
|---|---|
| `distribution.default=plugin` | 发布 plugin artifact，运行时默认使用 `plugin_ref` |
| `ENABLE_SKILL_ARTIFACTS=false` | 跳过单 Skill 打包 |
| `ENABLE_SKILL_ARTIFACTS=true` | 增加单 Skill 打包，并在 catalog 增量写入 `skill_ref` |
| 缺失 `plugin_ref` | 运行时读取 catalog 失败并阻断执行 |

## 6. 约束建议
- 生产消费默认使用 plugin artifact。
- 单 Skill 分发必须显式开关启用。
- 所有入口路径必须可解析到仓库文件。
- Subagent 路径固定指向 `agents/<id>.md`，不使用 `agent.yaml` 或嵌套 `AGENT.md`。


## 7. Agent 消费配置入口
- 具体 Agent 消费规范与示例见 [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md)。
- 本文仅保留统一字段口径：`skills[]` 最小必需 `id/path/mode/entry`。
- Agent 路由以 `mode + entry` 为准，默认消费 `plugin_ref`，可选 `skill_ref` 回退策略见专文。
