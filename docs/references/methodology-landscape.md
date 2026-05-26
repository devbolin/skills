# 项目管理与软件开发方法论全景

## 目录

1. [概述](#1-概述)
2. [传统方法论（计划驱动）](#2-传统方法论计划驱动)
3. [敏捷宣言与核心理念](#3-敏捷宣言与核心理念)
4. [敏捷框架](#4-敏捷框架)
5. [专门化方法论](#5-专门化方法论)
6. [过程框架](#6-过程框架)
7. [开发实践与纪律](#7-开发实践与纪律)
8. [AI 时代的新方法论](#8-ai-时代的新方法论)
9. [全景对比](#9-全景对比)
10. [选型指南](#10-选型指南)

---

## 1. 概述

软件开发方法论（Software Development Methodology）是指导团队**如何规划、执行、交付和维护软件项目**的结构化框架。它们包含：

- **过程模型**：阶段划分和活动顺序
- **管理实践**：角色、会议、制品
- **技术实践**：编码、测试、集成方式
- **哲学原则**：价值观和指导理念

这些方法论大致可分为四个时代：

```
┌─────────────────────────────────────────────────────────────┐
│  1970s-1990s  │  2000s-2010s  │  2010s-2020s  │  2025+       │
│─────────────────────────────────────────────────────────────│
│  Waterfall    │  Agile/Scrum  │  DevOps       │  AI-Agent    │
│  Spiral       │  XP           │  Shape Up     │  SDD         │
│  RUP          │  Kanban       │  Lean         │  Superpowers │
│  V-Model      │  FDD          │  DDD          │  gstack      │
│  MSF          │  Crystal      │  TDD/BDD      │  BMAD        │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. 传统方法论（计划驱动）

### 2.1 Waterfall（瀑布模型）

**提出**：1970 年，Winston W. Royce
**核心**：线性顺序执行，每个阶段完成后进入下一阶段

```
Requirements → Design → Implementation → Testing → Deployment → Maintenance
```

**特点**：
- 严格文档驱动，每个阶段有明确的交付物
- 阶段之间没有回溯（或回溯成本极高）
- 适合需求明确、变化少的项目

**适用场景**：国防/航天/医疗等合规性要求高的领域、需求稳定的项目

**缺点**：无法适应需求变化、用户直到最后才能看到产品

---

### 2.2 V-Model（V 模型）

**提出**：1980s，德国国防部
**核心**：每个开发阶段对应一个测试阶段，形成"V"字形

```
Requirements ──────────── Acceptance Testing
    System Design ─────── System Testing
        Module Design ─── Integration Testing
            Coding ────── Unit Testing
```

**特点**：
- 测试贯穿整个生命周期
- 每个阶段都有对应的验证活动
- 强调质量保证从需求阶段开始

**适用场景**：安全性/可靠性要求高的系统（医疗、汽车、航空）

---

### 2.3 Spiral（螺旋模型）

**提出**：1986 年，Barry Boehm
**核心**：迭代 + 瀑布 + **风险驱动**

```
确定目标 → 识别风险 → 开发验证 → 评审计划 → (循环)
```

**特点**：
- 每次循环都是一个完整的开发周期（从规划到评审）
- 循环次数由风险分析决定
- 原型用于降低不确定性

**适用场景**：大型、高风险的复杂项目

---

### 2.4 RUP / Unified Process（Rational 统一过程）

**提出**：1990s，Rational Software（后并入 IBM）
**核心**：**用例驱动、架构为中心、迭代增量**

**4 个阶段**：

| 阶段 | 目标 | 里程碑 |
|------|------|--------|
| Inception（初始） | 确定项目范围和商业论证 | 生命周期目标 |
| Elaboration（细化） | 建立架构基线 | 生命周期架构 |
| Construction（构建） | 完成所有功能 | 初始可运行能力 |
| Transition（移交） | 部署到用户环境 | 产品发布 |

**6 个核心工程规程**（在整个生命周期中迭代进行）：
- Business Modeling（业务建模）
- Requirements（需求）
- Analysis & Design（分析与设计）
- Implementation（实现）
- Test（测试）
- Deployment（部署）

**变体**：
- **OpenUP** — RUP 的开源精简版，Eclipse 基金会维护
- **AUP（Agile Unified Process）** — Scott Ambler 提出的敏捷版本
- **Essential Unified Process (EssUP)** — 更轻量的版本

**特点**：非常重量级，适合大型企业项目。定义了完整的角色、制品、工作流。

---

### 2.5 MSF（Microsoft Solutions Framework）

**提出**：1993 年，Microsoft
**核心**：基于风险管理、团队协作的灵活过程框架

**5 个阶段**：Envision（构想）→ Plan（规划）→ Develop（开发）→ Stabilize（稳定）→ Deploy（部署）

**特点**：强调"版本零风险"、团队健康模型

---

## 3. 敏捷宣言与核心理念

### 3.1 Agile Manifesto（敏捷宣言）

**提出**：2001 年，17 位软件方法论者（Kent Beck, Martin Fowler, Robert C. Martin 等）

**四大价值观**：
1. **个体和互动** > 流程和工具
2. **可工作的软件** > 详尽的文档
3. **客户合作** > 合同谈判
4. **响应变化** > 遵循计划

> 右项有价值，左项更有价值。

**十二条原则**：
1. 最高优先级——通过尽早和持续地交付有价值的软件来满足客户
2. 欢迎变化的需求，即使是在开发后期
3. 频繁交付可工作的软件（几周到几个月）
4. 业务人员和开发人员每天一起工作
5. 围绕被激励的个体构建项目
6. 面对面交谈最有效率
7. 可工作的软件是进度的主要度量
8. 可持续的开发速度
9. 持续关注技术卓越和好的设计
10. 简单——最大化未完成工作量的艺术
11. 自组织团队产生最好的架构、需求和设计
12. 定期反思如何更有效，并调整

### 3.2 Agile vs 传统（对比）

| 维度 | 传统（Plan-Driven） | Agile |
|------|-------------------|-------|
| 需求确定性 | 前期完整确定 | 持续演进 |
| 变更成本 | 后期越高 | 任何时候可控 |
| 客户参与 | 主要在开始和结束 | 持续参与 |
| 团队结构 | 按角色分工 | 跨职能自组织 |
| 交付频率 | 一次交付 | 频繁小版本 |
| 文档量 | 详细 | 适度 |
| **风险对冲** | 预防式（详细计划） | 适应式（快速反馈） |

---

## 4. 敏捷框架

### 4.1 Scrum

**提出**：1995 年，Jeff Sutherland 和 Ken Schwaber

**核心**：**固定时间盒（Sprint）+ 三个角色 + 五个事件 + 三个制品**

**三个角色**：
- **Product Owner** — 定义需求，管理 Product Backlog，最大化产品价值
- **Scrum Master** — 确保 Scrum 流程执行，移除障碍
- **Development Team** — 自组织地完成 Sprint 工作

**五个事件**：
1. **Sprint**（1-4 周的时间盒）
2. **Sprint Planning** — 规划 Sprint 目标和工作范围
3. **Daily Scrum** — 15 分钟的日常同步
4. **Sprint Review** — 展示完成的工作
5. **Sprint Retrospective** — 回顾改进

**三个制品**：
- **Product Backlog** — 所有需求的优先级列表
- **Sprint Backlog** — 当前 Sprint 要完成的任务
- **Increment** — 可交付的增量产品

**适用场景**：大部分软件开发团队。最大的挑战是：严格的时间盒可能导致未完成的工作被挤压。

---

### 4.2 Kanban（看板）

**起源**：1940s，丰田生产系统
**引入软件领域**：David J. Anderson（2010 年出版 *Kanban*）

**核心**：**可视化工作流 + 限制在制品（WIP Limit）+ 拉动系统**

**六个实践**：
1. 可视化工作流（看板板：To Do / Doing / Done）
2. 限制在制品（WIP Limit）
3. 管理流动
4. 明确过程策略
5. 建立反馈循环
6. 协同改进

**与 Scrum 的区别**：

| 维度 | Scrum | Kanban |
|------|-------|--------|
| 迭代 | 固定 Sprint | 连续流动 |
| 时间盒 | 必须按时结束 Sprint | 没有时间限制 |
| 角色 | Product Owner / Scrum Master | 没有预设角色 |
| 变更 | Sprint 内不接新需求 | 随时可以添加 |
| 度量 | Velocity | Cycle Time |
| 适合作 | 需要节奏感、批量交付 | 持续交付、运维类工作 |

---

### 4.3 XP（Extreme Programming，极限编程）

**提出**：1996 年，Kent Beck（《Extreme Programming Explained》1999）

**核心**：**将好的实践推到极致**

**5 个价值观**：Communication（沟通）、Simplicity（简单）、Feedback（反馈）、Courage（勇气）、Respect（尊重）

**13 个核心实践**：

| 实践 | 说明 |
|------|------|
| **Pair Programming** | 两人一机，实时代码审查 |
| **TDD** | 测试先行 |
| **Continuous Integration** | 每天多次集成 |
| **Simple Design** | 最简设计 |
| **Refactoring** | 持续重构 |
| **Collective Ownership** | 任何开发者可以修改任何代码 |
| **Coding Standards** | 统一编码规范 |
| **Sustainable Pace** | 40 小时工作周 |
| **Whole Team** | 客户是团队一部分 |
| **Planning Game** | 每迭代开始时由用户确定优先级 |
| **Small Releases** | 频繁发布 |
| **Metaphor** | 用共享隐喻描述系统 |
| **Test First** | 先写测试 |

**适用场景**：需求变化频繁的小型团队

**影响**：XP 的技术实践（TDD、CI、重构、Pair Programming）已成为现代开发的标准实践，即使团队不声称使用 XP。

---

### 4.4 FDD（Feature Driven Development，特征驱动开发）

**提出**：1997 年，Jeff De Luca 和 Peter Coad
**核心**：**特征（Feature）为中心，5 步流程**

**5 个过程**：
1. **Develop Overall Model** — 建立全局领域模型
2. **Build Feature List** — 列出所有特征
3. **Plan by Feature** — 按特征排序和分配
4. **Design by Feature** — 每个特征详细设计
5. **Build by Feature** — 逐个实现特征

**特点**：
- 强调领域模型驱动
- 每个特征通常 2-10 天完成
- 角色清晰（Project Manager、Chief Architect、Class Owner）

---

### 4.5 Crystal Clear（水晶方法论）

**提出**：1991 年，Alistair Cockburn
**核心**：**人本主义，按项目规模"调色"**

**系列**：Crystal Clear（<8人）→ Crystal Yellow（<20人）→ Crystal Orange（<50人）→ Crystal Red（<100人）

**7 个属性**：Frequent Delivery、Reflective Improvement、OSMOSIS（团队沟通）、Personal Safety、Focus、Easy Access to Expert Users、Technical Environment

**特点**：最灵活——不做超出项目规模的强制要求

---

### 4.6 Lean Development（精益开发）

**起源**：1948 年，Toyota Production System
**引入软件**：Mary Poppendieck（2003 年 *Lean Software Development*）

**7 个原则**：
1. **Eliminate Waste** — 消除浪费（半成品、多余功能、等待、沟通延迟等）
2. **Amplify Learning** — 增强学习（迭代、反馈）
3. **Decide as Late as Possible** — 延迟决策（保留选项）
4. **Deliver as Fast as Possible** — 快速交付
5. **Empower the Team** — 授权团队
6. **Build Integrity In** — 内建质量
7. **See the Whole** — 全局优化

**7 种浪费（MUDA）**：部分完成的工作、多余功能、重复学习、任务切换、等待、移交、缺陷

**与 Lean Manufacturing 的映射**：
- Just-In-Time → 持续交付
- Kanban → 看板系统
- Kaizen → 持续改进
- Andon Cord → 构建中断（发现质量问题立即停机）

---

### 4.7 Shape Up（塑造方法）

**提出**：2018 年，Ryan Singer（Basecamp），《Shape Up: Stop Running in Circles》

**核心**：**6 周周期 + 无积压 + 固定时间变量范围**

**三个阶段**：

| 阶段 | 内容 | 时间 | 产出 |
|------|------|------|------|
| **Shaping**（塑造） | 定义问题范围、勾画解决方案边界 | 1-2 周 | Pitch（提案） |
| **Betting**（下注） | 团队选择哪些提案进入开发周期 | 1 天 | Cycle Plan |
| **Building**（建造） | 6 周开发，无干扰 | 6 周 | 可交付的功能 |

**对 Scrum 的批判**：
- Scrum 的积压会无限增长
- Scrum 的 Sprint 没有明确的上界（未完成就顺延）
- 需求不是自下而上的，需要"塑造"阶段

**关键概念**：
- **Appetite（胃口）** — 不是"需要多久"，而是"值得给多少时间"
- **Fixed Time, Variable Scope** — 固定时间，灵活范围（而非固定范围灵活时间）
- **No Backlog** — 没有积压，每次 Betting 重新判断
- **Cool-Down（冷却期）** — 6 周开发后的 2 周缓冲

**适用场景**：产品开发团队（尤其是自研产品），不适合合同/外包开发

---

## 5. 专门化方法论

### 5.1 RAD（Rapid Application Development，快速应用开发）

**提出**：1991 年，James Martin
**核心**：**原型 + 迭代 + 时间盒**

**4 个阶段**：Requirements Planning → User Design → Construction → Cutover

**特点**：强调用户参与、CASE 工具支持、组件复用

---

### 5.2 JAD（Joint Application Development，联合应用开发）

**提出**：1970s，IBM（Chuck Morris 和 Tony Crawford）
**核心**：**集中式工作坊 + 利益相关者 + 决策**

**特点**：通过密集的协作工作坊替代传统需求收集，减少文档和会议

---

### 5.3 Prototype Model（原型模型）

**核心**：**快速构建可工作的原型 → 用户反馈 → 迭代完善**

**类型**：
- Throwaway Prototype（抛弃型）
- Evolutionary Prototype（演进型）

**适合**：用户需求不明确、界面为主的应用

---

### 5.4 DevOps

**提出**：2009 年，Patrick Debois 和 Andrew Clay Shafer（DevOpsDays）
**核心**：**开发 + 运维 一体化，文化、自动化、度量、分享（CAMS）**

**关键实践**：
- **CI（Continuous Integration）** — 频繁将代码合并到主干
- **CD（Continuous Delivery / Deployment）** — 自动化部署
- **Infrastructure as Code（IaC）** — 基础设施代码化
- **Monitoring & Observability** — 监控与可观察性
- **Microservices** — 微服务架构
- **Blameless Postmortems** — 免责事后分析

**CALMS 框架**：
- **Culture** — 协作文化
- **Automation** — 自动化一切
- **Lean** — 精益思维
- **Measurement** — 数据驱动
- **Sharing** — 跨团队分享

---

### 5.5 DAD（Disciplined Agile Delivery）

**提出**：2012 年，Scott Ambler
**核心**：**目标驱动的混合敏捷方法**

**三个阶段**：Inception → Construction → Transition

**特点**：提供了比 Scrum 更多的选择——团队可以根据上下文选择适合的实践。

---

### 5.6 Large-Scale Scrum（LeSS）

**核心**：**在大型团队中扩展 Scrum**

**LeSS**（2-8 个团队，约 50 人）：一个 Product Owner + 一个 Product Backlog + 所有团队共享一个 Sprint
**LeSS Huge**（8+ 团队，数百人）：按需求区域划分

**原则**：Scrum 的核心原则不变，只增加协调机制。

---

### 5.7 SAFe（Scaled Agile Framework）

**提出**：2011 年，Dean Leffingwell
**核心**：**企业级敏捷扩展**

**4 层配置**：Team → Program → Large Solution → Portfolio

**特点**：最完整的企业级框架，涵盖从团队到投资组合的完整治理

**批评**：过于重，增加了不必要的层级和仪式

---

## 6. 过程框架

### 6.1 CMMI（Capability Maturity Model Integration）

**核心**：**过程成熟度评估模型**

**5 个成熟度等级**：
1. **Initial（初始级）** — 过程无序，依赖个人英雄主义
2. **Managed（管理级）** — 基本项目管理
3. **Defined（定义级）** — 过程标准化
4. **Quantitatively Managed（量化管理级）** — 数据驱动管理
5. **Optimizing（优化级）** — 持续改进

**适用场景**：政府/国防合同、合规性要求高的组织

---

### 6.2 ISO/IEC 12207

国际标准化的软件生命周期过程框架，定义了 5 个核心过程、9 个支持过程、4 个组织过程

### 6.3 PMBOK（Project Management Body of Knowledge）

**发布者**：PMI（Project Management Institute）
**核心**：**5 个过程组 + 10 大知识领域 + 49 个过程**

**5 个过程组**：Initiating → Planning → Executing → Monitoring & Controlling → Closing

---

## 7. 开发实践与纪律

### 7.1 TDD（Test-Driven Development，测试驱动开发）

**RED → GREEN → REFACTOR**

- 先写失败的测试
- 写最简代码使其通过
- 重构

**核心价值**：可验证的设计决策、安全的回归测试覆盖

### 7.2 BDD（Behavior-Driven Development，行为驱动开发）

**提出**：2003 年，Dan North
**格式**：**Given-When-Then**
- **Given** <初始上下文>
- **When** <事件触发>
- **Then** <预期结果>

**工具**：Cucumber、SpecFlow、Behave

### 7.3 DDD（Domain-Driven Design，领域驱动设计）

**提出**：2003 年，Eric Evans，《Domain-Driven Design》
**核心**：**以领域模型为核心**

**关键概念**：
- **Ubiquitous Language（统一语言）**
- **Bounded Context（限界上下文）**
- **Aggregate（聚合）**
- **Entity（实体）** vs **Value Object（值对象）**
- **Repository（仓库）**
- **Domain Event（领域事件）**

### 7.4 IDD / SDD（Intent/Spec-Driven Development，意图/规范驱动开发）

属于 AI 时代的实践（详见第 8 节）。核心是：**先写意图/规范 → AI 生成代码**。

### 7.5 CI/CD（Continuous Integration / Continuous Deployment）

- **CI**：每日多次合并代码到主干，自动构建和测试
- **CD**：自动化部署到生产环境

### 7.6 Pair Programming（结对编程）

**提出**：XP 实践之一
**模式**：Driver（写代码）+ Navigator（审查 + 思考策略）
**节奏**：每 15-30 分钟轮换

### 7.7 Mob Programming（群体编程）

**提出**：Woody Zuill
**核心**：整个团队在同一台机器上、同一时间、处理同一件事

### 7.8 Trunk-Based Development（主干开发）

短生命周期分支（< 1 天）→ 频繁合并到主干 → 功能开关控制发布

---

## 8. AI 时代的新方法论

随着 AI 编码代理（Claude Code、Codex、Cursor 等）的普及，2025-2026 年涌现了一批**针对 AI 代理的软件开发方法论**。这些新方法论的共同特征是：**在"人写代码"和"AI 写代码"之间，增加了流程层来确保 AI 的行为可预测、可控制、可审计。**

### 8.1 总览

| 框架 | 类型 | 核心主张 | Stars | 创建者 |
|------|------|---------|-------|--------|
| **Superpowers** | Workflow Methodology | 流程约束 + 1% 规则 + TDD 铁律 | ~190K | obra (Jesse Vincent) |
| **BMAD Method** | Team Simulation | 21 角色 × 4 阶段，模拟完整 Agile 团队 | ~37K | BMad Code |
| **Spec Kit** | Spec-Driven Dev | 先写规范再写代码，Spec/Plan 严格分离 | ~97K | GitHub |
| **gstack** | Software Factory | 28 角色命令，从 /office-hours 到 /ship | ~65K | Garry Tan (YC) |

### 8.2 对传统方法论的映射

AI 时代的方法论不是凭空出现的，它们是对传统方法论的**适配和自动化**：

| AI 方法论 | 对应传统方法论 | AI 带来的变化 |
|-----------|--------------|--------------|
| Superpowers | XP + TDD | 流程由代理自动执行，不再依赖人的纪律 |
| BMAD | Scrum + RUP | 21 个角色全部是 AI 代理，不再需要多人团队 |
| Spec Kit | Waterfall（规格先行的版本） | 规格由代理解释和执行，不再需要手写需求到代码的映射 |
| gstack | Full Agile Lifecycle | 每个阶段对应一个 AI 角色命令，端到端自动化 |

### 8.3 关键区别

与传统方法论相比，AI 时代方法论有 4 个本质区别：

**1. 角色自动化**
- 传统：Scrum 需要 PO + SM + Team（真人）
- AI：BMAD 的 21 个角色全部是 AI 代理

**2. 流程强制化**
- 传统：人的纪律性决定流程遵循度
- AI：Superpowers 通过 Cialdini 说服原则从系统层面强制流程

**3. 文档可执行化**
- 传统：规格是文字文档，需要人来实现
- AI：Spec Kit 的 spec.md 由 AI 代理直接解释和执行

**4. 反馈实时化**
- 传统：Sprint Review 每 2 周一次反馈
- AI：gstack 的审查和验证即时发生

### 8.4 AI 时代的象限图

```
                      AI 生成代码
                         ↑
                         │
        gstack ●         │         ● BMAD
        工具集            │         团队模拟
                         │
   灵活 ─────────────────┼──────────────── 强制
                         │
  Spec Kit ●             │         ● Superpowers
  规范驱动               │         流程约束
                         │
                      AI 遵循流程
```

---

## 9. 全景对比

### 9.1 按核心特征分组

```
                     Plan-Driven               Adaptive
                    ┌──────────┐           ┌──────────┐
      Micro         │  TDD     │           │  XP      │
      (单个实践)     │  BDD     │           │  Pair    │
                    │  DDD     │           │  CI/CD   │
                    └──────────┘           └──────────┘

                    ┌──────────┐           ┌──────────┐
      Meso          │ Waterfall│           │  Scrum   │
      (框架)        │ V-Model  │           │  Kanban  │
                    │ RUP      │           │  Shape Up│
                    │ Spiral   │           │  FDD     │
                    └──────────┘           └──────────┘

                    ┌──────────┐           ┌──────────┐
      Macro         │ CMMI     │           │  SAFe    │
      (组织级)      │ PMBOK    │           │  LeSS    │
                    │ ISO12207 │           │  DAD     │
                    └──────────┘           └──────────┘

                    ┌──────────┐           ┌──────────┐
      AI Era        │ Spec Kit │           │  gstack  │
      (代理驱动)    │ Superpowers          │  BMAD    │
                    └──────────┘           └──────────┘
```

### 9.2 按时间线排列

```
1970 ─ Waterfall (Royce)
1986 ─ Spiral (Boehm)
1988 ─ RUP (Rational/IBM)
1991 ─ Crystal (Cockburn)
       RAD (Martin)
1993 ─ MSF (Microsoft)
1995 ─ Scrum (Sutherland/Schwaber)
1996 ─ XP (Beck)
1997 ─ FDD (De Luca/Coad)
1999 ─ Lean Dev (Poppendieck)
2001 ─ Agile Manifesto
2003 ─ TDD (Beck)
       BDD (North)
       DDD (Evans)
2009 ─ DevOps (Debois)
2010 ─ Kanban (Anderson)
2011 ─ SAFe (Leffingwell)
2012 ─ DAD (Ambler)
2018 ─ Shape Up (Singer/Basecamp)
     ─────────────────────────────
2025 ─ Superpowers (obra)
       Spec Kit (GitHub)
2026 ─ BMAD Method
       gstack (Garry Tan)
```

### 9.3 关键维度对比

#### 按项目管理风格

| 方法论 | 风格 | 计划方式 | 变更适应性 | 文档要求 |
|--------|------|---------|-----------|---------|
| Waterfall | 指令性 | 前期完整 | 极低 | 详细 |
| RUP | 框架性 | 迭代细化 | 中等 | 详细 |
| Scrum | 敏捷 | Sprint 规划 | 高 | 适度 |
| Kanban | 流动 | 持续 | 极高 | 轻量 |
| XP | 敏捷+技术 | 发布规划 | 高 | 轻量+测试 |
| Shape Up | 固定时间 | 塑造+下注 | 周期内低 | 提案+范围 |
| Spec Kit | 规范驱动 | 规格先行 | 规格迭代 | 高（spec 即代码） |
| Superpowers | 流程强制 | 计划先行 | 中等 | plan.md + design.md |
| BMAD | 角色模拟 | 四阶段 | 中等（制品驱动） | 高（PRD+ADR+Stories） |
| gstack | 命令驱动 | 无预设 | 高 | 嵌入命令输出 |

#### 按规模适用性

| 方法论 | Solo | 2-5 人 | 5-20 人 | 20-100 人 | 100+ 人 |
|--------|------|--------|---------|-----------|---------|
| Waterfall | ✅ | ✅ | ✅ | ✅ | ✅ |
| Scrum | ❌ | ✅ | ✅ | ✅ | ⚠️（需扩展） |
| Kanban | ✅ | ✅ | ✅ | ✅ | ✅ |
| XP | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| Shape Up | ⚠️ | ✅ | ✅ | ⚠️ | ❌ |
| RUP | ❌ | ❌ | ⚠️ | ✅ | ✅ |
| SAFe | ❌ | ❌ | ❌ | ✅ | ✅ |
| Spec Kit | ✅ | ✅ | ✅ | ⚠️ | ⚠️ |
| Superpowers | ⭐ | ⭐ | ✅ | ⚠️ | ❌ |
| BMAD | ⚠️ | ⚠️ | ⭐ | ✅ | ⚠️ |
| gstack | ⭐ | ⭐ | ✅ | ⚠️ | ❌ |

---

## 10. 选型指南

### 10.1 按场景选择

| 你的情况 | 推荐方法 |
|----------|---------|
| 刚成立的创业公司，需要快速交付 | **Shape Up** 或 **Scrum** |
| 大型企业，多个团队协作 | **SAFe** 或 **LeSS** |
| 需要严格合规（医疗/航空） | **Waterfall** + **V-Model** + **CMMI** |
| 运维为主（Bug 修复、小需求） | **Kanban** |
| 需求极不明确，需要探索 | **Spiral** + **Prototype** |
| Solo 开发者用 AI 编程 | **gstack**（30 秒安装） |
| 需要强工程纪律 | **Superpowers**（TDD 铁律） |
| 完整产品生命周期管理 | **BMAD**（虚构团队） |
| 需要严格的文档/审计追溯 | **Spec Kit**（spec 即真相源） |

### 10.2 按团队成熟度

```
新手团队 ───── Waterfall（容易理解）
     │
     ▼
Scrum（引入节奏感）
     │
     ▼
Kanban + 技术实践（XP 的 TDD/CI）
     │
     ▼
Shape Up（更成熟的产品管理）
     │
     ▼
AI-assisted（Superpowers / gstack）
     │
     ▼
AI-native（BMAD / Spec Kit）
```

### 10.3 混合实践推荐

在实践中，大多数团队不使用单一方法论，而是**有机组合**最佳实践：

| 层面 | 推荐实践 | 来源 |
|------|---------|------|
| **项目管理** | Scrum 的仪式（Planning / Daily / Review / Retro） | Scrum |
| **需求管理** | Shape Up 的塑造 + 下注机制 | Shape Up |
| **开发技术** | TDD + CI/CD + 持续重构 | XP |
| **领域建模** | DDD 的 Bounded Context + Ubiquitous Language | DDD |
| **团队结构** | 跨功能自组织团队 | Agile |
| **运维** | DevOps 的自动化 + 监控 + 可观察性 | DevOps |
| **AI 代理** | Superpowers 的 TDD 强制 + gstack 的 QA/部署 | AI |

---

## 11. 总结

软件方法论的发展趋势：

```
Waterfall ─→ RUP ─→ Scrum/XP ─→ DevOps ─→ AI-Assisted
 1970       1990     2000        2009       2025

 文档驱动    过程驱动   迭代驱动     自动化驱动    代理驱动
 线性        迭代      自组织        持续交付      流程强制
```

**每一次转变都不是替代，而是叠加。** Agile 没有杀死 Waterfall——在高合规领域它仍然是标准。TDD 没有替代 Scrum——它在 Scrum 内部作为技术实践运行。

同样，AI 时代的方法论**不会取代**传统方法论，而是提供了新的工具层：

- 如果你已经有成熟的 Scrum 团队 → 叠加 **Superpowers** 强化 TDD 纪律
- 如果你觉得 Scrum 太重 → 尝试 **gstack** 让 AI 承担部分角色
- 如果你的产品需要完整文档追溯 → 用 **BMAD** 或 **Spec Kit** 替代传统文档流程

> **方法论是手段，不是目的。最好的方法论是你团队会真正使用的那个。**

---

*参考来源：各方法论官方文档、Wikipedia、Agile Manifesto、相关书籍及社区讨论*
