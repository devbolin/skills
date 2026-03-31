# 阶段一配置文档

本文档对应 `templates/phase1/` 下的模板文件。

## 1. `repo.yaml` 关键字段
- `repo_id`: 仓库稳定标识（小写+连字符）。
- `defaults.release_channel`: 默认发布通道。
- `skills`: skill 列表，`id` 与 `path` 必须一一对应。

## 2. `skill.yaml` 关键字段
- `id/version/status`: Skill 主键与生命周期。
- `runtime`: 执行入口。
- `permissions`: 安全边界。
- `compatibility.agents`: 多 Agent 入口声明。

## 3. workflow 说明
- `ci.yml`:
  - 检查目录与配置文件存在性
  - 校验 JSON 文件合法性
  - 预留 test 入口
- `release.yml`:
  - 基于 tag 触发
  - 打包 skill artifact
  - 生成 manifest/checksums

## 4. catalog 说明
- `index.json` 是入口。
- 每个 skill 对应单独 `skills/<skill-id>.json`。
- channel 指针（如 stable）由发布流程维护。

## 5. 约束建议
- 所有生产调用必须 pin 到版本。
- 不允许未声明权限的网络调用。
- `compatibility.agents[*].entry` 必须可解析到仓库文件。
