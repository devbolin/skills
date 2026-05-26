# 深度调查：gstack（garrytan/gstack）

## 基本信息

| 项目 | 内容 |
|------|------|
| 仓库 | https://github.com/garrytan/gstack |
| 创建者 | Garry Tan（Y Combinator 总裁兼 CEO） |
| ⭐ Stars | ~65K |
| 许可证 | MIT |
| 首次发布 | 约 2026.03 |
| 安装耗时 | 30 秒 |
| 技能数量 | 28+（23 slash commands + 8 power tools + CLIs） |
| 平台支持 | Claude Code, Codex CLI, Cursor, OpenCode, Factory Droid, Slate, Kiro, Hermes, GBrain（共 10 种） |
| 前置依赖 | Claude Code、Git、Bun v1.0+ |

### 关键数据

- **60 天 shipping 600K+ 行生产代码**（35% 测试代码）
- **每日生产力** 10,000-20,000 行/天（兼职）
- **2026 年截至 4 月 18 日** 的生产力是 2013 年全年的 **240 倍**
- **Logical change 跑分**：2026 年约 11,417 logical 行/天 vs 2013 年 14 行/天（约 **810 倍**）

## 起源与哲学

### 起源

gstack 是 YC（Y Combinator）总裁兼 CEO Garry Tan 个人使用的 Claude Code 工具集，在 2026 年 3 月开源。

Garry Tan 的背景：
- YC 总裁兼 CEO（前合伙人）
- Palantir 早期工程师/PM/设计师
- Posterous 联合创始人（出售给 Twitter）
- YC 内部社交网络 Bookface 的创建者

他的动机来自著名的 Andrej Karpathy 语录：
> *"I don't think I've typed like a line of code probably since December, basically, which is an extremely large change."*

Garry Tan 的个人数据表明一个人在正确工具辅助下可以多快交付：
- 2026 年（60 天内）shipping 3 个生产服务 + 40+ 功能（兼职，同时全职管理 YC）
- 他自己的估计："810 倍于 2013 年的生产力"

### 核心理念

1. **Boil the Lake** — 当完成的成本只比捷径多花几分钟时，永远选择完成。在 AI 时代，"需要 2 周"应该重新定义为"需要 1 小时"。追求 100% 完成而不是 90% 方案。

2. **Search Before Building** — "别人解决过吗？"永远优先于"让我从头设计"。三层知识：
   - 第一层（已验证）：广泛使用、经过实战检验的模式
   - 第二层（新兴）：最新的最佳实践（需要批判性评估）
   - 第三层（第一性原理）：针对特定问题的原始推理

3. **流程即 Sprint** — 技能不是零散的命令，而是一个 Sprint 流程：
   **Think → Plan → Build → Review → Test → Ship → Reflect**

4. **跨模型审计** — Claude 写的代码让 Codex 复审，反之亦然

5. **gstack 不是一个提示集合，它是一个软件工厂**

### 与其他框架的本质区别

- **vs Superpowers**：Superpowers 是流程约束系统，gstack 是角色模拟系统
- **vs BMAD**：BMAD 模拟 21 个虚拟角色（"团队"），gstack 提供 28 个 slash command 角色（"工具"）
- **vs Spec Kit**：Spec Kit 以文档为中心，gstack 以命令驱动的工作流为中心

gstack 最独特的地方在于：它**有实际的生产力数据验证**，而不仅仅是理论框架。

## 架构设计

### 整体架构

gstack 的架构分为三个层次：

