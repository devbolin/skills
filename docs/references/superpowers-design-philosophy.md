# Superpowers 设计核心理念

## 背景与问题

AI 编码代理（如 Claude Code、Codex、Gemini CLI）在独立工作时面临一系列固有缺陷：

- **缺乏判断力** — 代理无法区分"好代码"与"能跑的代码"
- **缺乏项目上下文** — 每个会话都是全新的，没有长期记忆
- **缺乏成本意识** — 不知道何时该停、何时该问
- **偏好"直接写代码"** — 默认跳过需求分析、设计、测试
- **缺乏纪律性** — 不会主动遵循 TDD、也不会主动进行系统化调试

Superpowers 的核心问题：**如何让一个有能力的"没有经验的初级工程师"行为像一个资深工程师？**

答案是：**不要告诉他"该做什么"，而是强迫他走正确的工作流。**

## 设计目标

1. **流程强制优于自觉** — 工作流不是建议，是规则
2. **技能自动匹配** — 代理不需要手动选择，系统根据上下文自动触发
3. **可组合、可扩展** — 技能是独立的 markdown 文件，可自由组合
4. **跨平台兼容** — 同一套技能工作在 Claude Code、Codex、Gemini CLI、OpenCode 等多个平台
5. **证据驱动** — 成功需要验证，不能空口声称
6. **最小认知负担** — 每个任务 2-5 分钟，每一步只需关注当前步骤

## 核心理念

### 1. 流程 > 直觉（Process over Ad-hoc）

代理的天性是"遇到问题直接写代码"。Superpowers 的做法是拦截这个冲动，强制走流程：

```
构思 → 设计 → 计划 → 实现 → 审查 → 完成
```

每个阶段由一个专门的技能负责，技能之间通过文档（设计文档、实现计划）传递上下文。

### 2. 技能即约束（Skills as Constraints）

每个技能是一份 markdown 文件，包含：
- 触发条件（什么场景下自动激活）
- 强制步骤（必须遵循的流程）
- 检查清单（完成前必须验证的事项）

技能不是"建议"，是"规则"。代理被要求 **"Follow skill exactly"**。

### 3. 1% 规则（The 1% Rule）

即使只有 1% 的可能性某个技能适用，**也必须加载并检查。** 这确保了：

- 代理不会因为"觉得不需要"而跳过关键流程
- 思维模式从"要不要用"转变为"为什么不检查"

### 4. 计划驱动（Plan-then-Execute）

不允许边写边想。任何实现工作前必须经过：

| 步骤 | 输出 |
|------|------|
| brainstorming | 设计文档（design.md） |
| writing-plans | 实现计划（plan.md），每个任务 2-5 分钟 |
| 实施 | 按计划逐任务执行 |

### 5. 子代理与审查（Subagent + Review）

单一代理的工作质量受限于其上下文窗口。Superpowers 将大任务拆解为小任务，每个小任务由一个**全新的、无偏见的子代理**执行，然后经过两阶段审查：

1. **Spec Compliance** — 实现是否符合设计要求？
2. **Code Quality** — 代码是否整洁、是否遵循现有模式？

审查不通过则标记为 Blocking，阻塞后续任务。

### 6. RED-GREEN-REFACTOR（TDD 铁律）

测试先行，不可协商：

| 阶段 | 动作 | 验证 |
|------|------|------|
| RED | 写一个失败的测试 | 测试必须失败 |
| GREEN | 写最简代码让它通过 | 测试必须通过 |
| REFACTOR | 重构代码 | 测试仍然通过 |

**在写测试之前写的代码，必须删除。**

### 7. YAGNI + DRY

- **YAGNI（You Aren't Gonna Need It）** — 只实现当前任务需要的功能，不提前做"未来可能需要"的设计
- **DRY（Don't Repeat Yourself）** — 不重复已有的模式、代码、配置

### 8. 证据 > 声称（Evidence over Claims）

任何声称"完成了"或"修复了"的声明，必须有可验证的证据：

- 测试通过（具体的命令输出）
- 功能可运行（截图、浏览器验证）
- Lint/TypeCheck 通过

使用 `verification-before-completion` 技能强制执行。

### 9. 隔离工作区（Worktree Isolation）

每个新功能在独立的 git worktree 中开发，确保：

- 当前工作不干扰主分支
- 可以同时并行多个功能
- 完成后可以独立选择 merge、PR、或丢弃

### 10. 可组合性（Composability）

技能之间通过标准化的接口（文件）通信：

```
brainstorming → design.md
writing-plans → plan.md
subagent-development → task execution + review
finishing → merge/PR/cleanup
```

这种设计使得技能可以像乐高一样组合：

- 简单任务：brainstorming → writing-plans → executing-plans
- 复杂任务：brainstorming → writing-plans → subagent-driven-development

## 落地机制

### 技能触发流程

```
用户消息 → 检查技能（1% 规则） → 加载技能 → 遵循技能 → 输出结果
```

### 检查清单（TodoWrite）

每个技能如果包含检查清单，代理必须创建 TodoWrite 逐项跟踪，完成后标记。

### 平台适配

技能定义使用 Claude Code 的工具命名，其他平台（Copilot CLI、Codex、Gemini CLI、OpenCode）通过工具映射适配：

| 技能中的工具 | OpenCode | Copilot CLI | Gemini CLI |
|-------------|----------|-------------|------------|
| TodoWrite | todowrite | — | — |
| Task | task | — | activate_skill |
| Read/Write/Edit/Bash | 原生工具 | 原生工具 | 原生工具 |

## 总结

Superpowers 的本质是**用流程弥补 AI 代理的缺陷**。它不试图让代理变得更聪明，而是：

1. **阻止代理做错误的事**（跳过设计、不写测试）
2. **迫使代理做正确的事**（需求分析、计划、TDD、验证）
3. **让正确的事变得容易**（自动触发、小任务、隔离环境）

整套系统的核心思想可以浓缩为一句话：

> **不要信任代理的判断，信任流程的约束。**
