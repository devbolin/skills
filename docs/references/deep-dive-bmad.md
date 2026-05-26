# 深度调查：BMAD Method（Build More Architect Dreams）

## 基本信息

| 项目 | 内容 |
|------|------|
| 仓库 | https://github.com/bmad-code-org/BMAD-METHOD |
| ⭐ Stars | ~37K |
| Fork 率 | 12.4%（四个框架中最高） |
| Open Issues | 38（四个框架中最少） |
| 许可证 | MIT |
| 首次发布 | 约 2024.12 |
| 当前版本 | V6 |
| 安装 | `npx bmad-method install`（需要 Node.js 20+） |
| 安装耗时 | 小时级（需配置 12+ 角色） |
| 平台支持 | Claude Code（推荐）、Cursor、Codex CLI |
| 核心模块 | BMM（核心，34+ 工作流）、BMB（Builder）、TEA（测试）、BMGD（游戏）、CIS（创意智能） |

## 起源与哲学

### 核心理念

BMAD（Breakthrough Method for Agile AI-Driven Development，全称 Build More Architect Dreams）是一个将 AI 代理组织为完整敏捷团队的**开源方法论框架**。

核心洞察：**AI 工具的能力取决于你给它的指令质量。** 如果你打开 Claude Code 说"给我做个应用"，你能得到一些东西，但很可能不是你真正需要的——因为你跳过了弄清楚"你到底需要什么"的步骤。

BMAD 的解决方式：用结构化的流程和角色专业化，将 AI 从"快速打字员"转变为真正的开发伙伴。

### 核心哲学

1. **文档优先** — AI 遵循文档化规格比遵循对话指令要好得多。对话会丢失上下文，文件不会
2. **角色专业化** — 每个代理有明确职责和领域专长，而不是万能通用代理
3. **制品即契约** — 每个阶段的输出（PRD、ADR、Stories）是下一阶段的输入，也是长期可复用的知识资产
4. **规模自适应** — 小变更走 Quick Flow（3 步），大变更走 Full Method（4 阶段），自动调整计划深度
5. **完整生命周期** — 从 Analysis（分析）到 Implementation（实现）到 Retro（回顾），全覆盖
6. **反馈循环** — 当实现发现需要变更规格时，规格会被更新。制品是活的

### 与其他框架的本质区别

- **vs Superpowers**：Superpowers 约束"怎么写代码"，BMAD 管理"整个项目怎么做"
- **vs Spec Kit**：Spec Kit 以文档为中心，BMAD 以角色和流程为中心
- **vs gstack**：gstack 是工具集合（28 个命令），BMAD 是团队模拟（21 个角色）

## 架构设计

### 模块化架构

```
BMad Method Ecosystem
│
├── BMM (BMad Method) — 核心框架
│   ├── 34+ 结构化工作流
│   ├── 4 阶段方法论
│   └── 专有 Agent 角色配置
│
├── BMB (BMad Builder) — 扩展工具
│   ├── 创建自定义 Agent
│   ├── 创建自定义工作流
│   └── 扩展 BMad 模块
│
├── TEA (Test Architect) — 测试架构师
│   ├── 基于风险的测试策略
│   ├── 自动测试生成
│   └── 覆盖率分析
│
├── BMGD (Game Dev Studio) — 游戏开发
│   ├── Unity 工作流
│   ├── Unreal 工作流
│   └── Godot 工作流
│
└── CIS (Creative Intelligence Suite) — 创意智能
    ├── 创新思维工作流
    ├── 头脑风暴引导
    └── 设计思维方法论

Web Bundles（可选）
├── Google Gemini Gems
└── ChatGPT Custom GPTs
    └── 将规划阶段打包到 Web LLM，节省 IDE token
```

### 21 个专业代理

BMAD 的角色体系是其最大特色和最显著的优势。每个代理有独立的系统提示，优化用于其特定角色：