```
┌─────────────────────────────────────────────────────────────┐
│                 Slash Commands 层 (28+ 技能)                  │
│                                                              │
│  Think & Plan (6)          │  Code Review (3)               │
│  ├─ /office-hours          │  ├─ /review                    │
│  ├─ /plan-ceo-review       │  ├─ /investigate               │
│  ├─ /plan-eng-review       │  └─ /codex                     │
│  ├─ /plan-design-review    │                                 │
│  ├─ /plan-devex-review     │  Design (3)                    │
│  └─ /autoplan              │  ├─ /design-consultation       │
│                             │  ├─ /design-shotgun            │
│  Test & Security (3)       │  └─ /design-html               │
│  ├─ /qa                    │                                 │
│  ├─ /qa-only               │  Ship & Deploy (5)             │
│  └─ /cso                   │  ├─ /ship                      │
│                             │  ├─ /land-and-deploy           │
│  Safety (4)                │  ├─ /canary                    │
│  ├─ /careful               │  ├─ /benchmark                 │
│  ├─ /freeze                │  └─ /document-release           │
│  ├─ /guard                 │                                 │
│  └─ /unfreeze              │  Meta (4)                      │
│                             │  ├─ /retro                     │
│  Multi-Agent               │  ├─ /learn                      │
│  └─ /pair-agent            │  ├─ /gstack-upgrade             │
│                             │  └─ /context-restore           │
├─────────────────────────────────────────────────────────────┤
│                Power Tools 层 (Browser + CLIs)                │
│                                                              │
│  /browse — 内置 Chromium 浏览器自动化                         │
│  │  ├─ 持久化守护进程（无冷启动）                               │
│  │  ├─ a11y tree 元素引用系统（@e1, @e2）                      │
│  │  ├─ Cookie 导入（Chrome/Arc/Brave/Edge）                   │
│  │  ├─ 仅 localhost 绑定 + Bearer token 认证                  │
│  │  └─ ~100ms/命令，比 MCP 节约 3-4 万 token                  │
│                                                              │
│  /open-gstack-browser — GStack Browser                       │
│  │  ├─ 侧边栏 mini-agent                                     │
│  │  ├─ Anti-bot stealth                                      │
│  │  ├─ 自动模型路由（Sonnet 做动作, Opus 做分析）               │
│  │  └─ $B handoff 处理 MFA/CAPTCHA                           │
│                                                              │
├─────────────────────────────────────────────────────────────┤
│               CLI 工具层 (独立命令行工具)                        │
│                                                              │
│  gstack-model-benchmark — 跨模型基准测试                      │
│  │  Claude / GPT (via Codex) / Gemini, 比较延迟/token/成本    │
│  │  --dry-run 验证配置                                         │
│                                                              │
│  gstack-taste-update — 设计品味学习                            │
│  │  记录你在 /design-shotgun 中的选择/拒绝                     │
│  │  跨项目持久化，随时间衰减（5%/周）                            │
│  │  影响未来的 UI 变体生成                                     │
│                                                              │
│  gstack-ios-qa-daemon — iOS 真机测试守护进程                   │
│  │  USB CoreDevice 驱动真实 iPhone                            │
│  │  三层能力：observe / interact / mutate / restore            │
└─────────────────────────────────────────────────────────────┘
```

### 安装结构

```bash
# 全局安装（30 秒）
git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack && ./setup

# 团队安装（共享给队友）
cd ~/.claude/skills/gstack && ./setup --team
# 会创建 .claude/ 元数据，队友自动安装
# 静默每小时更新检查

# 特定代理安装
./setup --host codex     # Codex CLI
./setup --host opencode  # OpenCode
./setup --host cursor    # Cursor
```

## 完整工作流详解

gstack 的工作流是一条从产品想法到生产部署的端到端管道。

### Stage 1: Think（思考阶段）

#### /office-hours — YC Office Hours

**角色**：YC 合伙人模拟
**目标**：在写任何代码之前重新定义产品方向

这是 gstack 的入口点。通过 6 个 forcing questions 来重构你的产品思路：

1. 你真正想解决什么痛点？（不是功能，是痛点）
2. 谁有这个痛点？多痛？
3. 现在的解决方案是什么？为什么不够好？
4. 你最窄的可行方案是什么？
5. 这个方案需要什么前提条件？
6. 你怎么知道做对了？

两种模式：
- **Startup Mode**：尖锐的 PMF（产品市场契合）问题
- **Builder Mode**：探索性和生成性方法

输出：设计文档，自动馈送到下游技能。

**关键行为**：如果用户说"我想建一个每日简报应用"，/office-hours 可能会说："你说的不是每日简报，你描述的是一个个人 AI 助手。让我们重新定义问题。"

#### /plan-ceo-review — CEO/Founder 模式

**角色**：CEO / 创始人
**目标**：重新思考问题，找到隐藏在请求中的"10 星产品"

