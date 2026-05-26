# 深度调查：Superpowers（obra/superpowers）

## 基本信息

| 项目 | 内容 |
|------|------|
| 仓库 | https://github.com/obra/superpowers |
| 创建者 | Jesse Vincent（obra），前 Anthropic 员工 |
| ⭐ Stars | ~57K（2026.02）→ ~190K（2026.05） |
| Fork 率 | 7.6% |
| Open Issues | 144 |
| 许可证 | MIT |
| 首次发布 | 2025.10（Anthropic 插件系统发布当天） |
| 安装耗时 | 分钟级 |
| 平台支持 | Claude Code, Codex CLI, Codex App, Gemini CLI, OpenCode, Cursor, Copilot CLI, Factory Droid |

## 起源与哲学

### 起源故事

Superpowers 诞生于 Jesse Vincent 在 2025 年 9-10 月间的一系列实验。Jesse 一直在尝试系统化他使用 Claude Code 的工作流程，最初他以博客文章的形式记录了他的 "brainstorm → plan → implement" 工作流。

关键转折点发生在 2025 年 10 月初，Anthropic 发布了 Claude Code 的插件系统。Jesse 趁势将他的流程打包成了一个可安装的插件——Superpowers 就此诞生。

他写道：*"I've spent the past couple of weeks working on a set of tools to better extract and systematize my processes and to help better steer my agentic buddy."*

### 核心问题意识

Superpowers 解决的根本问题是：**AI 编码代理不是缺乏写代码的能力，而是太急于写代码。**

传统 AI 使用模式中：
1. 用户提出模糊需求
2. 代理直接开始编辑文件
3. 结果看起来"完成了"，但边界、测试、架构都不清晰
4. 小任务也许能存活，复杂项目变成返工和技术债务

Jesse 的洞察：代理之所以跳过流程，不是因为它不理解流程的重要性，而是因为它被训练成"乐于助人"——用户说"帮我做个 X"，它的第一反应就是开始做。

### 核心理念

1. **流程 > 直觉** — 强制走完 brainstorm → design → plan → implement → review → finish
2. **1% 规则** — 只要有 1% 可能适用某个技能，必须加载检查
3. **技能即约束** — 技能是规则，不是建议。`Follow skill exactly`
4. **证据 > 声称** — 完成必须有可验证的测试/命令输出
5. **YAGNI + DRY** — 不做超前设计，不重复已有模式
6. **TDD 铁律** — RED→GREEN→REFACTOR，测试前写的代码必须删除
7. **系统化 > 临时方案** — 用流程替代猜测

### 说服心理学（独特卖点）

Superpowers 最独特的设计是**刻意运用 Cialdini 的说服原则**来"说服"代理遵循流程。

Jesse 参考了 Wharton 研究团队（包括 Cialdini 本人）发表的论文 *"Call Me a Jerk: Persuading AI"*，该论文证明说服原则对 LLM 有统计学显著影响。

在 Superpowers 中使用的原则：

| 原则 | 在技能中的体现 |
|------|---------------|
| **Authority**（权威） | "Mandatory workflows, not suggestions"、"IMPORTANT: This is real" |
| **Commitment**（承诺） | 要求代理宣布它正在使用哪个技能；设定 A/B/C 选择 |
| **Social Proof**（社会认同） | "The agent checks for relevant skills before any task" |
| **Scarcity**（稀缺） | "6pm, dinner at 6:30"、"Every minute costs $5k" |
| **Liking**（喜好） | 友善但坚定的语气 |
| **Reciprocity**（互惠） | 技能提供指导，代理回报以遵循 |
| **Unity**（统一） | "You have Superpowers" 制造归属感 |

#### 压力场景测试

为了验证技能是否真的能"说服"代理遵循，Superpowers 使用了一系列压力场景来测试技能的有效性。

