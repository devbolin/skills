# Agentic Skills Framework 生态全景

## 概述

AI 编码代理的技能框架生态正在快速形成。截至 2026 年 2 月，市场上已有 11+ 个主流框架，分为三个层次：

1. **Methodology Frameworks** — 强制执行完整开发工作流
2. **Official Skill Catalogs** — 平台官方的可复用技能库
3. **Orchestration Platforms** — 多代理协调平台

此外，**AGENTS.md** 和 **SKILL.md** 已逐渐成为跨平台标准，被 11+ 工具支持。

---

## 一、Methodology Frameworks（方法论框架）

### 1. Superpowers — obra/superpowers ⭐57K

**定位**：最"有主见"的方法论框架，为 solo/小团队设计的完整开发纪律。

| 维度 | 说明 |
|------|------|
| 核心理念 | 流程 > 直觉，1% 规则，技能即约束 |
| 工作流 | Brainstorm → Design → Plan → TDD → Review → Finish |
| 技能数 | 14+ 内置技能 |
| 安装 | 分钟级 — 将 skills 文件夹放入项目 |
| 适合 | Solo 开发者、2-3 人小团队、Greenfield 项目 |
| 缺点 | 对有经验团队可能过度约束；子代理审查每次加 ~3 分钟 |

**独特设计**：
- 使用 Cialdini 影响力原则防止代理跳过步骤
- 子代理驱动开发 + 两阶段审查（Spec Compliance → Code Quality）
- TDD 铁律：RED→GREEN→REFACTOR，测试前代码必须删除

---

### 2. BMAD Method — docs.bmad-method.org ⭐37K

**定位**：最全面的方法论框架，覆盖完整敏捷生命周期。

| 维度 | 说明 |
|------|------|
| 核心理念 | AI 驱动的敏捷开发，12+ 专业角色代理 |
| 工作流 | Analysis → Brief → Architecture → Readiness → Implementation |
| 角色 | PM、Architect、Developer、QA 等 12+ 专业角色 |
| 安装 | 小时级 — 12+ 角色配置需要调优 |
| 适合 | 5-20 人团队，已有敏捷流程 |
| 缺点 | 对 <5 人团队过于臃肿；配置复杂 |

**独特设计**：
- 角色专业化：每个代理有自己的"人格"和职责
- 完整的 Agile 生命周期覆盖（从 ideation 到 deployment）
- Fork 率 12.4%（生态中最高的二次开发率）

---

### 3. Spec Kit — github/spec-kit ⭐71K

**定位**：最结构化的规范驱动开发工具，GitHub 官方出品。

| 维度 | 说明 |
|------|------|
| 核心理念 | 规范即执行：specifications 直接生成实现 |
| 工作流 | /constitution → /specify → /plan → /tasks → /implement |
| 工具 | Specify CLI（Python），30+ 代理集成 |
| 安装 | 分钟级 — CLI 脚手架全部搞定 |
| 适合 | 规范化项目、需要严格审批门禁的企业团队 |
| 缺点 | 刚性阶段不适合探索性/研究性工作 |

**独特设计**：
- 每个阶段有人类审批门禁（approval gate）
- Extensions + Presets 机制，可深度定制
- 支持 30+ AI 编码代理集成

---

### 4. gstack — garrytan/gstack ⭐~65K

**定位**：YC CEO Garry Tan 的个人开发工具集，将 Claude Code 变为虚拟工程团队。

| 维度 | 说明 |
|------|------|
| 核心理念 | 一个代理 = 一个团队（CEO + Designer + Eng Manager + QA + Security...） |
| 技能数 | 23 个 slash command + 8 power tools |
| 工作流 | Think → Plan → Build → Review → Test → Ship → Reflect |
| 安装 | 30 秒，支持 10 种代理 |
| 适合 | 创始人、技术 CEO、需要全流程管理的个人开发者 |
| 特点 | 极其强调产品思维（/office-hours 会反驳你的需求） |

**独特设计**：
- /office-hours — 用 6 个 forcing questions 重新定义产品方向
- /plan-ceo-review — 从 CEO 视角挑战需求范围
- /cso — 首席安全官，自动 OWASP + STRIDE 审计
- 跨模型审计（/codex 让 Codex CLI 做二次复审）
- Continuous checkpoint mode：自动 WIP commit + 上下文恢复

---

## 二、Skill Catalogs（官方技能库）

### 5. Anthropic Skills — anthropics/skills ⭐73K

- 最大的技能目录，SKILL.md 格式的创立者
- 涵盖 Creative、Technical、Enterprise、Document 四类
- 通过 Plugin Marketplace 分发
- 注意：**没有方法论层**，技能不强制流程纪律