4 种范围模式：
1. **Expansion** — 大胆扩展（增加功能）
2. **Selective Expansion** — 精选机会
3. **Hold Scope** — 当前范围内最高质量
4. **Reduction** — 最小可行版本

10 维度审查：市场、用户、竞争、技术、风险等。

### Stage 2: Plan（规划阶段）

#### /plan-eng-review — 工程经理

**角色**：Engineering Manager
**目标**：在编码前锁定架构

生成：
- ASCII 系统图（数据流、状态转换、边界情况、错误路径）
- 测试矩阵
- 自动传递给 /qa 的测试计划

#### /plan-design-review — 高级设计师

**角色**：Senior Designer
**目标**：在编码前发现设计问题

行为：
- 7 个设计维度评分（0-10）
- 解释每个维度的"10 分"长什么样
- AI Slop 检测（发现模糊的 AI 生成描述）
- 交互式：每个设计选择问一个问题

7 个维度：布局、色彩、排版、交互、响应式、无障碍、空状态/错误状态

#### /plan-devex-review — 开发者体验

**角色**：Developer Experience Lead
**目标**：确保开发者首次体验流畅

20-45 个 forcing questions，三种模式：
- DX EXPANSION
- DX POLISH
- DX TRIAGE

#### /autoplan — 自动计划管道

自动串联 CEO → Design → Engineering 审查。只向用户展示"品味决策"（taste decisions），技术决策自动处理。

### Stage 3: Design（设计阶段）

#### /design-consultation — 设计合伙人

**角色**：Design Partner
**目标**：从头创建完整的设计系统

- 竞争对手设计调研
- 排版/色彩/间距系统
- HTML 预览生成
- 输出 DESIGN.md

#### /design-shotgun — 设计探索

**角色**：Design Explorer
**目标**：生成多个设计变体

- 生成 4-6 个 AI mockup 变体
- 在浏览器中打开比较板
- 收集反馈，迭代
- Taste memory（gstack-taste-update）记住偏好

#### /design-html — 设计工程

**角色**：Design Engineer
**目标**：将 mockup 转化为可部署的 HTML

- Pretext computed layout（文字重排、高度自适应）
- 30KB，零依赖
- 自动检测 React/Svelte/Vue
- 输出可直接上线

### Stage 4: Code Review（代码审查阶段）

#### /review — Staff Engineer

**角色**：Staff Engineer
**目标**：发现 CI 查不出但生产环境会爆的 bug

检测模式：
- N+1 queries
- 竞态条件
- 注入漏洞
- 边界情况遗漏

自动修复明显问题。标记完整性缺口。

#### /investigate — Debugger

**角色**：Debugger
**目标**：系统化根因调试

铁律：**没有调查就不能修复。**
1. 追踪数据流
2. 测试假设
3. 3 次修复失败后重新评估架构

#### /codex — 第二意见

**角色**：独立审阅者（OpenAI Codex CLI）
**目标**：跨模型审计

三种模式：
1. **review** — 通过/失败门禁
2. **adversarial** — 对抗性挑战
3. **consultation** — 开放式咨询

当 /review 和 /codex 都已运行时，分析结果重叠和独特发现。

### Stage 5: Test & Security（测试与安全阶段）

#### /qa — QA Lead

**角色**：QA Lead
**目标**：功能测试 + 回归测试 + bug 自动修复

三种模式：
- **Diff-aware**（默认）— 仅测试 git diff 影响的页面。基于 git diff 智能确定测试范围
- **Full** — 完整探索（5-15 分钟），记录 5-10 个问题
- **Quick**（--quick）— 30 秒冒烟测试

自动行为：
- 发现 bug → 自动修复 → 原子 commit → 重新验证
- 自动生成回归测试

#### /qa-only — 仅报告

与 /qa 相同方法，但只生成错误报告，不修改代码。

#### /cso — 首席安全官

**角色**：Chief Security Officer
**目标**：安全审计

- OWASP Top 10 + STRIDE 威胁模型
- 17 个误报排除规则
- 置信度门禁 8/10+
- 独立发现验证
- 每个发现包含具体的攻击场景

