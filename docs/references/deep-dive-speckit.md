# 深度调查：Spec Kit（github/spec-kit）

## 基本信息

| 项目 | 内容 |
|------|------|
| 仓库 | https://github.com/github/spec-kit |
| 创建者 | GitHub（官方出品） |
| ⭐ Stars | ~71K（2026.02）→ ~97K（2026.05） |
| Fork 率 | 8.6% |
| Open Issues | 632（四个框架中最多） |
| 首次发布 | 约 2025.08 |
| 核心工具 | Specify CLI（Python，通过 uv 安装） |
| 安装耗时 | 分钟级 |
| 平台支持 | 30+ AI 编码代理集成（Copilot, Gemini, Codex, Claude, Windsurf, Cursor 等） |
| 社区 | 91 Extensions, 18 Presets, 200+ Contributors |

## 起源与哲学

### 起源

Spec Kit 是 GitHub 官方出品的开源工具包，旨在将 **Spec-Driven Development（SDD，规范驱动开发）** 方法论落地为可执行的工具。

SDD 的提出是对"Vibe Coding"（氛围编程）问题的直接回应：

> *"You prompted the agent. It wrote 400 lines. Half of them solved the wrong problem. Welcome to vibe coding at scale — the thing every team learned to regret in 2025."*

传统 AI 编程模式中，系统的"真相"存在于一个每次上下文窗口填满就会重置的聊天历史中。代理会忘记，你也会忘记。新功能与旧功能矛盾。SDD 使真相成为一个磁盘上的文件。

### 核心理念：权力反转（Power Inversion）

SDD 最根本的哲学是**权力反转**：

> **传统模式：** 代码是真相源，规格是写完就扔的脚手架。  
> **SDD 模式：** 规格是真相源，代码是规格生成的可执行制品。

这意味着：
- **规格是可执行资产** — 精确到可以直接由 AI 代理生成代码
- **意图驱动开发** — 用自然语言表达意图，AI 负责实现
- **单一真相源** — 维护软件就是维护 spec.md 和 plan.md。调试就是修复生成错误代码的规格或计划
- **Spec 与 Plan 严格分离** — spec.md 只描述"什么"（纯功能），plan.md 只描述"怎么"（纯技术）

### 批判性观点

一位实践者（LPains）的观察：
> *"Spec Kit 感觉类似于 TDD 或 BDD——它更多是关于你的思维方式，而不是你生产什么。"*

另一个重要观察：**SDD 不会消除编码的复杂性，它只是将复杂性从编码阶段转移到了规格阶段。**

## 架构设计

### 整体架构

Spec Kit 的架构分为三层：

