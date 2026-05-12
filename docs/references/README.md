# 参考资料

## 文件组织规范

### 目录分类

| 目录/文件 | 内容 | 来源 |
|-----------|------|------|
| `agentskills/` | Agent Skills 开放标准文档（格式规范无关具体工具） | [agentskills.io](https://agentskills.io) |
| `vscode-copilot/` | VS Code Copilot 官方文档存档（仅限 `code.visualstudio.com/docs/copilot/` 页面） | [code.visualstudio.com](https://code.visualstudio.com/docs/copilot/overview) |
| `agent-plugins.md` | 多工具插件/Skills 系统横向对比 | 多源汇总 |
| `skill-best-practices.md` | Skill 与 Subagent 编写最佳实践 | 多源汇总 |
| `agent-capability-platform-report.md` | 历史调研材料（阶段一之前的技术调研） | 内部 |

### 命名规范

1. 所有文件名使用 **kebab-case**（连字符分隔），禁止 snake_case
2. `vscode-copilot/` 内文件名 = 官方 URL 页面 slug（如 `custom-agents.md` 对应 `/copilot/customization/custom-agents`）；若 slug 过于通用（如 `overview`），则前加所属 section 名（如 `customization-overview.md`）
3. `agentskills/` 内文件名用动词短语描述文档用途（如 `adding-skills-support.md`、`optimizing-descriptions.md`）
4. 文件首行 `#` 标题应与文件名含义一致，不依赖目录上下文消歧

### 内容规范

#### 文档类型与元数据

按文档类型执行不同的元数据要求：

| 类型 | 说明 | 元数据要求 |
|------|------|-----------|
| **归档 (Archived)** | 从外部 URL 逐字抓取的官方文档，如 `vscode-copilot/*` | YAML frontmatter: `source`, `retrieved`(精确到日), `type: archived` |
| **标准文档 (Standard)** | 来自开放标准的本地存档，如 `agentskills/*` | YAML frontmatter: `source`, `retrieved`(精确到月), `type: standard` |
| **汇总 (Compilation)** | 从多源编译的横向对比/最佳实践，如 `agent-plugins.md`, `skill-best-practices.md` | YAML frontmatter: `sources`(列表), `last_updated`, `type: compilation` |
| **历史 (Historical)** | 内部调研材料，非当前规范，如 `agent-capability-platform-report.md` | YAML frontmatter: `status`, `superseded_by`, `type: historical` |

#### 格式规范

1. **标题**：文件第一行（frontmatter 之后）必须是 `# <标题>`，标题必须能独立表意（不依赖目录名理解内容）
2. **元数据格式**：使用 YAML frontmatter 包围在 `---` 之间，位于文件最顶部、标题之前
3. **语言**：标题与正文语言一致，同一文件内不混用中英文标题
4. **标题层级**：使用 ATX 标题（`##` → `###` → `####`），禁止跳级
5. **内部链接**：使用相对路径，如 `[标题](agentskills/overview.md)`
6. **列表一致性**：同一列表中所有项目使用相同的标记（全部 `-` 或全部 `*`）
7. **表格**：表头与内容行之间必须有分隔行 `|---|`
8. **代码块**：须标注语言，如 ` ```json`, ` ```yaml`, ` ```bash`

#### 内容准则

1. **归档文档**必须与官方源逐字核对，不做简化改写
2. **跨工具对比**须注明调研截止日期
3. **状态变更**：若文档所述功能状态变更（如预览→GA），须更新 README 中的状态标记并在文件名/标题中同步
4. **外部引用**：引用外部链接必须使用完整 URL，禁止使用短链接

## Agent 工具支持

### Claude Code

| 组件 | 状态 | 说明 |
|------|------|------|
| **Skills** | ✅ 完整 | SKILL.md 格式，`.claude/skills/` |
| **Plugins** | ✅ 完整 | `/plugin` 命令安装 |
| **MCP** | ✅ 完整 | 原生 Model Context Protocol |
| **Hooks** | ✅ 完整 | 事件钩子自动化 |
| **Commands** | ✅ 完整 | 斜杠命令 `/clear`, `/compact` |
| **Subagents** | ✅ 完整 | 多 Agent 协作 |

### GitHub Copilot

| 功能 | 状态 | 说明 |
|------|------|------|
| **Agents** | ✅ GA | Local / Copilot CLI / Cloud / Third-party 四种 Agent |
| **Agent Skills** | ✅ GA | `.github/skills/`, `.claude/skills/`, `.agents/skills/` 等位置 |
| **Custom Instructions** | ✅ GA | 项目级与用户级编码规范 |
| **Custom Agents** | ✅ GA | 自定义 Agent Persona |
| **Prompt Files** | ✅ GA | `.github/prompts/` 可复用提示词 |
| **MCP** | ✅ GA | Model Context Protocol 原生支持 |
| **Plugins** | ✅ 预览 | Marketplace 扩展贡献 Agents / Skills / MCP |
| **Hooks** | ✅ 预览 | 事件驱动的自动化钩子 |
| **Copilot Extensions** | ✅ GA | Marketplace 数十种扩展 |

### Cursor

| 功能 | 状态 | 说明 |
|------|------|------|
| **Rules** | ✅ 支持 | `.cursor/rules/` 或 `.cursorrules` |
| **Skills** | ✅ 支持 | 从 GitHub 安装 |
| **MCP** | ✅ 支持 | 原生 MCP |

### Gemini CLI

| 功能 | 状态 | 说明 |
|------|------|------|
| **MCP** | ✅ 完整 | 原生双轨制工具发现 |
| **Tools** | ✅ 完整 | 内置工具 + MCP 扩展 |
| **Skills** | 🔶 有限 | 通过 MCP 扩展 |

## MCP 生态

| 资源 | 链接 |
|------|------|
| 官网 | [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| 官方 SDKs | [Python](https://github.com/modelcontextprotocol/python-sdk), [TypeScript](https://github.com/modelcontextprotocol/typescript-sdk), [Go](https://github.com/modelcontextprotocol/golang-sdk), [Java](https://github.com/modelcontextprotocol/java-sdk), [C#](https://github.com/modelcontextprotocol/csharp-sdk) |
| MCP Servers | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |
| MCP Directory | [model-context-protocol.com](https://model-context-protocol.com) |

## Agent Skills 开放标准

本地存档的 [agentskills.io](https://agentskills.io) 文档：

| 文档 | 说明 |
|------|------|
| [概述](agentskills/overview.md) | Agent Skills 概览与核心理念 |
| [规范说明](agentskills/specification.md) | 完整格式规范 (frontmatter、目录结构、渐进式加载) |
| [最佳实践](agentskills/best-practices.md) | 编写高质量 Skill 的最佳实践 |
| [描述优化](agentskills/optimizing-descriptions.md) | 测试并优化 description 字段以提高触发准确率 |
| [输出质量评估](agentskills/evaluating-skills.md) | 通过 eval 驱动迭代测试 Skill 输出质量 |
| [脚本使用指南](agentskills/using-scripts.md) | 在 Skill 中运行命令和打包可执行脚本 |
| [快速入门](agentskills/quickstart.md) | 创建第一个 Agent Skill |
| [客户端支持](agentskills/clients.md) | 支持 Agent Skills 格式的产品列表 |
| [Agent 集成指南](agentskills/adding-skills-support.md) | 为你的 Agent 添加 Skills 支持 |

## VS Code Copilot 文档存档

来自 [code.visualstudio.com/docs/copilot/overview](https://code.visualstudio.com/docs/copilot/overview) 的本地存档：

### 入门与概览

| 文档 | 说明 |
|------|------|
| [概述](vscode-copilot/overview.md) | GitHub Copilot in VS Code 总览 — Agent、Chat、Inline Suggestions |
| [AI 功能概念](vscode-copilot/ai-features-overview.md) | AI 功能全景：Agents、Chat、Inline Chat、Inline Suggestions、Smart Actions |
| [安装配置](vscode-copilot/setup.md) | 安装步骤、GHE 账号、多账号配置、禁用 AI 功能 |
| [速查表](vscode-copilot/cheat-sheet.md) | 快捷键、Chat 操作、内置工具、Agent 功能速览 |
| [快速入门教程](vscode-copilot/getting-started.md) | 5 步上手：Inline Suggestions → Agents → Inline Chat → 个性化 → Smart Actions |
| [常见问题](vscode-copilot/faq.md) | 订阅、用量、故障排查、企业管理 FAQ |

### Agent

| 文档 | 说明 |
|------|------|
| [Agent 使用指南](vscode-copilot/agents-overview.md) | Agent 类型（Local/CLI/Cloud/Third-party）、内置 Agent、权限级别、Session 移交 |
| [Agent 核心概念](vscode-copilot/concepts-agents.md) | Agent Loop、Subagent（并行机制）、Memory、Planning 架构详解、Agents Application (Insiders) |

### 编辑器功能

| 文档 | 说明 |
|------|------|
| [内联建议](vscode-copilot/inline-suggestions.md) | Ghost Text、Next Edit Suggestions (NES)、配置与设置 |

### 概念

| 文档 | 说明 |
|------|------|
| [Context 概念](vscode-copilot/concepts-context.md) | 上下文组装、Workspace Indexing、隐式/显式上下文、conversation.md 格式、Participants/contextValue 对照表 |
| [Tools 概念](vscode-copilot/concepts-tools.md) | 内置工具、MCP Tools、Extension Tools、权限控制 |

### 自定义配置

| 文档 | 说明 |
|------|------|
| [自定义总览](vscode-copilot/customization-overview.md) | 所有自定义选项全景：Instructions / Prompts / Agents / Skills / MCP / Hooks / Plugins |
| [自定义指令](vscode-copilot/custom-instructions.md) | `copilot-instructions.md`、`*.instructions.md`、`AGENTS.md`、`CLAUDE.md`、指令优先级（个人 > 仓库 > 组织） |
| [Prompt 文件](vscode-copilot/prompt-files.md) | `.prompt.md` 斜杠命令：格式、位置、最佳实践 |
| [自定义 Agent](vscode-copilot/custom-agents.md) | `.agent.md` 格式、Handoff 工作流、工具/模型限制、agentify 自动 ID 生成、Claude 格式兼容 |
| [Agent Skills 使用指南](vscode-copilot/agent-skills.md) | SKILL.md 格式、创建/使用/分发 Skills、Forked Context、Slash 命令、扩展贡献 |
| [语言模型](vscode-copilot/language-models.md) | 模型选择、Thinking Effort、BYOK（API 类型/Provider 配置/消息角色）、Auto Selection、FAQ |
| [MCP 服务器](vscode-copilot/mcp-servers.md) | `mcp.json` 配置、stdio/HTTP/SSE、Sandbox、Dev Container、Auto-discovery、`--add-mcp` CLI |
| [MCP 配置参考](vscode-copilot/mcp-configuration.md) | 完整配置 schema（stdio/HTTP/SSE）、Input variables、Sandbox、Dev mode、命名规范、命令列表 |
| [Hooks](vscode-copilot/hooks.md) | 8 个生命周期事件、JSON schema、退出码（0/2/其他）、命令属性、Matcher 格式、事件序列 |
| [Plugins](vscode-copilot/plugins.md) | `plugin.json` 格式、REST/命令行/MCP 三种插件类型、manifest.json、OpenPlugin 映射、市场配置 |

## Skills 市场

| 市场 | 说明 |
|------|------|
| [**anthropics/skills**](https://github.com/anthropics/skills) | Claude Code 官方技能 (16+) |
| [**agentskills/agentskills**](https://github.com/agentskills/agentskills) | 规范 (14.5k stars) |
| [**skillhub.club**](https://skillhub.club) | 36k+ Skills 语义搜索 |
| [**awesome-agent-skills**](https://github.com/VoltAgent/awesome-agent-skills) | 1000+ VoltAgent 整理 |
