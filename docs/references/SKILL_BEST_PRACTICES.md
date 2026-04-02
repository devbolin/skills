# Skill 与 Subagent 编写最佳实践

> 本文档收集 Skill 和 Subagent 的编写规范与最佳实践，来源：Claude Code 官方文档、Agent Skills 开放标准、社区最佳实践。

---

## 一、SKILL.md 格式与结构

### 1.1 核心文件结构

每个 Skill 是包含 `SKILL.md` 文件的目录：

```
my-skill/
├── SKILL.md          # 必需：元数据 + 指令
├── scripts/          # 可选：可执行脚本
├── references/       # 可选：模板、参考文档
└── INFO.md          # 可选：扩展文档
```

### 1.2 YAML Frontmatter

```yaml
---
name: skill-name            # 必需：小写、数字、连字符
description: 清晰描述何时激活此技能
version: "1.0"              # 可选
author: "Name"              # 可选
license: "MIT"              # 可选
tags: ["tag1", "tag2"]     # 可选
---
```

### 1.3 SKILL.md 示例

```markdown
---
name: invoice-extractor
description: 从发票 PDF 或图片中提取结构化信息。当用户说"提取发票信息"、"报销扫描"、"发票识别"时触发。
version: "1.0"
author: "finance-platform"
license: "Apache-2.0"
---

# Invoice Extractor

从发票中提取商户名称、金额、日期、税率等结构化信息。

## 使用场景

- 用户上传发票图片并要求"提取信息"
- 用户说"报销发票"、"扫描票据"
- 用户需要将发票信息录入系统

## 使用方法

```python
from scripts.run import process
result = process("invoice.pdf")
```

## 注意事项

- 仅处理中文发票
- 不处理手写发票
- 最大支持 10MB 文件
```

---

## 二、Progressive Disclosure 架构

Skill 采用三层渐进式架构，控制 Token 成本：

| 层级 | 组件 | 加载时机 | 内容 |
|------|------|---------|------|
| **L1** | Metadata | 始终加载 | name + description（最小 Token 消耗） |
| **L2** | Core Instructions | 触发时加载 | SKILL.md 正文 - 工作流、步骤、示例 |
| **L3** | Resources | 按需加载 | Scripts、模板、参考文档 |

### 设计原则

- **L1 保持简洁**：description 是 Claude 决定何时激活的关键
- **L2 聚焦清晰**：明确的边界，什么时候用，什么时候不用
- **L3 按需加载**：避免一次性加载所有资源

---

## 三、Skill 描述编写指南

### 3.1 Description 模板

```
[做什么]。[何时使用]。[关键词/触发词]。
```

### 3.2 优秀示例

```
Analyzes git commits and generates changelogs. Use when:
- User asks "what changed in this release"
- Preparing release notes
- Summarizing project activity
Triggers: changelog, release notes, git history, what changed
```

### 3.3 常见错误

| 错误 | 问题 | 修正 |
|------|------|------|
| "helps with coding" | 太模糊 | 具体说明做什么 |
| 范围过大 | 一个 Skill 做所有事 | 拆分成多个专注 Skill |
| 缺少激活上下文 | Claude 无法判断何时触发 | 列出具体触发词 |

### 3.4 Description 编写检查清单

- [ ] 清晰说明 Skill 做什么
- [ ] 列出具体使用场景
- [ ] 包含触发关键词
- [ ] 明确边界（什么时候**不**用）
- [ ] YAML 格式正确（name 匹配目录名）

---

## 四、Subagent 模式与最佳实践

### 4.1 什么是 Subagent

Subagent 是运行在独立上下文窗口中的专业化 AI 助手：

| 特性 | 说明 |
|------|------|
| 独立系统提示 | 不继承主对话的 prompt |
| 独立上下文 | 200k context，不污染主对话 |
| 可配置模型 | Sonnet/Haiku/Opus |
| 工具权限控制 | 精细化权限管理 |
| 生命周期钩子 | onStart、onEnd 等 |
| 跨会话持久化 | 记忆能力 |

### 4.2 Subagent 配置示例

```json
{
  "subagents": {
    "code-reviewer": {
      "description": "专注于发现 bug 和安全问题的代码审查专家",
      "model": "claude-sonnet-4-20250514",
      "systemPrompt": "你是一个代码审查专家，专门擅长发现安全漏洞和 bug...",
      "abilities": ["code_review", "file_access", "web_search"],
      "lifecycleHooks": {
        "onStart": "analyze-pr-context",
        "onEnd": "summarize-findings"
      }
    }
  }
}
```

### 4.3 Subagent 最佳实践

1. **明确边界**：清晰定义何时委托给 Subagent
2. **显式约束**：在 system prompt 中说明只读、不写文件等限制
3. **指定弱点**：说明 Skill 的局限性，避免不必要的调用
4. **独立上下文**：Subagent 不继承主 Claude Code system prompt，只使用自己的

---

## 五、多 Agent 协作模式

### 5.1 常见架构

| 模式 | 说明 | 适用场景 |
|------|------|---------|
| **Sequential Flow** | A -> B -> C | 流水线任务 |
| **Two-Agent Chat** | 双向对话 | 需要协商的场景 |
| **Hub-and-Spoke** | 中心节点协调 | 中央调度 |
| **Peer-to-Peer** | 点对点网络 | 去中心化 |
| **Hierarchical** | 嵌套团队 | 复杂任务分解 |

### 5.2 信息流模式

- **Bidirectional**：双向信息交换
- **Unidirectional**：单向流动

### 5.3 Agent 间通信方式

- 通过共享上下文/记忆系统
- 通过触发其他 Agent 的工具调用
- 通过传递任务结果作为下一个 Agent 的输入

---

## 六、Skill vs Prompt Engineering

| 方面 | Prompt Engineering | Skill Engineering |
|------|-------------------|-------------------|
| 关注点 | 如何思考 | 如何行动 |
| 可复用性 | 单次会话 | 跨会话持久 |
| 触发方式 | 手动输入 | AI 自动检测 |
| 作用域 | 对话级别 | 模块化、可组合 |
| 包含内容 | 仅指令 | 指令 + 脚本 + 资源 |

---

## 七、相关资源

| 资源 | 原始文档 | 说明 |
|------|----------|------|
| [Claude Code 最佳实践](https://code.claude.com/docs/en/best-practices) | code.claude.com/docs/en/best-practices | Claude Code 官方最佳实践指南 |
| [Anthropic Skills 示例](https://github.com/anthropics/skills) | github.com/anthropics/skills | Anthropic 官方 Skills 示例仓库 |
| [Awesome Agent Skills](https://github.com/VoltAgent/awesome-agent-skills) | VoltAgent/awesome-agent-skills | 社区收集的 Agent Skills 列表 |
| [Agent Skills 开放标准](https://agentskills.io) | agentskills.io | Agent Skills 开放标准规范 |
| [Anthropic Claude 文档](https://docs.anthropic.com) | docs.anthropic.com | Claude API 和工具使用文档 |
| [Model Context Protocol](https://modelcontextprotocol.io) | modelcontextprotocol.io | MCP 官方规范和实现指南 |

---

## 八、编写检查清单

### SKILL.md

- [ ] frontmatter 格式正确（name 匹配目录名）
- [ ] description 清晰、具体、包含触发词
- [ ] 列出明确的使用场景
- [ ] 提供代码示例
- [ ] 说明边界和限制

### Subagent

- [ ] 有清晰的职责边界
- [ ] system prompt 包含约束条件
- [ ] 指定了合适的模型
- [ ] 工具权限最小化
- [ ] 定义了生命周期钩子（如适用）