### 6. OpenAI Skills — openai/skills ⭐9K

- 兼容 SKILL.md 格式，三层 tier 系统
- Codex CLI 专属
- 目录较小，仍快速增长中

### 7. Google Gemini Skills — google-gemini/gemini-skills ⭐1.8K

- 目前只有 1 个技能（gemini-api-dev）
- 质量数据公开：Gemini API 编码准确率 Flash 87%、Pro 96%

---

## 三、Orchestration Platforms（编排平台）

### 8. wshobson/agents ⭐29K

- 72 个插件，112 个代理
- Conductor 插件协调 Agent Teams 并行工作
- 插件架构灵活但决策负担重

### 9. Claude-Flow — ruvnet/claude-flow ⭐14K

- Swarm 编排，支持 Raft/BFT/CRDT 共识协议
- 494 open issues（稳定性隐患）

### 10. Babysitter — a5c-ai/babysitter ⭐317

- Event-sourced 确定性工作流执行
- 2000+ 预定义流程，支持 Human-in-the-loop
- 仅支持 Claude Code

### 11. Microsoft Amplifier ⭐3K

- Self-improving agents：代理写 DISCOVERIES.md
- 研究项目，非生产就绪

---

## 四、Standards（标准层）

### AGENTS.md ⭐18K

- 项目级上下文：技术栈、规范、边界
- 始终加载，定义"在哪里工作"
- 被 Codex、Copilot、Cursor、Claude Code、Gemini CLI 等支持

### SKILL.md

- 任务级能力：定义"如何工作"
- 渐进式披露：先加载轻量元数据，需要时加载完整指令
- 被 Claude Code、Cursor、Copilot、Codex、Gemini CLI、Kiro、OpenCode 等 11+ 平台支持
- agentskills.io 已成为开放标准

**两者关系**：AGENTS.md 是"where"，SKILL.md 是"how"。

---

## 五、横向对比

### 定位矩阵

```
                    强制（Prescriptive）
                         │
          Superpowers ●  │  ● BMAD
                         │
                         │  ● Spec Kit
   Catalog ──────────────┼───────────── Methodology
                         │
  Anthropic Skills ●     │
  OpenAI Skills ●        │  ● wshobson/agents
  Gemini Skills ●        │  ● Claude-Flow
                         │
                    灵活（Flexible）
```

### 关键指标

| 框架 | ⭐ Stars | Fork 率 | Open Issues | 工作量 | 适合团队 |
|------|----------|---------|-------------|--------|----------|
| Anthropic Skills | 73K | 10.2% | 329 | 分钟 | 任何团队 |
| Spec Kit | 71K | 8.6% | 632 | 分钟 | 企业/规范化 |
| gstack | ~65K | - | - | 30秒 | 创始人/全栈 |
| Superpowers | 57K | 7.6% | 144 | 分钟 | Solo/小团队 |
| BMAD | 37K | 12.4% | 38 | 小时 | 5-20 人团队 |
| wshobson/agents | 29K | 11.0% | 2 | 小时 | 多代理需求 |

### 共同模式

1. **Workflow Gate Pattern** — 所有方法论框架都有人类审批门禁
2. **Subagent Isolation** — 每个任务使用全新上下文
3. **Progressive Disclosure** — 元数据先行，指令按需加载
4. **Evidence-Driven** — 完成需要可验证的证据

---

## 六、安全警示

Snyk ToxicSkills 研究发现：
- 36% 的技能包含 prompt injection
- 26% 至少有一个漏洞
- 从 SKILL.md 到 Shell 访问只需 3 行 markdown

**应对**：优先使用官方目录，审计第三方技能，永远不要盲目安装。

---

## 七、选型建议

| 场景 | 推荐 |
|------|------|
| Solo 开发者 Greenfield 项目 | Superpowers |
| 5-20 人敏捷团队 | BMAD |
| 企业级规范化开发 | Spec Kit |
| 创始人/独立开发者想快速交付 | gstack |
| 只需要技能库不需要方法论 | Anthropic Skills |
| 多代理协调 | wshobson/agents + Superpowers |

**当前（2026 年 2 月）最佳实践组合**：
```
Superpowers (方法论) + Anthropic Skills (技能库) + AGENTS.md (上下文)
```

---

## 八、数据来源

- Ry Walker Research: agentic-skills-frameworks (2026.02)
- GitHub API (star/issue/fork 数据)
- 各项目官方文档
- Snyk ToxicSkills 安全报告
