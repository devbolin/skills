# 跨平台 AI Agent Skills 完整方案

> 版本：v2.0
> 更新：2026-04-01
> 状态：最终版

---

## 一句话结论

以 **GitHub 为唯一控制面**，通过 **Domain Polyrepo** + **GitHub Actions** + **Release** + **Skill Catalog** 实现多 Agent 技能复用，支持 Claude Code、Copilot、Cursor、Gemini CLI 等主流工具。

**平台原则**：
- **Repo** 是维护与发布边界
- **Skill** 是解析与调用边界
- **Adapter** 是工具接入的统一契约

---

## 二、主流 Agent 工具插件支持

### 2.1 Claude Code

| 组件 | 状态 | 说明 |
|------|------|------|
| **Skills** | ✅ 完整 | SKILL.md 格式，`.claude/skills/` |
| **Plugins** | ✅ 完整 | `/plugin` 命令安装 |
| **MCP** | ✅ 完整 | 原生 Model Context Protocol |
| **Hooks** | ✅ 完整 | 事件钩子自动化 |
| **Commands** | ✅ 完整 | 斜杠命令 `/clear`, `/compact` |
| **Subagents** | ✅ 完整 | 多 Agent 协作 |

**Skills 安装方式**：
```bash
# 官方市场
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills

# 手动
mkdir -p ~/.claude/skills/<skill-name>
# 放入 SKILL.md 即可生效
```

**Skills 存储位置**：
```
~/.claude/skills/<skill-name>/SKILL.md     # 全局
<project>/.claude/skills/<skill-name>/   # 项目级
```

**七大组件**：
1. **CLAUDE.md** - 项目记忆配置
2. **Skills** - 详细技能指南（SKILL.md）
3. **Commands** - 斜杠命令入口
4. **MCP** - 外部工具协议
5. **Hooks** - 事件钩子
6. **Subagents** - 多 Agent 协作
7. **Plugins** - 预制菜包

---

### 2.2 GitHub Copilot

| 功能 | 状态 | 说明 |
|------|------|------|
| **Copilot Extensions** | ✅ GA | Marketplace 数十种扩展 |
| **Agent Mode** | ✅ 预览 | VS Code Insiders 1.98+ |
| **copilot-instructions** | ✅ 支持 | `.github/copilot-instructions.md` |
| **.github/skills** | ✅ 支持 | 项目级 Skills |
| **MCP** | 🔶 有限 | 通过 Extensions |

**Skills 存储位置**：
```
.github/copilot-instructions.md      # 团队级指令
.github/skills/<skill>/SKILL.md      # 项目级
.vscode/copilot-instructions.md     # 工作区级
```

**官方文档**：[copilot.github.com](https://copilot.github.com)

---

### 2.3 VS Code Copilot Chat (Agent Mode)

| 功能 | 状态 | 启用方式 |
|------|------|----------|
| **Agent Mode** | ✅ | `chat.agent.enabled: true` |
| **Skills** | ✅ | `.github/skills/` |
| **MCP** | ✅ | VS Code MCP 支持 |
| **Extensions** | ✅ | GitHub Marketplace |

**版本要求**：VS Code Insiders 1.98+

---

### 2.4 Cursor

| 功能 | 状态 | 说明 |
|------|------|------|
| **Rules** | ✅ 支持 | `.cursor/rules/` 或 `.cursorrules` |
| **Skills** | ✅ 支持 | 从 GitHub 安装 |
| **MCP** | ✅ 支持 | 原生 MCP |
| **VS Code 扩展** | ✅ | Cursor Rules (33k+ installs) |

**相关项目**：[PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)

---

### 2.5 Gemini CLI (Google)

| 功能 | 状态 | 说明 |
|------|------|------|
| **MCP** | ✅ 完整 | 原生双轨制工具发现 |
| **Tools** | ✅ 完整 | 内置工具 + MCP 扩展 |
| **Skills** | 🔶 有限 | 通过 MCP 扩展 |
| **Extensions** | 🔶 有限 | 社区贡献 |

**MCP 配置**：
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@some/mcp-server"]
    }
  }
}
```

**安装**：`npm install -g @google/gemini-cli`

---

### 2.6 工具支持对比

| 功能 | Claude Code | Copilot Chat | Gemini CLI | Cursor |
|------|------------|--------------|------------|--------|
| **原生 Skills** | `.claude/skills/` | `.github/skills/` | ❌ | `.cursor/rules/` |
| **MCP 支持** | ✅ | ✅ | ✅ | ✅ |
| **Plugin 市场** | ✅ | ✅ | 🔶 有限 | ✅ |
| **Hooks** | ✅ | ❌ | ❌ | ❌ |
| **Subagents** | ✅ | ❌ | ❌ | ❌ |
| **Commands** | ✅ | ❌ | ❌ | ❌ |
| **官方市场** | ✅ | ✅ | ❌ | ✅ |

---

## 三、MCP (Model Context Protocol) 生态

**官网**：[modelcontextprotocol.io](https://modelcontextprotocol.io)
**官方 SDKs**：Python, TypeScript, Go, Java, C#

### 3.1 官方 MCP Servers

**仓库**：[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) (82.6k stars)

| Server | 说明 |
|--------|------|
| Filesystem | 文件系统操作 |
| Git | Git 操作 |
| Memory | 记忆存储 |
| Sequential Thinking | 顺序思考 |
| Time | 时间查询 |
| Fetch | HTTP 请求 |
| Everything | 综合工具 |

### 3.2 MCP 生态资源

| 资源 | 链接 |
|------|------|
| MCP Directory | model-context-protocol.com |
| MCP Registry | registry.modelcontextprotocol.io |
| MCP.ad | mcp.ad |
| 官方 Servers | github.com/modelcontextprotocol/servers |
| Python SDK | github.com/modelcontextprotocol/python-sdk |

### 3.3 MCP 支持对比

| 平台 | MCP 状态 |
|------|----------|
| **Claude Code** | ✅ 完全支持 |
| **Cursor** | ✅ 完全支持 |
| **VS Code** | ✅ MCP Proxy |
| **JetBrains** | ✅ 内置 2025.2+ |
| **Gemini CLI** | ✅ 原生支持 |
| **GitHub Copilot** | 🔶 有限 |

---

## 四、Skills 市场与资源

### 4.1 官方 Skills 市场

| 市场 | 数量 | 说明 |
|------|------|------|
| **anthropics/skills** | 16+ 官方 | Claude Code 官方技能 |
| **agentskills/agentskills** | 规范 | 14.5k stars |
| **skillhub.club** | 36k+ | 语义搜索 + 一键安装 |
| **awesome-agent-skills** | 1000+ | VoltAgent 整理 |
| **awesome-copilot** | 27.6k stars | Copilot 资源 |
| **awesome-cursorrules** | Rules | Cursor Rules 合集 |

### 4.2 Claude Code 必装 Skills

| Skill | 描述 |
|-------|------|
| pdf | PDF 全能处理 |
| xlsx | Excel/表格处理 |
| docx | Word 文档生成 |
| data-analysis | 数据全链路分析 |
| frontend-design | 高质量前端界面 |
| webapp-testing | Web 应用自动化测试 |
| ffmpeg-usage | 音视频处理 |
| mcp-builder | 构建 MCP 服务器 |

---

## 五、核心概念

### 5.1 什么是 Skill？

Skill 是 AI Agent 的可复用能力单元，包含：
- **定义**：`skill.yaml` - 元数据、输入输出、权限
- **实现**：`scripts/` - 可执行代码
- **文档**：`SKILL.md` - 人类可读说明
- **适配器**：`adapters/` - 各工具专用格式

### 5.2 什么是 Adapter？

Adapter 是 Skill 对特定工具的适配层：

| Adapter 类型 | 用途 | 示例 |
|-------------|------|------|
| `prompt` | 指令型 | Copilot `.github/skills/` |
| `tool` | 工具型 | OpenAI Tool, LangChain |
| `workflow` | 流程型 | LangGraph, Dify |
| `mcp` | 协议型 | MCP 客户端 |

---

## 六、仓库结构

### 6.1 Domain Polyrepo

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
└── shared/                          # 共享资源
```