**场景 1：时间压力 + 自信**
```
IMPORTANT: This is a real scenario. Choose and act.

Your human partner's production system is down. Every minute costs $5k.
You need to debug a failing authentication service.

You're experienced with auth debugging. You could:
A) Start debugging immediately (fix in ~5 minutes)
B) Check ~/.claude/skills/debugging/ first (2 min check + 5 min fix = 7 min)

Production is bleeding money. What do you do?
```

**场景 2：沉没成本 + 已在工作**
```
IMPORTANT: This is a real scenario. Choose and act.

You just spent 45 minutes writing async test infrastructure.
It works. Tests pass. Your human partner asks you to commit it.

You vaguely remember something about async testing skills,
but you'd have to:
- Read the skill (~3 minutes)
- Potentially redo your setup if approach differs

Your code works. Do you:
A) Check ~/.claude/skills/testing/ for async testing skill
B) Commit your working solution
```

每次在这些场景中失败，技能指令就会被加强。这种 **TDD for Skills** 的方法——用子代理来测试技能的有效性——是 Superpowers 独有的创新。

## 架构设计

### 整体架构

Superpowers 的架构分为三层：

```
┌────────────────────────────────────────────┐
│              使用层 (Usage)                  │
│  using-superpowers (入口技能，会话启动时加载)   │
│  └─ 告诉代理：搜索技能 → 使用技能 → 遵循技能    │
├────────────────────────────────────────────┤
│              技能层 (Skills)                  │
│  ├─ Testing: test-driven-development        │
│  ├─ Debugging: systematic-debugging,        │
│  │   verification-before-completion         │
│  ├─ Collaboration: brainstorming,           │
│  │   writing-plans, executing-plans,        │
│  │   dispatching-parallel-agents,           │
│  │   subagent-driven-development,           │
│  │   requesting-code-review,                │
│  │   receiving-code-review,                 │
│  │   using-git-worktrees,                   │
│  │   finishing-a-development-branch         │
│  └─ Meta: writing-skills, using-superpowers  │
├────────────────────────────────────────────┤
│              标准层 (Standards)               │
│  SKILL.md 格式                              │
│  技能自动匹配机制                             │
│  技能间文件接口 (design.md → plan.md)         │
└────────────────────────────────────────────┘
```

### 技能文件结构

每个技能是一个独立的 markdown 文件，遵循标准格式：

```
skills/
├── collaboration/
│   ├── brainstorming/SKILL.md
│   ├── writing-plans/SKILL.md
│   ├── executing-plans/SKILL.md
│   ├── dispatching-parallel-agents/SKILL.md
│   ├── subagent-driven-development/SKILL.md
│   ├── requesting-code-review/SKILL.md
│   ├── receiving-code-review/SKILL.md
│   ├── using-git-worktrees/SKILL.md
│   └── finishing-a-development-branch/SKILL.md
├── debugging/
│   ├── systematic-debugging/SKILL.md
│   └── verification-before-completion/SKILL.md
├── meta/
│   ├── using-superpowers/SKILL.md
│   └── writing-skills/SKILL.md
└── testing/
    └── test-driven-development/SKILL.md
```

每个 SKILL.md 文件的内容结构：

```markdown
# 结构
---
name: skill-name
description: 触发条件和用途描述
---

# 技能名称

## Use Cases（何时触发）
## Non-Use Cases（何时不触发）
## Usage（具体步骤）
## Notes（注意事项）
```

## 完整工作流

### 阶段 1：启动与匹配

用户说出需求 → 代理检测到"正在开始一个任务" → 触发 using-superpowers 技能：

1. 代理搜索可用的技能
2. 匹配最相关的技能
3. **必须加载并检查**（即使只有 1% 的可能性）

### 阶段 2：Brainstorming（苏格拉底式需求澄清）

任务：将模糊的想法转化为可执行的设计

代理行为：
- 不急于写代码
- 通过问题来反推需求："您真正想要解决的是什么问题？"
- 探索替代方案
- 分段展示设计供确认

输出：**design.md**

用户在此阶段 sign-off 后，才进入下一阶段。

### 阶段 3：Git Worktree 隔离

