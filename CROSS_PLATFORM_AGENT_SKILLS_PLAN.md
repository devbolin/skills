# 跨平台 AI Agent Skills 管理方案

> 版本：v1.0
> 更新：2026-03-31
> 状态：阶段一详细设计

---

## 一句话结论

以 **GitHub 为唯一控制面**，通过 **Domain Polyrepo** + **GitHub Actions** + **Release** + **Skill Catalog** 实现多 Agent 技能复用，支持 Copilot、Claude、LangChain 等主流工具。

**平台原则**：
- **Repo** 是维护与发布边界
- **Skill** 是解析与调用边界
- **Adapter** 是工具接入的统一契约

---

## 二、核心概念

### 2.1 什么是 Skill？

Skill 是 AI Agent 的可复用能力单元，包含：
- **定义**：`skill.yaml` - 元数据、输入输出、权限
- **实现**：`scripts/` - 可执行代码
- **文档**：`SKILL.md` - 人类可读说明
- **适配器**：`adapters/` - 各工具专用格式

### 2.2 什么是 Adapter？

Adapter 是 Skill 对特定工具的适配层，让同一 Skill 可被不同 Agent 调用：

| Adapter 类型 | 用途 | 示例 |
|-------------|------|------|
| `prompt` | 指令型 | Copilot `.github/skills/` |
| `tool` | 工具型 | OpenAI Tool, LangChain |
| `workflow` | 流程型 | LangGraph, Dify |
| `mcp` | 协议型 | MCP 客户端（阶段二） |

---

## 三、仓库结构

### 3.1 Domain Polyrepo

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
│       └── ...
└── shared/                          # 共享资源
    └── prompts/
```

### 3.2 Skill Catalog 仓库

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

## 四、核心文件格式

### 4.1 repo.yaml - 仓库元数据

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

shared:
  prompts_path: shared/prompts
  libs_path: shared/libs
  tests_path: shared/tests

skills:
  - id: invoice-extractor
    path: skills/invoice-extractor
  - id: expense-auditor
    path: skills/expense-auditor
```

### 4.2 skill.yaml - 技能定义

```yaml
id: invoice-extractor
name: Invoice Extractor
version: 1.0.0
status: active
summary: 从发票 PDF/图片中提取结构化字段

owners:
  - finance-platform

inputs:
  - name: file
    type: pdf
    required: true
  - name: locale
    type: string
    required: false

outputs:
  - name: invoice_json
    type: json

runtime:
  type: python
  entry: scripts/run.py

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
    - name: langchain
      mode: tool
      entry: skills/invoice-extractor/adapters/tool/tool.json

release:
  channel: stable
  artifact: invoice-extractor-1.0.0.zip

observability:
  emit_metrics: true
  emit_trace: true
```

### 4.3 adapter/tool/tool.json - 工具适配器

```json
{
  "tool": {
    "name": "invoice_extractor",
    "description": "从发票 PDF 或图片中提取结构化信息",
    "input_schema": {
      "type": "object",
      "properties": {
        "file_path": {
          "type": "string",
          "description": "发票文件路径"
        },
        "locale": {
          "type": "string",
          "description": "语言设置",
          "default": "zh-CN"
        }
      },
      "required": ["file_path"]
    }
  }
}
```

### 4.4 SKILL.md - Agent 文档

```markdown
---
name: invoice-extractor
description: 从发票 PDF 或扫描图片中提取结构化信息。触发：发票提取、报销扫描、票据识别。
license: Apache-2.0
---

# Invoice Extractor

从发票中提取商户名称、金额、日期、税率等结构化信息。

## 使用时机
- 用户上传发票图片并要求"提取信息"
- 用户说"报销发票"、"扫描票据"

## 使用方法

```python
from scripts.run import process

result = process("invoice.pdf")
# result = {"merchant": "...", "amount": 100.0, ...}
```
```

### 4.5 catalog/index.json - 全局索引

```json
{
  "version": "1",
  "generated_at": "2026-03-31T12:00:00Z",
  "skills": [
    {
      "skill_id": "invoice-extractor",
      "repo_id": "finance-skills",
      "repo": "org/skills-finance",
      "path": "skills/invoice-extractor",
      "status": "active",
      "channels": {
        "stable": "1.0.0",
        "beta": "1.1.0-beta"
      },
      "catalog_entry": "skills/invoice-extractor.json"
    }
  ]
}
```

---

## 五、工具接入矩阵

| 工具/框架 | 阶段一 | Adapter 模式 | 入口路径 |
|-----------|--------|--------------|----------|
| **Copilot Chat Agent** | ✅ | `prompt` | `.github/skills/<skill>/SKILL.md` |
| **Claude Code** | ✅ | `prompt` | `.claude/skills/` |
| **Cursor** | ✅ | `prompt` | `.cursor/rules/` |
| **OpenAI Chat** | ✅ | `tool` | `adapters/tool/tool.json` |
| **LangChain** | ✅ | `tool` | `adapters/tool/tool.json` |
| **CrewAI** | ✅ | `tool/prompt` | `adapters/tool/` |
| **Dify / n8n** | 阶段二 | `workflow` | `adapters/workflow/` |
| **MCP 客户端** | 阶段二 | `mcp` | `adapters/mcp/` |