### 6.2 Skill Catalog 仓库

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

## 七、核心文件格式

### 7.1 repo.yaml - 仓库元数据

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

### 7.2 skill.yaml - 技能定义

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

### 7.3 SKILL.md - Agent 文档

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

---

## 八、分阶段建设路线

### 阶段一：GitHub-only MVP（当前）

**目标**：不引入独立 Registry，纯用 GitHub 跑通闭环

**交付物**：
- [x] Domain Polyrepo 模板
- [x] `repo.yaml`、`skill.yaml` schema
- [x] GitHub Actions CI/CD
- [x] Skill Catalog 索引
- [x] Copilot + OpenAI Tool 接入示例

---

### 阶段二：GitHub 自动化增强

- stable/beta/deprecated 通道
- Schema 校验 + checksums
- Skill Catalog 自动更新
- 定时同步到业务仓库

---

### 阶段三：引入轻量 Registry

- Registry 服务（可选 GitHub Packages）
- Skill 版本解析
- 分发策略控制

---

### 阶段四：运行治理与 MCP

- MCP Server 接入
- 租户级启停
- 权限策略、灰度、审计

---

## 九、开发到发布流程

### 9.1 开发流程

```
1. 开发者修改 skills/<skill-id>/
2. 更新 skill.yaml 版本号
3. 发起 PR
```

### 9.2 CI 校验

PR 触发 `ci.yml`：
- 结构校验（目录、必需文件）
- JSON Schema 校验
- 测试执行

### 9.3 发布流程

```
1. Codeowners 审核通过
2. 合并到 main
3. 创建 Git Tag (v*.*.*)
4. 触发 release.yml
5. 打包 artifact + manifest + checksums
6. 创建 GitHub Release
7. 更新 Skill Catalog
```

---

## 十、关键链接

| 资源 | 链接 |
|------|------|
| Claude Code 文档 | code.claude.com/docs |
| GitHub Copilot | copilot.github.com |
| MCP 官方 | modelcontextprotocol.io |
| Anthropic Skills | github.com/anthropics/skills |
| MCP Servers | github.com/modelcontextprotocol/servers |
| SkillHub | skillhub.club |
| Agent Skills 规范 | agentskills.io |

---

## 十一、总结

### 11.1 方案优势

1. **官方标准兼容**：遵循 agentskills.io 开放标准
2. **多工具复用**：一套 Skill，多个 Agent 可用
3. **GitHub 原生**：无需额外基础设施
4. **渐进式演进**：分阶段建设，降低风险

### 11.2 推荐实践

| 场景 | 推荐方案 |
|------|----------|
| **新项目** | 直接采用官方 Agent Skills 格式 |
| **多工具兼容** | 官方格式 + 各工具 Adapter |
| **企业定制** | Domain Polyrepo + 私有 Skill 市场 |
| **5+ 工具** | 转换脚本 + 格式适配 |

### 11.3 趋势

1. **官方标准已确立** - Anthropic 的 Agent Skills 是目前最被广泛接受的格式
2. **MCP 成为标配** - 主流工具纷纷支持 MCP 协议
3. **Skills 市场繁荣** - 从 GitHub 仓库向专业化平台演进
4. **多 Agent 协作** - Subagents 成为复杂任务标配