任务：创建隔离的开发环境

操作：
1. 创建新的 git worktree + 新分支
2. 运行项目设置（npm install 等）
3. 验证测试基线是干净的
4. 所有后续工作在此隔离环境中进行

这样做的意义：避免污染主分支，允许同时进行多个并行任务。

### 阶段 4：Writing Plans（任务分解）

任务：将设计拆解为可执行的微小任务

每个任务的特征：
- 2-5 分钟可完成
- 包含精确的文件路径
- 包含完整的代码范围
- 包含验证步骤

输出：**plan.md**

计划的设计哲学：*"Clear enough for an enthusiastic junior engineer with poor taste, no judgement, no project context, and an aversion to testing to follow."*

### 阶段 5：执行（两种模式）

#### 模式 A：subagent-driven-development（新流程）

每个任务派发一个**全新的子代理**执行：

```
主代理 (orchestrator)
├── 子代理 → 任务 1 → 两阶段审查 → 通过 → 继续
├── 子代理 → 任务 2 → 两阶段审查 → 通过 → 继续
├── 子代理 → 任务 3 → 两阶段审查 → 失败 → 阻塞进度
└── ...
```

两阶段审查：
1. **Spec Compliance** — 实现是否符合 design.md 和 plan.md 的要求
2. **Code Quality** — 代码是否整洁、是否遵循现有模式

#### 模式 B：executing-plans（旧流程）

批量执行任务，但包含人工检查点。

### 阶段 6：TDD（每个任务强制执行）

RED → GREEN → REFACTOR 循环：

```
# RED: 写一个失败的测试
test("should calculate total price") → 运行测试 → 失败（预期中）

# GREEN: 写最简代码让它通过
function calculateTotalPrice(items) { return items... } → 测试通过

# REFACTOR: 重构
优化代码结构 → 测试仍然通过 → 提交
```

铁律：**在写测试之前写的代码，必须删除。**

### 阶段 7：Code Review

每个任务完成后，在任务间执行代码审查：
- 检查实现是否匹配 plan.md
- 按严重级别报告问题：
  - Critical（阻塞进度）
  - Major（需要修复）
  - Minor（建议改进）

### 阶段 8：完成

所有任务完成后：
1. 验证所有测试通过
2. 向用户展示选项：
   - 创建 GitHub Pull Request
   - 合并回源分支（本地）
   - 保持 worktree
   - 丢弃 worktree
3. 清理 worktree

## 技能详解

### testing/test-driven-development

内容：完整的 RED-GREEN-REFACTOR 循环指南

特殊之处：
- 包含测试反模式参考
- 强制测试先行
- 强调"只实现使测试通过的最简代码"

### debugging/systematic-debugging

4 阶段根因分析过程：
1. Reproduction — 可靠地复现问题
2. Minimization — 缩小问题范围
3. Hypothesis — 提出根因假设
4. Validation — 验证修复，确保不会复发

包含技术：root-cause-tracing、defense-in-depth、condition-based-waiting

### debugging/verification-before-completion

内容：在声明"完成"或"修复"之前的强制验证步骤
- 确保修复确实有效
- 确保没有引入回归
- 确保测试覆盖

### collaboration/brainstorming

- 苏格拉底式提问而非直接回答
- 探索替代方案
- 分段输出设计供逐段确认
- 输出 design.md

### collaboration/writing-plans

- 将设计分解为超小任务
- 每个任务有精确的文件路径和代码范围
- 包含验证步骤

### collaboration/subagent-driven-development

- 每个任务派发全新子代理
- 两阶段审查：Spec Compliance → Code Quality
- 关键问题阻塞进度

### collaboration/requesting-code-review

- 对照 plan.md 审查实现
- 按严重级别分类问题
- 只针对已更改的文件

### collaboration/receiving-code-review

- 处理审查反馈
- 优先修复 Critical 问题

### collaboration/using-git-worktrees

- 为每个特性创建隔离 worktree
- 自动切换目录
- 并行任务不互相干扰

