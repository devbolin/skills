# 阶段一详细设计（GitHub-only, Plugin-first）

## 1. 目标与边界
阶段一目标是在不引入独立 Registry 服务的前提下，基于 GitHub 完成 Skill 的开发、评审、发布、分发、调用闭环。

### 1.1 目标
- 使用 GitHub 管理 Skill 生命周期：开发、评审、发布、回滚。
- 默认以 **Plugin 形态整体分发 Pack**。
- 保留单 Skill 分发能力，但默认关闭，仅在需要时启用。
- 支持多个 Agent 通过统一契约调用同一批 Skill。

### 1.2 非目标
- 不实现独立在线 Registry API。
- 不实现租户级复杂授权中心。
- 不实现全量运行时调度平台。

## 2. 设计决策（P0）

### 2.1 单真相源
- 仓库级只保留一个清单文件：`pack.yaml`。
- 不并列维护 `repo.yaml` 与 `pack.yaml`。

### 2.2 Skill 元数据分层
- `SKILL.md`：模型消费层（触发、边界、指令）。
- `pack.yaml`：平台控制层（索引、入口、分发策略、默认权限）。
- `skill.yaml`：可选生成产物（generated manifest），不作为手工维护真相源。

### 2.3 分发策略
- 默认产物：Plugin（整仓/整包）。
- 可选产物：单 Skill artifact（按需启用）。

## 3. 架构设计

### 3.1 仓库划分
- `skills-<domain>`：按领域管理 Pack 源码与发布。
- `skill-catalog`：静态索引仓库，记录 `plugin_ref` 与可选 `skill_ref`。

### 3.2 核心对象
- `pack.yaml`：仓库级治理与分发元数据。
- `SKILL.md`：每个 skill 的执行说明。
- `artifact`：默认 plugin artifact，可选 skill artifact。
- `manifest.json`：发布产物与校验信息。
- `catalog index`：全局索引与通道映射。

### 3.3 统一调用接口
所有 Agent 统一按两个动作接入：
- `resolve(skill_id, channel|version)` -> 返回 plugin_ref、可选 skill_ref、adapter 入口、权限声明
- `execute(skill_id, version, input_payload)` -> 执行并返回结果

## 4. `pack.yaml` 规范（阶段一）
建议字段：
- `id`, `name`, `version`, `owners`
- `distribution.default: plugin`
- `distribution.enable_skill_artifacts: false`（默认）
- `defaults.permissions`
- `skills[]`：最小必需 `id/path/mode/entry`（`description/adapters` 可选）

## 5. 安全与治理基线
- 默认最小权限：`defaults.permissions.network=false`。
- 连接器权限需显式声明。
- 生产执行默认使用 plugin release 产物，不直接引用 `main`。
- 发布产物必须附带 checksum。

## 6. 可观测性基线
- 每次调用至少记录：`pack_id`、`skill_id`、`version`、耗时、结果状态。
- 错误统一分类：参数错误、执行错误、外部依赖错误。

## 7. 里程碑（建议 2~3 周）
1. Week 1：`pack.yaml` 契约落地 + CI 结构校验。
2. Week 2：Plugin-first 发布流水线 + catalog `plugin_ref` 更新。
3. Week 3：两个 Agent 验证（Copilot + Tool Adapter），并演示可选单 Skill 分发开关。

## 8. 验收标准
- PR 能自动校验 pack 结构、入口与引用一致性。
- 合并后可发布 plugin artifact 与 manifest。
- catalog 默认提供 `plugin_ref`。
- 单 Skill artifact 仅在显式开启时生成。