| 代理名 | 角色 | 职责 | 关键能力 |
|--------|------|------|---------|
| Mary | Analyst | 需求发现、领域调研 | 利益相关者访谈、竞争分析、可行性评估 |
| John | Product Manager | 产品管理 | PRD 编写、用户故事、MoSCoW 优先级排序 |
| Winston | Architect | 系统设计 | ADR、组件图、技术决策、Readiness 验证 |
| Amelia | Developer | 实现 | Story 准备、TDD 实现、代码审查 |
| Sally | UX Designer | 用户体验 | 用户流程、线框图、设计模式 |
| Paige | Technical Writer | 技术写作 | 文档、API 文档、图表、标准 |
| — | Scrum Master | 项目管理 | Sprint 规划、Story 拆分、回顾 |
| — | QA Engineer | 质量保证 | 测试策略、对抗性审查、自动化测试 |
| — | DevOps Engineer | 运维 | CI/CD、基础设施、部署 |
| — | Security Analyst | 安全 | 威胁建模、安全审查 |
| — | Business Analyst | 业务分析 | 需求收集、流程建模 |
| — | Solo Dev | 快速通道 | 小变更时担当全流程 |

### 50+ 结构化工作流

BMAD 提供超过 50 个预定义工作流，引导代理完成特定任务：

| 工作流 | 用途 |
|--------|------|
| Requirements Workshop | 从模糊想法中提取需求 |
| Architecture Decision Record | 记录技术决策和理由 |
| Story Decomposition | 将 Epic 拆解为可实现的 Story |
| Code Review Checklist | 系统化的安全/性能/可维护性审查 |
| Incident Postmortem | 生产事故的结构化分析 |
| Sprint Retrospective | 识别流程改进点 |

### 安装结构

```
npx bmad-method install 后在你的项目中创建：

bmad/
├── agents/             # 各角色的系统提示和配置
│   ├── analyst.md
│   ├── pm.md
│   ├── architect.md
│   ├── dev.md
│   ├── qa.md
│   └── ...
├── workflows/          # 50+ 结构化工作流程
│   ├── requirements-workshop.md
│   ├── story-decomposition.md
│   └── ...
├── memory/             # 项目上下文和知识
│   ├── project-context.md
│   └── decisions/
└── config.toml         # 用户配置
```

## 完整工作流详解

### 双路径决策

BMAD 最重要的设计决策之一：**不是所有变更都走完整流程。**

```
New Change / 新变更
    │
    ├── Small / 小型变更 → Quick Flow（快速通道）
    │   ├── Who: Solo Dev 单代理
    │   ├── Steps: Quick Spec → Quick Dev → Code Review
    │   ├── 跳过: Analysis, Planning, Solutioning
    │   └── 不跳过: 测试、构建、类型检查
    │
    └── Large / 大型变更 → Full Method（完整方法）
        ├── Greenfield（新项目）→ 4 阶段
        └── Brownfield（已有代码）→ 先生成项目上下文 → 3-4 阶段
```

#### Quick Flow 细则

适用于：孤立 bug 修复、配置变更、< 3 文件的简单功能

流程：
1. **Quick Spec** — 创建简化的 tech-spec 或 story
2. **Quick Dev** — 实现变更
3. **Code Review** — 代理自我对抗审查

质量门禁不跳过：测试、构建、类型检查仍然必需。

#### 规模判断标准

| 场景 | 路径 |
|------|------|
| 孤立 bug 修复 | Quick Flow |
| 配置变更 | Quick Flow |
| 简单功能（< 3 文件） | Quick Flow |
| 多端点新功能 | Full Method |
| 架构重构 | Full Method |
| 新 Epic / 模块 | Full Method |
| 影响安全/认证的变更 | Full Method |
| 不确定 | 走 Full Method |

### Full Method — 四阶段详解

#### Phase 1: Analysis（分析阶段）

**角色**：Analyst（Mary）
**目标**：验证想法、理解问题域、确认"做对了事情"

流程：
1. 领域调研：搜索市场、竞品、技术方案
2. 用户画像：谁在用？解决什么痛点？
3. 可行性评估：技术上可行吗？资源够吗？
4. 成功指标：怎么衡量成功？

输出：**Product Brief**
- 产品愿景
- 用户画像
- 关键假设
- 成功指标
- 约束条件

#### Phase 2: Planning（规划阶段）

**角色**：PM（John）+ UX Designer（Sally）
**目标**：将想法转化为精确可执行的规格

流程：
1. PM 创建 Product Requirements Document（PRD）
   - 所有功能需求使用 Given/When/Then 格式
   - 功能矩阵 + MoSCoW 优先级
   - MVP 范围定义
2. UX Designer 创建设计规格
   - 用户流程和交互模式
   - 组件系统
   - 设计系统规范
3. 规格评审和确认

