# 阶段一配置文档（Plugin-first）

本文档对应 `templates/phase1/` 下的模板文件。

## 1. `pack.yaml` 关键字段
- `id/name/version/owners`: Pack 基础元信息。
- `distribution.default`: 默认分发模式，固定为 `plugin`。
- `distribution.enable_skill_artifacts`: 是否允许生成单 Skill artifact（默认 `false`）。
- `defaults.permissions`: 默认权限边界。
- `skills`: Skill 列表，最小必需字段为 `id/path/mode/entry`。

## 2. `SKILL.md` 关键字段
- `name`: Skill 名称（建议与目录一致）。
- `description`: 激活描述与触发关键词。
- 正文：使用场景、边界、调用方式。

## 3. workflow 说明
- `ci.yml`:
  - 检查 `pack.yaml` 存在
  - 校验 skills 入口文件存在
  - 预留测试入口
- `release.yml`:
  - 基于 tag 触发
  - 默认打包 plugin artifact
  - 生成 manifest/checksums
  - 可选生成单 Skill artifact

## 4. catalog 说明
- `index.json` 是入口。
- 每个 skill 对应单独 `skills/<skill-id>.json`。
- 记录 `plugin_ref`（默认）与可选 `skill_ref`。

## 5. 约束建议
- 生产消费默认使用 plugin artifact。
- 单 Skill 分发必须显式开关启用。
- 所有入口路径必须可解析到仓库文件。