### Stage 6: Ship & Deploy（交付与部署阶段）

#### /ship — Release Engineer

**角色**：Release Engineer
**目标**：一键从代码到 PR

顺序：
1. 同步 main 分支
2. 运行测试
3. 审计覆盖率
4. 创建 PR

如果项目没有测试框架，/ship 会自动引导一个。

#### /land-and-deploy — 部署工程师

**角色**：Release Engineer
**目标**：从 PR 到生产

顺序：
1. 合并 PR
2. 等待 CI
3. 部署
4. 验证生产健康

#### /canary — SRE

**角色**：Site Reliability Engineer
**目标**：部署后监控

- 监控控制台错误
- 性能回归
- 页面失败
- 持续监控直到确认稳定

#### /benchmark — 性能工程师

**角色**：Performance Engineer
**目标**：性能基线

- 页面加载时间
- Core Web Vitals
- 资源大小
- 每次 PR 前后对比

#### /document-release — 技术写作

**角色**：Technical Writer
**目标**：自动更新文档

- 捕获过时的 README
- 构建 Diataxis 覆盖图（reference / how-to / tutorial / explanation）
- 在 PR body 中显示缺口

### Stage 7: Reflect（回顾阶段）

#### /retro — 工程经理

**角色**：Engineering Manager
**目标**：每周回顾

- 按人统计产出
- 贡献连续性（shipping streaks）
- 测试健康趋势
- 成长机会
- `/retro global` 跨所有项目和 AI 工具汇总

### Safety Guardrails（安全护栏）

gstack 提供独有的安全机制：

| 命令 | 功能 |
|------|------|
| `/careful` | 在破坏性命令前警告（rm -rf, DROP TABLE, force-push 等） |
| `/freeze <dir>` | 限制编辑范围到特定目录 |
| `/guard` | /careful + /freeze 组合 |
| `/unfreeze` | 移除 /freeze 锁 |

## 独特设计

### 1. Boil the Lake 哲学

> "当完成的成本只比捷径多花几分钟时，永远选择完成。在 AI 辅助时代，'需要 2 周'应该重新定义为'需要 1 小时'。"

这不仅仅是口号——体现在 /ship 的行为中：
- 如果项目没有测试框架，/ship 会自动创建
- 如果文档过时，/document-release 会自动更新
- /qa 发现 bug 后自动修复而不是只报告

### 2. 跨模型审计（Cross-Model Auditing）

gstack 是唯一内置跨模型审计能力的框架：
- Claude 实现 → Codex（/codex）独立复审
- /codex 三种模式：review / adversarial / consultation
- 两种模型都运行后，分析发现的**重叠**和**独特发现**

这意味着：**同时利用两个不同的 AI 模型，他们犯同样的错误的概率更低。**

### 3. 内置浏览器自动化

gstack 自带完整的 Chromium 浏览器自动化系统（/browse），与其他框架对比：

| 特性 | gstack (/browse) | MCP Browser |
|------|-----------------|-------------|
| Token 开销 | 约 100ms，几乎为零 | 每次会话 3-4 万 token |
| 冷启动 | 持久化守护进程，无冷启动 | 每次新建 |
| 元素引用 | a11y tree + @e1/@e2 refs | DOM mutation |
| Cookie 导入 | Chrome/Arc/Brave/Edge | 通常需要手动 |
| 安全性 | localhost-only + Bearer token | 取决于实现 |

### 4. Continuous Checkpoint（连续检查点）

可选模式（`gstack-config set checkpoint_mode continuous`）：
- 技能自动 WIP commit 工作进度
- commit 包含 `[gstack-context]` 结构体（决策、剩余工作、失败方法）
- 崩溃和上下文切换后可以恢复
- `/context-restore` 读取这些 commit 重建会话状态
- `/ship` 过滤合并 WIP commit，保留非 WIP commit

### 5. 设计品味记忆（Taste Memory）

gstack-taste-update 记录你在 /design-shotgun 中的选择：
- 哪些变体你选择了（approve）
- 哪些你拒绝了（reject）
- 跨项目持久化
- 随时间衰减（5%/周）
- 影响未来的 UI 变体生成

### 6. Conductor 集成

