# 架构概述

## 一、核心概念

### 1.1 Skill 是什么？

Skill 是 AI Agent 的可复用能力单元，包含：

| 组成部分 | 文件 | 说明 |
|---------|------|------|
| 定义 | `skill.yaml` | 元数据、输入输出、权限 |
| 实现 | `scripts/` | 可执行代码 |
| 文档 | `SKILL.md` | 人类可读说明 |
| 适配器 | `adapters/` | 各工具专用格式 |

### 1.2 Adapter 是什么？

Adapter 是 Skill 对特定工具的适配层，让同一 Skill 可被不同 Agent 调用：

| Adapter 类型 | 用途 | 示例 |
|-------------|------|------|
| `prompt` | 指令型 | Copilot `.github/skills/` |
| `tool` | 工具型 | OpenAI Tool, LangChain |
| `workflow` | 流程型 | LangGraph, Dify |
| `mcp` | 协议型 | MCP 客户端（阶段二） |

### 1.3 Agent 是什么？

Agent 是带角色和技能组合的执行单元，是 Skill 的消费者：

| 组成部分 | 文件 | 说明 |
|---------|------|------|
| 定义 | `agent.yaml` | 角色、边界、关联的 Skills |
| 文档 | `AGENT.md` | 给模型看的 Agent 说明 |

示例 `agent.yaml`：
```yaml
id: finance-analyst
name: Finance Analyst
version: 1.0.0
description: 分析财务任务，选择并使用相关 Skills

skills:
  - invoice-extractor
  - expense-auditor
```

### 1.4 平台原则

- **Repo** 是维护与发布边界
- **Skill** 是解析与调用边界
- **Agent** 是 Skill 的消费者
- **Adapter** 是工具接入的统一契约

---

## 二、仓库结构

### 2.1 Domain Polyrepo

每个领域一个独立仓库：

```
skills-finance/                         # 财务领域
├── repo.yaml                          # 仓库元数据
├── .github/
│   └── workflows/
│       ├── ci.yml                    # PR 校验
│       └── release.yml               # 发布流程
├── skills/
│   ├── invoice-extractor/            # 技能：发票提取
│   │   ├── skill.yaml               # 技能定义
│   │   ├── SKILL.md                 # Agent 文档
│   │   ├── scripts/                 # 实现代码
│   │   │   └── run.py
│   │   ├── adapters/                # 适配器
│   │   │   ├── tool/tool.json       # OpenAI/LangChain
│   │   │   ├── prompt/SKILL.md      # Copilot
│   │   │   └── workflow/graph.yaml  # Dify/n8n
│   │   └── tests/
│   └── expense-auditor/             # 技能：费用审计
├── agents/                           # Agent 定义（阶段二）
│   └── finance-analyst/
│       ├── agent.yaml               # Agent 定义
│       └── AGENT.md                 # Agent 文档
└── shared/                          # 共享资源
```

### 2.2 Skill Catalog 仓库

```
skill-catalog/                          # 统一索引
├── index.json                        # 全局索引
├── skills/
│   ├── invoice-extractor.json        # 技能明细
│   └── expense-auditor.json
└── releases/
    └── v1.0.0/
```

---

## 三、核心文件格式

### 3.1 repo.yaml - 仓库元数据

```yaml
repo_id: finance-skills
name: Finance Skills
summary: finance domain skill pack
owners:
  - finance-platform
visibility: internal

defaults:
  release_channel: stable
  permissions:
    network: false
  connectors:
    - storage:read

skills:
  - id: invoice-extractor
    path: skills/invoice-extractor
```

### 3.2 skill.yaml - 技能定义

```yaml
id: invoice-extractor
name: Invoice Extractor
version: 1.0.0
status: active
summary: 从发票 PDF/图片中提取结构化字段

inputs:
  - name: file
    type: pdf
    required: true

outputs:
  - name: invoice_json
    type: json

permissions:
  network: false
  connectors:
    - storage:read

compatibility:
  agents:
    - name: copilot-chat-agent
      mode: prompt
      entry: .github/skills/invoice-extractor/SKILL.md
    - name: openai-chat
      mode: tool
      entry: skills/invoice-extractor/adapters/tool/tool.json
```

### 3.3 SKILL.md - Agent 文档

```markdown
---
name: invoice-extractor
description: 从发票 PDF 或扫描图片中提取结构化信息。触发：发票提取、报销扫描。
license: Apache-2.0
---

# Invoice Extractor

从发票中提取商户名称、金额、日期、税率等结构化信息。

## 使用时机
- 用户上传发票图片并要求"提取信息"
- 用户说"报销发票"、"扫描票据"

## 使用方法

\`\`\`python
from scripts.run import process
result = process("invoice.pdf")
\`\`\`
```

### 3.4 Skill 三层架构（Progressive Disclosure）

Skill 采用三层渐进式架构，控制 Token 成本：

| 层级 | 组件 | 加载时机 | 内容 |
|------|------|---------|------|
| **L1** | Metadata | 始终加载 | name + description（最小 Token 消耗） |
| **L2** | Core Instructions | 触发时加载 | SKILL.md 正文 - 工作流、步骤、示例 |
| **L3** | Resources | 按需加载 | Scripts、模板、参考文档 |

### 3.5 agent.yaml - Agent 定义（阶段二）

```yaml
id: finance-analyst
name: Finance Analyst
version: 1.0.0
description: 分析财务任务，选择并使用相关 Skills

skills:
  - invoice-extractor
  - expense-auditor
```

### 3.6 AGENT.md - Agent 文档

```markdown
---
name: finance-analyst
description: 财务分析 Agent，可调用发票提取和费用审计技能。
---

# Finance Analyst

我是财务分析 Agent。当用户提到发票处理、费用审计、财务分析时，我会选择合适的技能完成任务。

## 何时使用

- 用户要求处理发票
- 用户需要进行费用审计
- 用户询问财务相关问题

## 可用技能

- `invoice-extractor`：从发票提取结构化信息
- `expense-auditor`：审计费用报销
```

---

## 四、统一调用接口

所有 Agent 统一按两个动作接入：

- `resolve(skill_id, channel|version)` -> 返回版本、artifact、adapter 入口、权限声明
- `execute(skill_id, version, input_payload)` -> 执行并返回结果

---

## 五、Agent 适配策略

| Agent | 适配方式 | 入口路径 |
|-------|---------|---------|
| Copilot Chat Agent Mode | prompt | `.github/skills/<skill-id>/SKILL.md` |
| OpenAI/LangChain/CrewAI/n8n | tool | `adapters/tool/tool.json` |
| MCP 客户端（后续） | mcp | `adapters/mcp/*` |
| Claude Code | prompt | `.claude/skills/<skill-id>/SKILL.md` |

---

## 六、角色与职责

| 角色 | 职责 |
|------|------|
| Skill Owner | 维护 `skill.yaml`、业务逻辑、测试 |
| Repo Maintainer | 维护 `repo.yaml`、workflow、发布策略 |
| Security Reviewer | 审核权限字段与风险变更 |
| Agent Integrator | 实现 resolver/executor SDK |