输出：**PRD + UX Specs**
- 功能需求和用户故事
- 验收标准
- 设计规范和模式

#### Phase 3: Solutioning（方案设计阶段）

**角色**：Architect（Winston）
**目标**：确定技术架构，在编码前发现设计问题

流程：
1. **ADR（Architecture Decision Records）**
   - 记录每个技术决策及其理由
   - 例如：为什么用 PostgreSQL 而不是 MongoDB
2. **系统设计**
   - 组件图和交互图
   - API 契约和数据模型
   - 集成点和依赖关系
3. **Epic / Story 分解**
   - 将 PRD 拆解为 Epics
   - 每个 Epic 拆解为 Stories
   - 每个 Story 有明确的 DoR（Definition of Ready）
4. **Readiness 验证**
   - 架构对齐检查
   - 依赖就绪检查
   - 风险识别

输出：**ADRs + Stories**
- 架构决策记录
- 按优先级排序的开发故事

#### Phase 4: Implementation（实现阶段）

**角色**：Scrum Master + Developer（Amelia）+ QA
**目标**：按 Story 逐个实现、测试、交付

详细循环：

```
┌─────────────────────────────────────────────────┐
│  Scrum Master                                   │
│   ├── 规划 Sprint（确定当前 Sprint 的 Story）      │
│   └── 创建 Story 文件（含完整上下文）               │
│                                                    │
│  Architect (Winston)                              │
│   └── 验证 Definition of Ready（DoR）              │
│       ├── OK → 进入开发                            │
│       └── Failed → 返回 Story 修正                  │
│                                                    │
│  Developer (Amelia)                               │
│   └── TDD 实现（每个 Acceptance Criteria）           │
│       ├── RED: 写测试 → 确认失败                    │
│       ├── GREEN: 实现 → 确认通过                    │
│       └── REFACTOR: 重构 → 确认仍通过               │
│                                                    │
│  QA Engineer                                       │
│   └── 对抗性代码审查                               │
│       ├── OK → 质量门禁                            │
│       └── Fix → Developer 修正                      │
│                                                    │
│  质量门禁                                          │
│   ├── 测试全部通过                                  │
│   ├── 构建成功                                     │
│   └── TypeCheck 通过                               │
│       ├── Passed → Commit + 同步制品                │
│       └── Failed → Developer 修正                    │
└─────────────────────────────────────────────────┘
```

#### Brownfield（已有代码）特化

当代码已经存在于生产环境中时，流程调整：

```
Existing Code
    │
    ├── 步骤 0: 生成项目上下文
    │   ├── 分析现有代码库的模式、技术栈、约定
    │   └── 输出 project-context.md
    │
    ├── 需要 PRD？→ Phase 2: Planning
    │
    ├── Phase 3: Solutioning（架构）
    │
    └── Phase 4: Implementation（实现）
```

关键区别：不需要 Phase 1（分析），因为领域已经已知。上下文来自代码库本身。

### 制品传递链

BMAD 最核心的设计：

```
Phase 1: Analysis
    │
    ├── Product Brief
    │   ↓
Phase 2: Planning
    │
    ├── PRD + UX Specs（详细产品需求 + 设计规格）
    │   ↓
Phase 3: Solutioning
    │
    ├── ADRs + Stories（架构决策 + 开发故事）
    │   ↓
Phase 4: Implementation
    │
    ├── Code + Tests（代码 + 测试）
    ├── Sprint Status（状态跟踪）
    │
    └── 反馈循环 ← 当实现发现需要变更规格时，更新上游制品
```

制品是活文档。反馈循环确保规格与代码保持一致性。

### Sprint Status 作为真相源

每个 Story 的状态集中跟踪在一个文件中：

```yaml
development_status:
  # Epic 1: Authentication
  epic-1: in-progress
  1-1-user-registration: done
  1-2-user-login: in-progress
  1-3-password-reset: ready-for-dev
  1-4-oauth-integration: backlog

  # Epic 2: Dashboard
  epic-2: backlog
  2-1-dashboard-layout: backlog
```

状态流转：**backlog → ready-for-dev → in-progress → review → done**

## 独特设计

### 1. Party Mode（派对模式）

多个代理角色在同一会话中协作讨论。描述一个问题，Master Agent 选择相关代理进行辩论。

