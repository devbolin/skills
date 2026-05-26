# 调研产出总览

> 本文档是本轮深度调研的全部原创文档索引。按产出顺序排列，另附知识图谱。

---

## 核心导航

**开始阅读建议**：
1. 如果你想了解 Superpowers 是什么 → `superpowers-design-philosophy.md`
2. 如果你想深入 Superpowers 架构 → `deep-dive-superpowers.md`
3. 如果你想对比四大框架 → `methodology-frameworks-deep-dive.md`
4. 如果你想看整个方法论谱系 → `methodology-landscape.md`
5. 如果你想看常用工具 → `pm-dev-tools-catalog.md`
6. 如果你想看思维模型 → `first-principles-mental-models.md`
7. 如果你想看跨域应用 → `ai-domain-cross-models.md`
8. 如果你想看技能设计原理 → `skill-analysis-tools-principles.md`

---

## 文档索引

### 第一轮：框架概览与生态全景

| # | 文件 | 行数 | 说明 |
|---|------|------|------|
| 1 | `superpowers-design-philosophy.md` | 156 | Superpowers 10 条核心理念提炼 |
| 2 | `agentic-skills-framework-landscape.md` | 239 | 11+ 框架三层分类（方法论/目录/编排）+ 定位矩阵 |

### 第二轮：生态调查与竞品横向

| # | 文件 | 行数 | 说明 |
|---|------|------|------|
| 3 | `methodology-frameworks-deep-dive.md` | 709 | 四框架横向对比（哲学/工作流/能力矩阵/场景选型/组合建议） |

### 第三轮：四框架独立深度调查

| # | 文件 | 行数 | 重点 |
|---|------|------|------|
| 4 | `deep-dive-superpowers.md` | 448 | Cialdini 说服心理学、压力场景测试、TDD for Skills、8 阶段工作流、14 技能 |
| 5 | `deep-dive-bmad.md` | 440 | 21 角色详解、双路径决策（Quick vs Full）、Party Mode、Sprint Status |
| 6 | `deep-dive-speckit.md` | 448 | Power Inversion 哲学、Spec/Plan 严格分离、Constitution 治理、91 Extensions |
| 7 | `deep-dive-gstack.md` | 540 | 28+ 技能、内置浏览器 QA、跨模型审计、Conductor、600K 行/60 天 |

### 第四轮：方法论全景

| # | 文件 | 行数 | 覆盖 |
|---|------|------|------|
| 8 | `methodology-landscape.md` | 779 | 传统（Waterfall/V-Model/Spiral/RUP）→ 敏捷（Scrum/Kanban/XP/FDD/Crystal/Lean/Shape Up）→ DevOps → AI 时代 + 选型指南 + 时间线 |

### 第五轮：常用工具图谱

| # | 文件 | 行数 | 覆盖 |
|---|------|------|------|
| 9 | `pm-dev-tools-catalog.md` | 805 | 12 大类：根因分析 → 质量管理 7+7 → Gantt/PERT/WBS → UML/C4/ADR → 估算 → Retro 8 种 → RACI → VSM → Story Mapping → 风险 → 决策 → AI 新工具 |

### 第六轮：技能分析工具与原理

| # | 文件 | 行数 | 覆盖 |
|---|------|------|------|
| 10 | `skill-analysis-tools-principles.md` | 629 | 12 类：渐进式披露 → 说服原理 → 技能隔离 → TDD for Skills → 压力场景 → 两阶段审查 → 自动匹配 → 上下文工程 → 安全分析 → 子代理模式 → 技能间通信 → 度量分析 |

### 第七轮：思维模型 — 第一性原理

| # | 文件 | 行数 | 覆盖 |
|---|------|------|------|
| 11 | `first-principles-mental-models.md` | 752 | 15 个模型：第一性原理、二阶思维、逆向思维、奥卡姆剃刀、能力圈、思维栅格、80/20、汉隆剃刀、KISS/YAGNI、过早优化、康威定律、古德哈特定律、帕金森定律、布鲁克斯定律、邓巴数 |

### 第八轮：跨域模型在 AI 领域的应用

| # | 文件 | 行数 | 覆盖 |
|---|------|------|------|
| 12 | `ai-domain-cross-models.md` | 749 | 12 个跨域模型：认知偏差 10 种、助推理论、Cynefin 框架、OODA 循环、约束理论、德雷福斯技能模型、双环学习、事前验尸、MECE 原则、信号 vs 噪声、Safety-I/II、心智模型补充 14 个 |

---

## 知识图谱

### 按层级

```
第 1 层: Superpowers 核心
  ├── superpowers-design-philosophy.md     ← 10 条核心理念
  └── deep-dive-superpowers.md             ← 完整架构

第 2 层: 竞品对比
  ├── deep-dive-bmad.md
  ├── deep-dive-speckit.md
  ├── deep-dive-gstack.md
  └── methodology-frameworks-deep-dive.md  ← 横向对比

第 3 层: 全局方法论文档
  ├── methodology-landscape.md             ← 完整方法论谱系
  ├── pm-dev-tools-catalog.md              ← 常用工具图谱
  ├── skill-analysis-tools-principles.md   ← 技能设计原理
  └── agentic-skills-framework-landscape.md ← 生态全景

第 4 层: 底层思维模型
  ├── first-principles-mental-models.md    ← 15 个核心思维模型
  └── ai-domain-cross-models.md            ← 跨域模型在 AI 中的应用
```

### 按阅读顺序

```
新手入门路径:
  superpowers-design-philosophy.md
  → deep-dive-superpowers.md
  → methodology-frameworks-deep-dive.md
  → methodology-landscape.md

工具查表路径:
  pm-dev-tools-catalog.md（附录速查表）

深度理论路径:
  skill-analysis-tools-principles.md
  → first-principles-mental-models.md
  → ai-domain-cross-models.md
```

### 按问题

| 你想了解 | 读哪个 |
|---------|--------|
| Superpowers 是什么？ | `superpowers-design-philosophy.md` |
| Superpowers 怎么工作的？ | `deep-dive-superpowers.md` |
| BMAD 和其他框架的区别？ | `deep-dive-bmad.md` + `methodology-frameworks-deep-dive.md` |
| 哪个框架最适合我？ | `methodology-frameworks-deep-dive.md` 场景选型表 |
| 有什么常用分析工具？ | `pm-dev-tools-catalog.md` 速查表 |
| 思维模型有哪些？ | `first-principles-mental-models.md` |
| AI 技能设计的原则？ | `skill-analysis-tools-principles.md` |
| 认知偏差怎么影响 Agent？ | `ai-domain-cross-models.md`第 1 节 |
| Cynefin 框架如何用于技能分类？ | `ai-domain-cross-models.md`第 3 节 |
| OODA 和 Agent 工作循环的关系？ | `ai-domain-cross-models.md`第 4 节 |
| 软件方法论全景？ | `methodology-landscape.md` |
| 生态中有哪些项目？ | `agentic-skills-framework-landscape.md` |

---

## 统计

| 指标 | 数值 |
|------|------|
| 总文档数（本轮新增） | 12 |
| 总行数 | ~7,700 |
| 覆盖方法论文数 | ~20 |
| 覆盖常用工具 | ~60 |
| 覆盖思维模型 | 27 |
| 覆盖跨域模型 | 12 |
| 覆盖 AI 框架 | 4（Superpowers/BMAD/Spec Kit/gstack） |
| 覆盖其他项目 | 7（Anthropic Skills/OpenAI Skills/Gemini Skills/wshobson/Claude-Flow/Babysitter/Amplifier） |