```
┌──────────────────────────────────────────────────────┐
│                  CLI 层 (Specify CLI)                    │
│  ┌──────────────────────────────────────────────────┐  │
│  │ specify init        — 脚手架初始化                │  │
│  │ specify integration — 管理代理集成                │  │
│  │ specify extension   — 管理扩展                    │  │
│  │ specify preset      — 管理预设                    │  │
│  │ specify workflow    — 管理工作流                  │  │
│  │ specify check       — 检查配置                    │  │
│  └──────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────┤
│               Slash Commands 层 (7 个命令)               │
│  ┌──────────────────────────────────────────────────┐  │
│  │ /speckit.constitution  — 治理原则（一次性）          │  │
│  │ /speckit.specify       — 编写规格                   │  │
│  │ /speckit.clarify       — 澄清模糊点（可选）           │  │
│  │ /speckit.plan          — 技术实现计划                │  │
│  │ /speckit.tasks         — 生成任务列表               │  │
│  │ /speckit.analyze       — 跨制品一致性分析（可选）      │  │
│  │ /speckit.implement     — 执行实现                    │  │
│  └──────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────┤
│               扩展层 (Extensions + Presets)              │
│  ┌──────────────────────────────────────────────────┐  │
│  │ Extensions — 添加新命令/新能力                     │  │
│  │   ├─ CI Guard              — CI 合规门禁           │  │
│  │   ├─ Architecture Guard    — 架构合规门禁           │  │
│  │   └─ ...91 个社区扩展                              │  │
│  ├──────────────────────────────────────────────────┤  │
│  │ Presets — 修改现有行为                            │  │
│  │   ├─ AIDE        — 7 步 AI 驱动工程生命周期        │  │
│  │   ├─ Canon       — 基线驱动工作流                  │  │
│  │   ├─ Product Forge — PM 导向 SDD                  │  │
│  │   └─ ...18 个社区预设                              │  │
│  └──────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

### 文件结构

```
my-project/
├── .specify/
│   ├── memory/
│   │   └── constitution.md    # 项目治理原则（永久上下文）
│   ├── specs/
│   │   ├── 001-feature-name/  # 每个功能一个独立文件夹
│   │   │   ├── spec.md        # 功能规格（WHAT）
│   │   │   ├── plan.md        # 实现计划（HOW）
│   │   │   └── tasks.md       # 任务分解
│   │   └── 002-next-feature/
│   │       └── ...
│   ├── templates/             # 核心模板
│   │   ├── spec-template.md
│   │   ├── plan-template.md
│   │   └── tasks-template.md
│   ├── scripts/               # 辅助脚本
│   ├── presets/               # 预设配置
│   └── extensions/            # 扩展配置
└── ...your code...
```

### 模板解析优先级

Spec Kit 的定制系统使用优先级堆叠：

| 优先级 | 组件 | 位置 |
|--------|------|------|
| ⬆ 1 | 项目本地覆盖（Project Override） | `.specify/templates/overrides/` |
| 2 | 预设（Presets） | `.specify/presets/templates/` |
| 3 | 扩展（Extensions） | `.specify/extensions/templates/` |
| ⬇ 4 | 核心（Core） | `.specify/templates/` |

模板在运行时解析，Spec Kit 从栈顶向下搜索，使用第一个匹配项。

## 完整工作流详解

### 一次性设置（Setup）

#### Step 0: 安装 Specify CLI

```bash
# 安装 uv 包管理器
curl -LsSf https://astral.sh/uv/install.sh | sh

# 安装 Specify CLI
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 新项目
specify init my-project --ai claude

# 已有项目
specify init . --ai copilot  # 或 gemini / cursor / codex
```

`--ai` 参数告诉 Spec Kit 要集成到哪个代理。Spec Kit 会将 slash command 文件写入代理的配置目录。

#### Step 1: 创建 Constitution（治理原则）

```bash
/speckit.constitution
```

创建 `.specify/memory/constitution.md`，定义项目必须遵守的根本规则。

**示例 Constitution：**
```markdown
- TypeScript only, strict mode
- No external state management libraries
- Every feature must have integration tests
- Accessibility: WCAG 2.1 AA minimum
- No telemetry without explicit user opt-in
- API responses under 200ms
- Minimum 80% code coverage
```

Constitution 一旦创建，**所有后续的 spec、plan、tasks 都会自动被检查是否符合它**。

### 每个功能的重复流程（4 步循环）

#### Step 2: Specify（编写规格）

```bash
/speckit.specify Build an app to organize photos into albums. Users can drag
photos between albums on the main page. Each album shows a tiled preview
of its contents, grouped by the date the photos were taken.
```

**关键规则：在 spec 中禁止提及技术栈。** 只描述用户价值。

Spec Kit 自动：
1. 创建新功能目录 `specs/001-albums/`
2. 创建 git 分支 `001-feature-name`
3. 基于模板生成 `spec.md`

**spec.md 内容结构：**
- User Stories（用户故事）
- Requirements（功能性和非功能性需求）
- Success Criteria（成功标准）
- Edge Cases（边界情况）

#### Step 2b: Clarify（澄清 — 可选但推荐）

```bash
/speckit.clarify
```

代理重新读取 spec.md 并发现模糊点。会提出尖锐问题：
- "照片从一张专辑删除但另一张专辑仍存在时，会发生什么？"
- "没有网络连接时行为如何？"

每个问题提供选项或自由文本回答。这个步骤专门用于在编码前发现 bug。

#### Step 3: Plan（技术实现计划）

```bash
/speckit.plan Use Vite + React + TypeScript. Store metadata in SQLite via
better-sqlite3. Image files live on the local filesystem.
```

**关键规则：plan 必须引用 spec 来证明每个技术选择。**

代理：
1. 验证技术选择符合 Constitution
2. 生成 plan.md，包含：
3. Tech Stack 和架构决策理由
4. 数据模型设计
5. 项目文件结构
6. Component Tree
7. 分阶段组织（Phase 0: Research, Phase 1: Foundation, Phase 2: Implementation）

**Spec vs Plan 的严格分离：**

| 制品 | 必须包含 | 禁止包含 |
|------|---------|---------|
| spec.md | 用户价值、功能需求、成功标准 | 编程语言、框架、数据库 |
| plan.md | 技术栈、架构决策、库选择 | 偏离 spec 的功能描述 |

这种分离使得一个 spec 可以有多个并行 plan（Creative Exploration）。

#### Step 4: Tasks（生成任务）

```bash
/speckit.tasks
```

将 plan 拆解为具体、按依赖排序的任务列表。

**tasks.md 的格式：**
```markdown
## Task 1: Set up database schema
- Create migration for albums table
- Create migration for photos table
- Define foreign keys and indexes
- Done when: migration runs cleanly and schema matches plan.md

