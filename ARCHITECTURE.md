# 架构概述

## 1. 设计原则

- **单真相源**：仓库级仅维护 `pack.yaml`
- **Plugin-first**：默认按 Pack 整体发布与消费
- **Skill 可选独立分发**：仅在显式开启时生成单 Skill artifact
- **多 Agent 统一消费**：通过 `resolve/execute` 与 adapter 契约接入

## 2. 核心概念

| 概念 | 说明 |
|------|------|
| **Pack** | 仓库级能力包，是最小治理边界 |
| **Skill** | 可复用 AI 能力单元，AI 可自动检测并激活 |
| **Catalog** | 记录可用 Pack/Skill 及访问方式 |
| **Plugin** | 包含 Skills/Commands/Agents 的分发包 |
| **Subagent** | 运行在独立上下文的专业 Agent |

详见 [CONCEPTS.md](docs/CONCEPTS.md)。

## 3. 核心文件职责

### 3.1 pack.yaml（唯一仓库清单）
- 声明 Pack 基本信息：`id/name/version/owners`
- 声明分发策略：`distribution.default=plugin`
- 声明单 Skill 产物开关：`distribution.enable_skill_artifacts=false`
- 声明默认权限与 skills 列表、入口与 adapter

### 3.2 SKILL.md（Skill 运行说明）
- 面向模型：触发条件、适用/不适用场景、执行步骤
- 作为 prompt 型消费入口

### 3.3 skill.yaml（可选）
- 仅作为构建阶段生成的机器清单（generated manifest）
- 不作为手工维护文件

## 4. 分发与消费

### 4.1 默认分发
- 发布 plugin artifact（整仓）
- catalog 记录 `plugin_ref`

### 4.2 可选分发
- 当 `enable_skill_artifacts=true` 时生成单 Skill artifact
- catalog 在对应 skill 条目附加 `skill_ref`

### 4.3 平台适配（Adapter 契约）
`resolve/execute` 返回平台中立的 catalog 引用，各平台的 adapter 层负责将 `agents/<id>.md` 等源码格式转换为目标平台原生格式：

| 转换内容 | Claude Code | VS Code | OpenCode |
|---------|-------------|---------|----------|
| Agent 文件扩展名 | `.md`（不变） | `.md`→`.agent.md` | `.md`（不变） |
| 工具命名 | PascalCase（不变） | PascalCase→kebab-case | 映射为 `permission` 配置 |
| 目录路径 | `.claude/agents/` | `.github/agents/` | `.claude/agents/`（兼容） |

> 转换逻辑在构建阶段（`scripts/build.py`）执行。源码层始终维护平台中立格式。参见 Phase 3 的多模式适配（Prompt/Tool/MCP）规划。

### 4.4 Agent 消费
1. `resolve(skill_id, channel|version)`
2. 默认返回/使用 `plugin_ref`
3. 可选切换 `skill_ref`
4. `execute(skill_id, version, input_payload)`

## 5. 阶段范围

| 阶段 | 范围 |
|------|------|
| **Phase 1** | 单 Pack 端到端：author → publish → Agent consume |
| **Phase 2** | 多 Pack 管理 + 自托管 Registry |
| **Phase 3** | 多平台适配（Prompt/Tool/MCP） |
| **Phase 4** | 企业治理（权限、审计、回滚） |

详细流程见 [FLOW.md](docs/phase1/FLOW.md)。
