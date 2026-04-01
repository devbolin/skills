# 阶段一模板验证报告

> 说明：本报告基于旧版模板（skill artifact 示例）生成，当前阶段一规范已切换为 Plugin-first。

## 1. 历史验证记录（保留）
- 验证时间: 2026-03-31 13:33:00 +0800
- 仓库: /Users/zhangbolin/vibe-coding/skill-management
- 范围: docs/phase1 + templates/phase1

### 验证项
1. 关键文件存在性检查: 通过
2. JSON 文件语法校验: 通过
3. 发布最小打包流程（zip + manifest + checksum）: 通过

### 历史产物
- zip: tmp/phase1-verify/dist/invoice-extractor-1.0.0.zip
- manifest: tmp/phase1-verify/dist/manifest.json
- checksum: 061602673e248d6edd3b8c98b3675f78204fc5439ca85f9ae6c1914e4aaf572b  invoice-extractor-1.0.0.zip

## 2. 当前规范差异说明
- 当前默认分发应为 plugin artifact，而非单 Skill artifact。
- catalog 当前语义应以 `plugin_ref` 为默认消费入口。
- 单 Skill artifact 仅在显式开启 `enable_skill_artifacts=true` 时生成。

## 3. 需重跑验证项（按当前规范）
1. Plugin artifact 验证
- release 产物是否包含 `*-plugin.zip`
- `manifest.json` 是否声明 plugin 产物字段

2. Catalog `plugin_ref` 验证
- `index.json` 中是否存在有效 `plugin_ref`
- skill 明细是否与通道版本一致

3. 可选单 Skill 分发开关验证
- 默认 `enable_skill_artifacts=false` 时不生成单 Skill 产物
- 开启时生成单 Skill 产物并写入可选 `skill_ref`

4. 运行链路验证
- Agent/Runtime 默认优先 `plugin_ref`
- 指定策略且存在 `skill_ref` 时可切换执行

## 4. 备注
- 本次为文档层透明化更新，不包含 GitHub Actions 在线执行与多 Agent 联调。