## Task 2: Create album API endpoints
- GET /api/albums — list all albums
- POST /api/albums — create album
- Done when: all endpoints return correct data with tests

...
```

任务通常 15-40 个，每个任务有明确的"Done when"条件。

#### Step 4b: Analyze（一致性分析 — 可选但推荐）

```bash
/speckit.analyze
```

在开始实现前，进行跨制品一致性分析：
- spec.md 和 plan.md 有没有矛盾？
- tasks.md 是否完整覆盖了 spec 的所有要求？
- 是否违反 constitution？
- 是否有覆盖缺口？

如果发现问题，迭代修正，然后重新生成 tasks。

#### Step 5: Implement（执行实现）

```bash
/speckit.implement
```

代理按 tasks.md 从上到下逐个执行。每个任务完成后标记完成。

**重要警告：** 不要让代理无人值守地运行 30 个任务的列表。建议：
1. 先执行 3-5 个任务
2. 审查结果
3. 调整 Constitution
4. 逐步扩大

推荐的实现节奏（结合 TDD）：
1. `/speckit.specify` 写 spec.md
2. `/speckit.plan` 在 plan.md 中添加测试策略章节
3. `/speckit.tasks` 在每个实现任务前生成一个测试任务
4. `/speckit.implement` 先写测试，再写代码，然后验证

### 完整流程示意图

```
一次性设置:
  specify init
  └── /speckit.constitution

每个功能:
  /speckit.specify ─→ spec.md
       │
       ├── (可选) /speckit.clarify → 发现模糊点
       │
       ▼
  /speckit.plan ───→ plan.md
       │
       ▼
  /speckit.tasks ──→ tasks.md
       │
       ├── (可选) /speckit.analyze → 跨制品一致性
       │
       ▼
  /speckit.implement
       ├── Phase 0: Setup / Research
       ├── Phase 1: Foundation
       ├── Phase 2: User Stories
       ├── Phase 3: Testing
       └── Phase 4: Polish