例如：PM 可能推动某个功能，Architect 可能提出可扩展性担忧，Developer 可能建议简化实现。他们**真的会互相反驳**，从而发现单个对话永远不会发现的盲点。

*实际案例*：在使用 Party Mode 评估一个认证方案时，Architect 代理提出了一个 Developer 忽略的会话存储问题。

### 2. 规模自适应（Scale-Domain-Adaptive）

根据项目复杂度自动调整计划深度。
- Bug fix → 最浅
- 中小功能 → Quick Flow
- 全新项目 → Full Method
- 企业系统 → Full Method + 多模块

### 3. 双路径（Quick Flow vs Full Method）

不是所有变更都值得完整的 4 阶段流程。BMAD 是唯一一个明确提供"轻量路径"的方法论框架。

### 4. Web Bundles

将 BMad 的规划阶段打包为 Google Gemini Gems 或 ChatGPT Custom GPTs。
- 在 Web LLM（订阅制，固定费用）中完成规划
- 将成品制品导入 IDE 进行实现
- 节省 IDE token 成本

### 5. 最高社区 Fork 率（12.4%）

说明被二次开发/定制最多。社区活跃度（Discord、YouTube 教程）是四个框架中最强的。

### 6. 最少 Open Issues（38）

可能说明：维护者控制严格，或 bug 较少。

## 优缺点分析

### 优点

1. **最完整的全生命周期覆盖** — 从 Analysis 到 Implementation 到 Retro，所有阶段都有角色和工作流
2. **角色专业化** — 21 个专业代理产生比通用代理更高质量的输出
3. **制品长期可复用** — PRD、ADR、Stories 是项目知识资产，不随会话消失
4. **Party Mode** — 多代理辩论机制发现盲点
5. **文档优先** — 规范化文档减少 AI 幻觉和需求漂移
6. **规模自适应** — Quick Flow 避免了对小变更的过度处理
7. **反馈循环** — 制品会随实现发现而更新，保持一致性
8. **社区最活跃** — 最高 Fork 率、教程最丰富

### 缺点

1. **配置复杂** — 12+ 角色需要调优，安装需要小时级
2. **对 <5 人团队臃肿** — 除非项目够大，否则角色过多
3. **Token 消耗极大** — 完整流程多次对话，API 成本高
4. **学习曲线陡峭** — 需要理解 4 阶段、21 角色、50+ 工作流
5. **不覆盖部署** — 没有 CI/CD、监控、生产验证功能
6. **Quick Flow 和 Full Method 的决策本身也需要经验**
7. **语言障碍** — 中文资料较少，社区以英文为主

## 优劣场景

| 场景 | 推荐度 |
|------|--------|
| 全新大型 Greenfield 项目 | ⭐⭐⭐⭐⭐ |
| 5-20 人团队（需模拟完整团队） | ⭐⭐⭐⭐⭐ |
| 需要完整文档跟踪的项目 | ⭐⭐⭐⭐⭐ |
| Solo 开发者 | ⭐⭐ |
| 快速原型 | ⭐⭐ |
| 小团队小项目 | ⭐⭐⭐ |
| 需要合规/审计的项目（文档化） | ⭐⭐⭐⭐⭐ |

## 与其他框架的关系

BMAD 作者推荐的组合方式：
- **BMAD 做规划**（Phase 1-3）
- **Superpowers 做工程**（Phase 4 中使用 TDD + 审查）
- **gstack 做交付**（QA + 部署 + 监控）

## 数据来源

- 官方仓库：https://github.com/bmad-code-org/BMAD-METHOD
- 官方文档：https://docs.bmad-method.org/
- DeepWiki：https://deepwiki.com/bmad-code-org/BMAD-METHOD
- Diego Rodrigo 工作流详解：*BMAD in Practice: The Complete AI Agent Development Workflow* — https://diegorodrigo.dev/en/2026/04/06/bmad-in-practice-the-complete-ai-agent-development-workflow/
- Hammad Haqqani 分析：*BMAD Method: The Framework That Turns Claude Code Into a Complete Agile Development Team* — https://hammadhaqqani.com/blog/bmad-method-claude-code-agile-development
- Mathieu Mafille 实践：*BMAD Method — Structured AI Development From Idea to Code* — https://www.mafille.me/posts/2026/bmad-method-structured-ai-development
- Ry Walker Research：*Agentic Skills Frameworks Compared* — https://rywalker.com/research/agentic-skills-frameworks
