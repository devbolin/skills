# Subagent 编写与协作指南

本文档说明如何在 Skill 体系中设计、配置和使用 Subagent。

> **与 Phase 1 的关系**：Subagent 是 Pack 内的一类能力对象，通过 `agents/<id>.md` 声明，并由 `pack.yaml` 的 `agents[]` 建立索引。运行时由 Agent/Runtime 按需委托执行。
> **相关概念**：[CONCEPTS.md](../CONCEPTS.md) - 核心概念与术语解释

---

## 一、Subagent 定义

Subagent 是在**独立上下文**中执行特定任务的专业化代理。

### 核心特性

| 特性 | 说明 |
|------|------|
| 独立 System Prompt | 不继承主对话的 prompt，隔离上下文 |
| 独立上下文窗口 | 不污染主对话，适合长任务 |
| 可配置模型 | 可选择 Sonnet/Haiku/Opus 等 |
| 工具权限控制 | 精细化权限管理，最小权限原则 |
| 生命周期钩子 | onStart、onEnd 等回调 |
| 可追溯性 | 输入/输出/耗时/错误均可记录 |

---

## 二、Subagent vs Skill

| 维度 | Skill | Subagent |
|------|-------|----------|
| 定位 | 可复用能力单元（Pack 侧） | 运行时委托机制（Agent 侧） |
| 定义位置 | `SKILL.md` + `pack.yaml` | `agents/<id>.md` + `pack.yaml` |
| 触发方式 | AI 自动检测 description | 显式调用或策略触发 |
| 上下文 | 依赖主对话 | 独立上下文 |
| 分发 | 通过 plugin artifact | 随 Pack/plugin 一起分发 |

**关系**：Skill 定义"能力是什么"，Subagent 决定"如何使用这个能力"。

---

## 三、何时使用 Subagent

### 推荐使用

- 任务可清晰切分且边界稳定（如代码审查、文档生成）
- 主流程需要并行化处理多个子任务
- 需要隔离高噪声上下文（大日志、长报告）
- 需要不同模型处理不同子任务

### 不推荐使用

- 子任务与主流程强耦合，需频繁往返确认
- 任务规模太小，委托成本高于直接处理
- 委托边界无法定义，容易反复升级问题

---

## 四、委托边界设计（核心）

每个 Subagent 至少明确四件事：

### 4.1 任务输入

- 接收哪些上下文
- 文件范围（如 `src/payments/**`）
- 目标约束（如"只审查安全问题"）

### 4.2 任务输出

- 必须返回什么格式
- 必需字段（如 `file`、`line`、`severity`、`message`）
- 是否需要证据和定位

### 4.3 禁止行为

- 不可修改什么（如"只读模式，禁止写文件"）
- 不可访问什么（如"禁止访问外部网络"）
- 不可执行什么（如"禁止执行 git push"）

### 4.4 失败策略

- 失败后返回什么
- 是否重试
- 何时回退主流程

---

## 五、Subagent Frontmatter 规范

Subagent 通过文件头 YAML frontmatter 声明配置元数据。以下字段覆盖 Claude Code 和 VS Code Copilot 的主流支持：

| 字段 | 必需 | Claude Code | VS Code | 说明 |
|------|------|-------------|---------|------|
| `name` | 是 | ✅ | ✅ | 小写字母+连字符，唯一标识 |
| `description` | 是 | ✅ | ✅ | 何时委托给此 Subagent |
| `tools` | 否 | ✅ | ✅ | 可用工具列表（allowlist） |
| `model` | 否 | ✅ | ✅ | 模型偏好（`sonnet`/`opus`/`haiku`/`inherit`） |
| `disallowedTools` | 否 | ✅ | ❌ | 禁止的工具列表（denylist） |
| `permissionMode` | 否 | ✅ | ❌ | 权限模式（`auto`/`acceptEdits` 等） |
| `handoffs` | 否 | ❌ | ✅ | VS Code 工作流转移（`label`/`agent`/`prompt`） |
| `mcpServers` | 否 | ✅ | ✅ | MCP 服务器引用或内联定义 |
| `memory` | 否 | ✅ | ❌ | 持久记忆作用域（`user`/`project`） |
| `maxTurns` | 否 | ✅ | ❌ | 最大 agentic 轮次 |

未列出的字段（如 `hooks`、`isolation`、`background` 等）由各平台按需支持，不同平台自动忽略不认 识的字段。

**最小示例（Claude Code + OpenCode）：**
```yaml
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Glob, Grep
model: sonnet
---
```

**兼容 VS Code 的示例：**
```yaml
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Glob, Grep
model: sonnet
handoffs:
  - label: Start Fix
    agent: implementer
    prompt: Implement the fixes suggested above.
---
```

### VS Code 兼容性说明

VS Code 的 custom agents 要求文件扩展名为 `.agent.md`（而非 `.md`），存放目录为 `.github/agents/`。
`agents/` 目录下的裸 `.md` 文件不会被 VS Code 自动发现。

**推荐策略**（与 Phase 1 Plugin-first 理念一致）：
- 源码层统一使用 `agents/<id>.md`（Claude Code / OpenCode 原生格式）
- 构建期转换为 VS Code 格式：`<id>.md` → `.github/agents/<id>.agent.md`
- 转换逻辑在 `scripts/build.py` 中实现

