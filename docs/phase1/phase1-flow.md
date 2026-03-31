# 阶段一流程文档（GitHub-only）

## 1. 开发到发布流程
1. 开发者修改 `skills/<skill-id>/` 并更新 `skill.yaml`。
2. 发起 PR，触发 CI：结构校验、schema 校验、变更 skill 测试。
3. Codeowners 审核通过后合并 `main`。
4. Release workflow 按变更 Skill 打包 artifact，生成 `manifest.json` 和 `checksums.txt`。
5. 创建 Git Tag 与 GitHub Release。
6. 更新 `skill-catalog/index.json` 及 `skills/<skill-id>.json`。

## 2. Agent 调用流程
1. Agent 调用 resolver：`resolve(skill_id, channel|version)`。
2. resolver 读取 `skill-catalog/index.json`，定位 skill 明细文件。
3. 获取目标版本 artifact、adapter、permissions。
4. 执行器下载 artifact 并调用对应 adapter。
5. 返回执行结果并记录日志。

## 3. 回滚流程
1. 将 catalog 的 `channels.stable` 指针回退到上一个稳定版本。
2. 触发 catalog 发布。
3. resolver 读取新索引后自动恢复到旧版本。

## 4. 失败处理规则
- 结构/Schema 失败：阻断 PR 合并。
- 测试失败：阻断发布。
- 发布成功但 catalog 更新失败：标记 release 为不完整并告警。
- 运行时权限冲突：拒绝执行并记录审计事件。
