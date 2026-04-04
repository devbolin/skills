# 阶段一流程文档（GitHub-only, Plugin-first）

## 1. 发布流程

```mermaid
flowchart TD
  A[Developer Update pack.yaml and SKILL.md] --> B[Open PR]
  B --> C[CI Validate Structure and References]
  C --> D{CI Pass}
  D -- No --> E[Block Merge and Return to PR]
  D -- Yes --> F[Codeowners Review]
  F --> G[Merge Main]
  G --> H[Create Tag]
  H --> I[Release Workflow]
  I --> J[Build Plugin Artifact]
  I --> K[Generate Manifest and Checksum]
  I --> L{ENABLE_SKILL_ARTIFACTS=true?}
  L -- true --> M[Build Skill Artifact]
  L -- false --> N[Skip Skill Artifact]
  J --> O[Update Catalog plugin_ref]
  M --> P[Update Catalog skill_ref Optional]
  N --> O
```

失败分支规则：
- CI 失败：阻断合并。
- 审核失败：阻断合并。
- 发布失败：阻断对外可用版本更新。
- catalog 更新失败：标记 release 不完整并告警。

## 2. 调用流程

调用配置细节见 [AGENT_CONSUMPTION.md](./AGENT_CONSUMPTION.md)，本文仅保留统一流程与失败分支。

```mermaid
flowchart TD
  A[Agent Request skill_id and channel_or_version] --> B[Agent or Runtime Read Catalog]
  B --> C{plugin_ref exists}
  C -- No --> X[Fail with catalog error]
  C -- Yes --> D[Select Plugin Artifact]
  D --> E{Policy allows skill_ref and skill_ref exists}
  E -- No --> F[Load Skill from Plugin]
  E -- Yes --> G[Load Skill Artifact]
  F --> H[Execute Skill]
  G --> H
  H --> I[Return Result and Metrics]
```

失败分支规则：
- 无 `plugin_ref`：直接失败。
- `skill_ref` 缺失：回退 plugin 执行，不中断主流程。
- 权限冲突：拒绝执行并记录审计事件。
- 运行时异常：返回错误码并落库指标。

## 3. 回滚流程

```mermaid
flowchart TD
  A[Incident Detected] --> B[Choose Target Stable Version]
  B --> C[Rollback Catalog plugin channel]
  C --> D{skill artifacts enabled}
  D -- true --> E[Rollback Related skill_ref]
  D -- false --> F[Skip skill_ref rollback]
  E --> G[Publish Updated Catalog]
  F --> G
  G --> H[Agent or Runtime Uses Rolled-back Version]
  H --> I[Runtime Recovery]
```

失败分支规则：
- 回滚版本不存在：中止回滚并升级告警。
- 仅 `skill_ref` 回滚失败：保持 plugin 回滚结果为主。
- catalog 发布失败：保留旧稳定通道并阻断切换。

## 4. 失败处理总表

| 失败点 | 处理策略 | 是否阻断 |
|---|---|---|
| 结构/引用校验失败 | PR 直接失败 | 是 |
| 发布阶段验证失败 | 阻断发布 | 是 |
| 发布构建失败 | release 失败告警 | 是 |
| catalog 更新失败 | 标记 release 不完整并告警 | 是 |
| 运行时权限冲突 | 拒绝执行并审计 | 否 |