---

## 六、分阶段建设路线

### 阶段一：GitHub-only MVP（当前）

**目标**：不引入独立 Registry，纯用 GitHub 跑通闭环

**交付物**：
- [x] Domain Polyrepo 模板
- [x] `repo.yaml`、`skill.yaml` schema
- [x] GitHub Actions CI/CD
- [x] Skill Catalog 索引
- [x] Copilot + OpenAI Tool 接入示例

**边界**：
- 不实现独立在线 Registry
- 不实现租户级授权
- 不实现运行时调度

---

### 阶段二：GitHub 自动化增强

**目标**：增强发布自动化与可观测性

**交付物**：
- [ ] stable/beta/deprecated 通道
- [ ] Schema 校验 + checksums
- [ ] Skill Catalog 自动更新
- [ ] 定时同步到业务仓库

---

### 阶段三：引入轻量 Registry

**目标**：引入 Registry 做 search/resolve/policy

**架构**：
```
GitHub (源码) → Registry (控制面) → 各工具
```

**交付物**：
- [ ] Registry 服务（可选 GitHub Packages）
- [ ] Skill 版本解析
- [ ] 分发策略控制

---

### 阶段四：运行治理与 MCP

**目标**：完整平台化

**交付物**：
- [ ] MCP Server 接入
- [ ] 租户级启停
- [ ] 权限策略、灰度、审计

---

## 七、开发到发布流程

### 7.1 开发流程

```
1. 开发者修改 skills/<skill-id>/
2. 更新 skill.yaml 版本号
3. 发起 PR
```

### 7.2 CI 校验

PR 触发 `ci.yml`：
- 结构校验（目录、必需文件）
- JSON Schema 校验
- 测试执行

### 7.3 发布流程

```
1. Codeowners 审核通过
2. 合并到 main
3. 创建 Git Tag (v*.*.*)
4. 触发 release.yml
5. 打包 artifact + manifest + checksums
6. 创建 GitHub Release
7. 更新 Skill Catalog
```

### 7.4 Agent 调用流程

```
1. Agent 调用 resolve(skill_id, channel)
2. 读取 Skill Catalog index.json
3. 获取版本、artifact、adapter 入口
4. 下载 artifact 并调用 adapter
5. 返回结果并记录日志
```

---

## 八、验收标准

### 阶段一验收

- [ ] PR 能自动校验 schema/结构/测试
- [ ] 合并后自动发布 artifact 与 manifest
- [ ] Catalog 自动指向最新 stable
- [ ] Copilot 成功调用 skill
- [ ] OpenAI/LangChain 成功调用 skill

### 里程碑（建议 2~3 周）

| Week | 目标 |
|------|------|
| Week 1 | 结构与契约（schema + repo/skill manifest） |
| Week 2 | CI/Release 与 catalog 更新 |
| Week 3 | 至少 2 个 Agent 接入验证（Copilot + Tool Adapter） |

---

## 九、安全与治理

### 9.1 权限基线

```yaml
permissions:
  network: false          # 默认禁止网络
  connectors: []          # 显式声明连接器
```

### 9.2 发布安全

- 必须使用 Release Artifact，不直接引用 `main` 分支
- 发布产物必须附带 checksum
- 默认最小权限原则

### 9.3 可观测性

```yaml
observability:
  emit_metrics: true      # 记录调用指标
  emit_trace: true        # 记录调用链路
```

---

## 十、关键文件清单

```
skills-finance/                          # Domain Repo
├── repo.yaml                           # 仓库元数据
├── .github/workflows/
│   ├── ci.yml                         # PR 校验
│   └── release.yml                     # 发布流程
├── skills/
│   └── invoice-extractor/
│       ├── skill.yaml                  # 技能定义
│       ├── SKILL.md                    # Agent 文档
│       ├── scripts/run.py              # 入口脚本
│       ├── adapters/
│       │   ├── tool/tool.json          # 工具适配器
│       │   ├── prompt/SKILL.md         # Copilot 适配器
│       │   └── workflow/graph.yaml      # 流程适配器
│       └── tests/test_extractor.py
└── shared/

skill-catalog/                          # Catalog Repo
├── index.json                          # 全局索引
└── skills/
    └── invoice-extractor.json          # 技能明细
```

---

## 十一、参考资料

- 官方规范：[agentskills.io](https://agentskills.io/specification)
- 官方技能库：[anthropics/skills](https://github.com/anthropics/skills) (106k stars)
- 规范库：[agentskills/agentskills](https://github.com/agentskills/agentskills) (14.5k stars)
