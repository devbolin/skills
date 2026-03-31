# 阶段一详细设计（GitHub-only）

## 1. 目标与边界
阶段一目标是在不引入独立 Registry 服务的前提下，基于 GitHub 完成 Skill 的开发、评审、发布、分发、调用闭环。

### 1.1 目标
- 使用 GitHub 管理 Skill 生命周期：开发、评审、发布、回滚。
- 支持多个 Agent 通过统一契约调用同一批 Skill。
- 用最小成本形成可演进到阶段二/三的架构。

### 1.2 非目标
- 不实现独立在线 Registry API。
- 不实现租户级复杂授权中心。
- 不实现全量运行时调度平台。

## 2. 架构设计

### 2.1 仓库划分
- `skills-<domain>`：按领域管理 Skill 源码与发布。
- `skill-catalog`：静态索引仓库，提供 resolve 能力（文件级）。
- `skill-workflows`：可复用 workflow 与 schema（可选，初期也可放在 domain repo）。

### 2.2 核心对象
- `repo.yaml`：仓库级治理元数据。
- `skill.yaml`：Skill 契约（id/version/permissions/compatibility）。
- `artifact`：按 Skill+Version 打包产物。
- `manifest.json`：产物与校验信息。
- `catalog index`：全局 Skill 索引与 channel 映射。

### 2.3 统一调用接口
所有 Agent 统一按两个动作接入：
- `resolve(skill_id, channel|version)` -> 返回版本、artifact、adapter 入口、权限声明
- `execute(skill_id, version, input_payload)` -> 执行并返回结果

### 2.4 Agent 适配策略
- Copilot Chat Agent Mode：`.github/skills/<skill-id>/SKILL.md`
- OpenAI/LangChain/CrewAI/n8n：`adapters/tool/tool.json`
- MCP 客户端（后续）：`adapters/mcp/*`

## 3. 角色与职责（RACI 简化）
- Skill Owner：维护 `skill.yaml`、业务逻辑、测试。
- Repo Maintainer：维护 `repo.yaml`、workflow、发布策略。
- Security Reviewer：审核权限字段与风险变更。
- Agent Integrator：实现 resolver/executor SDK。

## 4. 数据与版本策略
- `skill_id` 在 catalog 内唯一。
- 版本采用 semver：`MAJOR.MINOR.PATCH`。
- channel 仅支持：`dev`、`beta`、`stable`。
- 生产默认只允许 `stable`，除非显式灰度。

## 5. 安全与治理基线
- 默认最小权限：`permissions.network=false`。
- 连接器权限显式声明：`connectors: ["storage:read"]`。
- 必须使用 Release Artifact，不直接引用 `main` 分支。
- 发布产物必须附带 checksum。

## 6. 可观测性基线
- 每次调用至少记录：`skill_id`、`version`、耗时、结果状态。
- 错误统一分类：参数错误、执行错误、外部依赖错误。
- 阶段一可先落地结构化日志，阶段二再接监控平台。

## 7. 里程碑（建议 2~3 周）
1. Week 1：结构与契约（schema + repo/skill manifest）
2. Week 2：CI/Release 与 catalog 更新
3. Week 3：至少 2 个 Agent 接入验证（Copilot + Tool Adapter）

## 8. 验收标准
- PR 能自动校验 schema/结构/测试。
- 合并后自动发布 artifact 与 manifest。
- catalog 自动指向最新 stable。
- 至少两个 Agent 成功调用同一 skill。
