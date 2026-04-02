---
name: code-review
description: 代码审查技能。触发词：代码审查、PR review、代码检查、安全扫描。当用户要求审查代码、检查潜在问题或分析 PR 时激活。
version: "1.0"
author: "devtools-team"
license: "MIT"
---

# Code Review

自动审查代码质量、安全性和最佳实践。

## 使用场景

- 用户提交 PR 并请求审查
- 用户要求检查代码潜在问题
- 代码合并前进行安全扫描
- Code Review 指定某个文件或 Diff

## 不适用场景

- 紧急 hotfix 需要快速合并（建议单独开启例外流程）
- 纯文档修改（无需技术审查）
- 已知问题正在跟踪中（避免重复审查）

## 审查维度

### 1. 代码质量
- 可读性与可维护性
- 命名规范
- 函数复杂度

### 2. 安全检查
- SQL/Nosql 注入风险
- XSS/CSRF 漏洞
- 敏感信息暴露

### 3. 性能
- 循环效率
- 数据库查询优化
- 缓存使用

## 使用方法

### 自动触发
当检测到 PR 或 Diff 时，主动分析并输出审查报告。

### 手动触发
```
/code-review --file src/auth.py
/code-review --diff HEAD~1
```

## 输出格式

```json
{
  "file": "src/auth.py",
  "issues": [
    {
      "severity": "high",
      "line": 42,
      "rule": "security/sql-injection",
      "message": "Potential SQL injection detected",
      "suggestion": "Use parameterized queries"
    }
  ],
  "summary": {
    "high": 1,
    "medium": 2,
    "low": 3
  }
}
```
