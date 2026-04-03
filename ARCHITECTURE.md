# 架构概述

## 1. 设计原则
- 单真相源：仓库级仅维护 `pack.yaml`。
- Plugin-first：默认按 Pack 整体发布与消费。
- Skill 可选独立分发：仅在显式开启时生成单 Skill artifact。
- 多 Agent 统一消费：通过 `resolve/execute` 与 adapter 契约接入。

## 2. 仓库模型

```text
skills-devtools/
├── pack.yaml
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── release.yml
├── skills/
│   └── code-review/
│       ├── SKILL.md
│       ├── scripts/
│       ├── adapters/
│       └── tests/
└── shared/
```

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

### 4.3 Agent 消费
1. `resolve(skill_id, channel|version)`
2. 默认返回/使用 `plugin_ref`
3. 可选切换 `skill_ref`
4. `execute(skill_id, version, input_payload)`

## 5. CI/Release 基线
- CI：校验 `pack.yaml`、技能入口存在性、测试执行
- Release：tag 触发，默认产出 plugin artifact + manifest + checksum
- Catalog：更新版本通道及 `plugin_ref`（可选 `skill_ref`）