```
源码（平台中立）                 构建产物（平台专属）
  agents/                      
    reviewer.md    ──→  Claude Plugin: agents/reviewer.md
                  ──→  VS Code Plugin: .github/agents/reviewer.agent.md
```

## 六、文件布局与配置模板

### Pack 内文件布局

```text
pack-root/
├── pack.yaml
├── agents/
│   └── review-coordinator.md
└── skills/
    └── code-review/SKILL.md
```

### `pack.yaml` 索引方式

```yaml
agents:
  - id: review-coordinator
    path: agents/review-coordinator.md
```

### `agents/<id>.md` 最小模板

```markdown
# review-coordinator

## 职责
- 负责协调代码审查类任务

## 适用场景
- 需要把代码审查拆成多个子步骤

## 输入
- 代码变更范围
- 审查目标和边界

## 输出
- 审查结论
- 风险列表
- 后续建议

## 禁止行为
- 不直接发布
- 不执行危险命令

## 失败回退
- 无法完成时返回失败原因，并交回主 Agent
```

### 完整示例

```markdown
# review-coordinator

## 职责
- 负责协调 PR 审查、测试计划整理和风险归类

## 适用场景
- 变更文件较多
- 需要先总结 PR，再决定是否进入深度审查

## 输入
- PR diff 或变更文件列表
- 用户关注点，例如安全、回归、测试缺口

## 输出
- `summary`: 变更摘要
- `findings`: 风险与问题列表
- `test_plan`: 建议补充的验证项

## 协作方式
- 需要变更摘要时调用 `pr-summary`
- 需要详细审查时调用 `code-review`
- 需要测试建议时调用 `test-plan`

## 禁止行为
- 不直接修改生产配置
- 不执行发布或推送操作

## 失败回退
- 任一步骤失败时，返回已完成部分和失败原因，由主 Agent 决定是否继续
```

---

## 六、System Prompt 编写规范

System Prompt 是 Subagent 的核心，应包含：

### 6.1 必需元素

| 元素 | 示例 |
|------|------|
| 角色 | "你是代码审查专家" |
| 目标 | "负责分析 PR 变更，发现 bug 和安全漏洞" |
| 约束 | "只读模式，禁止修改文件" |
| 输出协议 | "输出 JSON 数组，包含 file/line/severity/message" |
| 终止条件 | "审查完成后返回汇总报告" |

### 6.2 反例

| 反例 | 问题 |
|------|------|
| "帮我看看代码" | 目标不清 |
| "随便总结一下" | 输出不定 |
| "可以做任何事" | 缺少约束，易越权 |

---

## 七、工具与权限策略

### 最小权限原则

- **默认只读**：不开放写权限
- **默认无网络**：不开放网络访问
- **按需授权**：只开放任务所需工具
- **危险命令阻断**：单独配置

### 权限分级建议

| 任务类型 | 建议权限 |
|----------|----------|
| 审查类 | `file_access:read`、`search` |
| 生成类 | `file_access:read`、`file_access:write:<限定路径>` |
| 运维类 | 需审批或专用受限执行器 |

---

## 八、生命周期与可观测性

### 生命周期钩子

| 钩子 | 时机 | 建议动作 |
|------|------|----------|
| `onStart` | 开始执行前 | 收集最小上下文、记录任务 ID |
| `onEnd` | 执行完成后 | 统一输出摘要、结构化结果 |

### 可观测性记录

每次 Subagent 调用应记录：

- `subagent_name`：Subagent 名称
- `input_summary`：输入摘要（如变更文件数）
- `output_summary`：输出摘要（如发现问题数）
- `duration_ms`：执行时长
- `error_type`：错误类型（如有）
- `fallback_triggered`：是否触发回退

---

## 九、多 Subagent 协作模式

### 9.1 常见架构

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| **Sequential** | A → B → C | 流水线任务，前者输出作为后者输入 |
| **Hub-and-Spoke** | 中心节点调度 | 中央调度并行任务 |
| **Hierarchical** | 嵌套团队 | 复杂任务分解为多层 |

### 9.2 协作示例：Sequential

```
code-reviewer（审查） → test-writer（生成测试） → security-scanner（安全扫描）
```

### 9.3 协作示例：Hub-and-Spoke

```
主 Agent
  ├── code-reviewer
  ├── test-writer
  └── security-scanner
  (主 Agent 收集各 Subagent 结果，汇总输出)
```

---

## 十、与 Skill 的集成

在 `SKILL.md` 中说明 Subagent 使用方式：

```markdown
## Subagent 委托

当输入满足以下条件时，建议委托给 Subagent：
- 变更文件数 > 10
- 涉及 `src/security/**` 或 `src/payments/**`
- 用户明确要求"深度审查"

委托 Subagent：`code-reviewer`
- 输入：PR diff 上下文
- 期望输出：问题列表（JSON）
- 失败回退：主流程最小检查
```

---

## 十一、检查清单

- [ ] 委托边界明确（输入/输出/禁止行为/失败策略）
- [ ] System Prompt 包含角色、目标、约束、输出协议
- [ ] 工具权限最小化并与任务匹配
- [ ] 生命周期钩子定义完整（如适用）
- [ ] 输出可追溯（含证据与定位）
- [ ] 失败时有明确回退路径
- [ ] 与 SKILL.md 的关系已说明