```

## 独特设计

### 1. Constitution 治理

与其他框架不同，Spec Kit 有一个全局的"宪法"来统辖所有功能开发：
- 一次性设置
- 所有后续的 spec、plan、tasks 自动检查合规
- 是项目的"永久上下文"，不会在会话间丢失
- 权重：Constitution 中**越靠前的规则，权重越高**

### 2. Spec vs Plan 严格分离

这是 SDD 的核心创新：
- 一个 spec 可以有多个并行 plan（不同技术方案）
- 技术决策被明确记录并关联到功能需求
- 支持 Creative Exploration——对同一个 spec 探索多种实现方案

### 3. Extensions + Presets 双层定制

| 机制 | 目的 | 示例 |
|------|------|------|
| Extension | 添加新命令/新能力 | CI Guard、Architecture Guard |
| Preset | 修改现有模板/行为 | AIDE、Canon、Product Forge |

安装：`specify extension add <name>` / `specify preset add <name>`

### 4. 30+ 代理集成

Spec Kit 是四个框架中代理集成最广泛的。一个命令切换代理：
```bash
specify init . --ai copilot   # 切换到 Copilot
specify init . --ai gemini    # 切换到 Gemini
specify init . --ai claude    # 切换到 Claude
```

代理切换不会丢失已有制品，因为制品是磁盘上的文件。

### 5. 完整的社区生态

- 200+ 贡献者
- 91 个社区扩展（50+ 作者）
- 18 个社区预设
- 多种 SDD 变体（AIDE、Canon、Product Forge、MAQA）

### 6. 一次性 Create / 按功能迭代

Constitution 创建一次，然后每个功能独立走 5 步循环。这种设计使得：
- 宪法稳定不变
- 功能可以并行开发
- 不存在跨功能的耦合

### 7. 品质检查清单

`/speckit.checklist` 命令生成自定义质量清单，作为"单元测试 for Engligh"。

## 优缺点分析

### 优点

1. **最广泛的代理集成**（30+）— 无平台锁定
2. **文档即架构** — spec.md 和 plan.md 是长期可复用的项目资产
3. **GitHub 背书** — 企业信赖度高
4. **Extensions/Presets** — 扩展性最强的框架
5. **Constitution 治理** — 全局规则一致性
6. **Spec/Plan 分离** — 支持一个 spec 多种技术方案
7. **社区最活跃** — 91 扩展、18 预设、200+ 贡献者
8. **完全免费** — MIT 许可证

### 缺点

1. **认知负担高** — Spec 极其详细，审查负担大
2. **Token 消耗高 20-40%** — 每轮都要重读 spec/plan/tasks
3. **Brownfield 困难** — 在已有大型代码库中集成困难
4. **刚性阶段** — 不适合探索性/研究性工作
5. **Open Issues 最多**（632）— 可能反映维护压力
6. **学习曲线** — 需要理解 SDD 思维方式的转变
7. **代理依赖** — Spec Kit 本身只是模板，需要 AI 代理执行
8. **"Vibe coding"的反面** — 快速原型时感觉繁琐

## 适用场景

| 场景 | 推荐度 |
|------|--------|
| 企业级规范化开发 | ⭐⭐⭐⭐⭐ |
| 需要合规/审计的项目 | ⭐⭐⭐⭐⭐ |
| Greenfield 新项目 | ⭐⭐⭐⭐⭐ |
| 多代理/多团队协作（30+ 代理） | ⭐⭐⭐⭐⭐ |
| 需要长期维护的项目 | ⭐⭐⭐⭐⭐ |
| 快速原型 / PoC | ⭐⭐ |
| Bug fix / 小功能 | ⭐ |
| Brownfield 大型项目 | ⭐⭐⭐ |
| 探索性/研究性工作 | ⭐ |

## 实践者建议

来自社区实践者的关键建议：

**开始方式：**
1. 先花 20 分钟写一个好的 Constitution——它会有 10 倍回报
2. 在"可丢弃"的功能上先尝试一次完整流程
3. Start vibe, finish spec-driven（原型用 vibe，生产用 SDD）

**常见陷阱：**
- 跳过 /speckit.clarify → plan 会出垃圾
- Constitution 靠后的规则 → 代理不遵守（权重问题）
- 任务太大（每个 10 文件）→ 要求"拆成不超过 3 文件的任务"
- 让代理无人值守实现 30 个任务 → 先 3-5 个，审查，再扩大

**成本管理：**
- SDD 比 Vibe Coding 多消耗 20-40% token
- 但减少的浪费性实现循环更多
- 可在便宜的模型上运行规划阶段

## 数据来源

- 官方仓库：https://github.com/github/spec-kit
- 官方文档网站：https://github.github.io/spec-kit/
- DeepWiki：https://deepwiki.com/github/spec-kit/3-spec-driven-development
- LPains 博客：*Deep Dive into SpecKit* — https://blog.lpains.net/posts/2025-12-07-deep-dive-into-speckit/
- Fundesk 指南：*Spec-Driven Development with AI: The 2026 Guide to GitHub Spec Kit* — https://www.fundesk.io/spec-driven-development-github-spec-kit-guide
- Microsoft Developer Blog：*Diving Into Spec-Driven Development With GitHub Spec Kit* — https://developer.microsoft.com/blog/spec-driven-development-spec-kit
- GitHub Blog：*Spec-driven development with AI* — https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
- Ry Walker Research：*Agentic Skills Frameworks Compared* — https://rywalker.com/research/agentic-skills-frameworks
