---
name: test-plan
description: 测试计划技能。触发词：测试计划、验证方案、回归检查、上线检查。当用户要求为代码变更制定测试范围和验证步骤时激活。
license: "MIT"
metadata:
  version: "1.0"
  author: "devtools-team"
  tags: "test-plan, regression, verification"
---

# Test Plan

为代码变更生成最小但有效的测试计划。

## 使用场景

- 用户需要为 PR 制定验证步骤
- 发布前需要回归测试建议
- 需要按风险排序列出检查项

## 不适用场景

- 需要直接执行自动化测试
- 没有变更上下文，无法判断影响面

## 输出要求

- 区分冒烟测试、核心回归、边界场景
- 标记高风险路径
- 优先给最小必要验证集

## 使用方法

输入应包含：
- 变更说明或 diff
- 影响模块
- 可选发布窗口约束

输出建议结构：

```markdown
## Smoke Tests
- ...

## Regression Tests
- ...

## Edge Cases
- ...
```