macOS 应用，同时运行 10-15 个并行 Claude Code 会话：
- 每个会话独立的 git worktree 隔离工作区
- 实时监控进度
- 代码审查和合并

### 7. YoE 数据验证

gstack 是唯一一个有实际生产力数据验证的框架：

| 指标 | 数值 |
|------|------|
| 时间跨度 | 60 天（兼职） |
| 生产代码 | 600K+ 行 |
| 测试代码 | ~210K 行（35%） |
| 生产服务 | 3 个 |
| 功能交付 | 40+ |
| 2013 vs 2026 对比 | 810x 生产力提升 |

## 优缺点分析

### 优点

1. **安装最快**（30 秒）
2. **技能最丰富**（28+ slash commands + 8 power tools + CLIs）
3. **唯一覆盖 QA → 安全 → 部署 → 监控全链的框架**
4. **内置浏览器 QA** — 真 UI 测试，非模拟
5. **跨模型审计** — Claude + Codex 双验证
6. **安全护栏** — /careful /freeze /guard 独一无二
7. **有实际数据验证** — 600K 行 / 60 天的数据背书
8. **Boil the Lake 哲学** — 自动补全缺失的基础设施
9. **YC 思维** — /office-hours 重新定义产品方向

### 缺点

1. **YC 风格** — /office-hours 的说教感（"你说的不是 X，你是说 Y"）
2. **重度依赖 Claude Code** — 虽然支持 10 种代理，但核心能力依赖 Claude
3. **快速迭代不稳定** — 命令和标志可能在版本间变化
4. **部分技能需要 Bun/Node** — 额外依赖
5. **Conductor 需要 macOS** — Windows/Linux 用户不能使用并行会话
6. **没有内置 AGENTS.md 管理** — 没有 Superpowers 那种 1% 自动触发机制
7. **学习成本** — 28+ 命令需要时间记忆
8. **Token 消耗高** — 每个技能读取大量上下文

## 适用场景

| 场景 | 推荐度 |
|------|--------|
| Solo 开发者想快速交付 | ⭐⭐⭐⭐⭐ |
| 创始人 / 独立开发者 | ⭐⭐⭐⭐⭐（YC 思维匹配） |
| 需要 QA + 安全 + 部署全链 | ⭐⭐⭐⭐⭐（唯一选择） |
| 快速原型 → 生产 | ⭐⭐⭐⭐⭐ |
| 团队协作（Conductor） | ⭐⭐⭐⭐ |
| 严格 TDD 要求的项目 | ⭐⭐⭐（/review 验证，但无强制） |
| 企业级规范开发 | ⭐⭐⭐ |
| 小项目 / 单文件修复 | ⭐⭐⭐（技能偏重） |

## 推荐使用路径

gstack 官方推荐的新手路径：
1. 安装（30 秒）
2. 运行 `/office-hours` —— 在真实用户痛点上，而不是功能愿望清单
3. 运行 `/review` —— 在真实的分支上
4. 如果觉得好用，添加 `/qa` 在 staging URL 上
5. 最后 `/ship`

全功能流程：
```
/office-hours → /autoplan → 实现代码 → /review → /qa → /cso → /ship → /canary
```

## 数据来源

- 官方仓库：https://github.com/garrytan/gstack
- 技能文档：https://github.com/garrytan/gstack/blob/main/docs/skills.md
- HARULOG 完整指南：*The Complete gstack Guide: How to Turn Claude Code into a Virtual Engineering Team* — https://www.harulogs.com/forum/blog/the-complete-gstack-guide-how-to-turn-claude-code-4347
- explainx.ai 分析：*gstack: Garry Tan's open-source "software factory" for Claude Code* — https://explainx.ai/blog/gstack-garry-tan-claude-code-skills-factory
- youmind 分析：*gstack 深度解析：YC 总裁如何利用 AI 每天编写 10,000 行代码* — https://youmind.com/blog/gstack-garry-tan-claude-code-workflow-guide
- Ry Walker Research：*Agentic Skills Frameworks Compared* — https://rywalker.com/research/agentic-skills-frameworks
- LOC 争议回应：*On the LOC Controversy*（gstack 仓库内）