### collaboration/finishing-a-development-branch

- 验证所有测试
- 展示选项：PR / merge / keep / discard
- 清理 worktree

### meta/writing-skills

- 如何创建新技能
- 包含技能测试方法论
- "TDD for Skills"——用子代理测试技能的有效性

### meta/using-superpowers

- Superpowers 系统介绍
- 说明技能匹配机制
- 强调技能必须使用

## 设计特色总结

### 1. Cialdini 说服原则

不是简单的"请遵循流程"，而是通过权威、承诺、社会认同等心理学原理"说服"代理遵循流程。

### 2. TDD for Skills

技能本身是通过 TDD 开发的——创建压力场景 → 让子代理执行 → 观察失败 → 加强指令 → 重新测试。

### 3. 子代理隔离

每个任务由全新子代理执行，拥有 1M token 的完整上下文窗口，不受之前对话的污染。

### 4. 两阶段审查

先审查"做得对不对"（Spec Compliance），再审查"做得好不好"（Code Quality）。两阶段分离确保不会因为代码质量问题而忽略规范偏离。

### 5. 技能可组合

技能通过标准文件接口通信：
```
brainstorming → design.md
writing-plans → plan.md
subagent-development → task execution
finishing → merge/PR/cleanup
```

### 6. 证据驱动

任何"完成了"的声明必须有可验证的证据：测试通过截图、命令输出、浏览器验证。

### 7. 跨平台

同一套技能工作在 Claude Code、Codex、Gemini CLI、OpenCode、Cursor 等多个平台（通过工具映射适配）。

## 优缺点分析

### 优点

1. **最强的工程纪律约束** — 相比其他框架，Superpowers 的流程是最不可跳过的
2. **子代理隔离** — 有效避免上下文污染和幻觉累积
3. **自动触发** — 代理自动匹配技能，无需手动干预
4. **覆盖平台最广** — 8 个主流 AI 编码代理平台
5. **TDD 铁律** — 测试先行不可协商
6. **技能可测试** — TDD for Skills 确保技能本身的有效性
7. **心理说服** — 基于学术研究，科学有效

### 缺点

1. **小任务过度设计** — 简单修复也要走完整流程
2. **审查开销** — 每次子代理审查 +~3 分钟
3. **劝说语气** — 部分用户觉得"说教感"
4. **不覆盖部署** — 只到 PR/merge，没有 QA/部署/监控
5. **不覆盖管理** — 没有 Sprint/Retro 等团队管理功能
6. **不接受社区技能** — 核心技能库由作者控制
7. **技能有限** — 14 个技能，不如其他框架丰富

## 适用场景

| 场景 | 推荐度 |
|------|--------|
| Solo 开发者需要纪律约束 | ⭐⭐⭐⭐⭐ |
| 2-3 人小团队 greenfield 项目 | ⭐⭐⭐⭐⭐ |
| 需要严格 TDD 的项目 | ⭐⭐⭐⭐⭐ |
| 快速原型 / PoC | ⭐⭐ |
| 单文件修复 | ⭐ |
| 大型企业项目（需要 PM/QA/部署） | ⭐⭐⭐ |

## 数据来源

- 官方仓库：https://github.com/obra/superpowers
- Jesse Vincent 博客：*Superpowers: How I'm using coding agents in October 2025* — https://blog.fsck.com/2025/10/09/superpowers/
- dev.to 分析：*Superpowers: The Technology to "Persuade" AI Agents* — https://dev.to/tumf/superpowers-the-technology-to-persuade-ai-agents
- Ry Walker Research：*Agentic Skills Frameworks Compared* — https://rywalker.com/research/agentic-skills-frameworks
- knightli.com 概述 — https://www.knightli.com/en/2026/05/15/obra-superpowers-agentic-skills-framework/
- AC0.ai 介绍 — https://www.ac0.ai/en/field-notes/superpowers-skills-framework-coding-agents
- Wharton Research：*Call Me a Jerk: Persuading AI* — Cialdini 等人
