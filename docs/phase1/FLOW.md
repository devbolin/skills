# 阶段一流程文档（GitHub-only, Plugin-first）

## 1. 开发到发布流程
1. 开发者修改 `pack.yaml` 与 `skills/<skill-id>/SKILL.md`（及 adapters/scripts）。
2. 发起 PR，触发 CI：结构校验、引用校验、变更测试。
3. Codeowners 审核通过后合并 `main`。
4. 创建 Git Tag (`v*.*.*`) 触发 Release workflow。
5. 默认打包 plugin artifact，生成 `manifest.json` 和 `checksums.txt`。
6. 若开启 `enable_skill_artifacts=true`，额外产出单 Skill artifact。
7. 更新 `skill-catalog/index.json` 与对应 skill 明细（包含 `plugin_ref`，可选 `skill_ref`）。

## 2. Agent 调用流程
1. Agent 调用 resolver：`resolve(skill_id, channel|version)`。
2. resolver 读取 catalog，优先返回 `plugin_ref`，并附可选 `skill_ref`。
3. 执行器默认从 plugin artifact 加载目标 skill 入口。
4. 若策略指定单 Skill 模式且存在 `skill_ref`，切换到单 Skill artifact 执行。
5. 返回执行结果并记录日志。

## 3. 回滚流程
1. 将 catalog 的稳定通道回退到上一个 plugin 版本。
2. 若单 Skill 分发启用，同时回退对应 `skill_ref`。
3. resolver 读取新索引后自动恢复到旧版本。

## 4. 失败处理规则
- 结构/引用校验失败：阻断 PR 合并。
- 测试失败：阻断发布。
- 发布成功但 catalog 更新失败：标记 release 为不完整并告警。
- 运行时权限冲突：拒绝执行并记录审计事件。
