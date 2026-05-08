---
name: pr-summary
description: PR 总结技能。触发词：PR 总结、变更摘要、合并说明、风险摘要。当用户要求概括代码变更、整理 review 背景或输出发布摘要时激活。
license: "MIT"
metadata:
  version: "1.0"
  author: "devtools-team"
  tags: ["pr-summary", "changelog", "release-notes"]
---

# PR Summary

生成适合 review、同步和发布说明的 PR 摘要。

## 使用场景

- 用户要求总结 PR 主要变更
- 用户需要面向 reviewer 的风险摘要
- 用户需要合并说明或发布说明草稿

## 不适用场景

- 需要逐行发现 bug 的深度审查
- 没有可用 diff 或变更上下文

## 输出要求

- 先给变更概览
- 再给风险点
- 最后给建议验证项

## 使用方法

输入应包含：
- PR diff、commit range 或变更文件列表
- 可选的目标读者（开发者、reviewer、发布负责人）

输出建议结构：

```markdown
## Summary
- ...

## Risks
- ...

## Suggested Checks
- ...
```
