# 参考资料

## 文件组织规范

### 目录分类

| 目录/文件 | 内容 | 来源 |
|-----------|------|------|
| `agentskills/` | Agent Skills 开放标准文档（格式规范无关具体工具） | [agentskills.io](https://agentskills.io) |
| `vscode-copilot/` | VS Code Copilot 官方文档存档（仅限 `code.visualstudio.com/docs/copilot/` 页面） | [code.visualstudio.com](https://code.visualstudio.com/docs/copilot/overview) |
| `opencode/` | OpenCode 官方文档存档 | [opencode.ai/docs](https://opencode.ai/docs) |
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

### OpenCode

| 功能 | 状态 | 说明 |
|------|------|------|
| **Skills** | ✅ 完整 | SKILL.md 格式，`.opencode/skills/`、`.claude/skills/` 兼容 |
| **Rules** | ✅ 完整 | `AGENTS.md` / `CLAUDE.md` 项目与全局规则 |
| **Custom Agents** | ✅ 完整 | `opencode.json` / Markdown 文件定义 primary & subagent |
| **Plugins** | ✅ 完整 | `.opencode/plugins/` + npm 包，事件钩子系统 |
| **MCP** | ✅ 完整 | 原生 Model Context Protocol |
| **Commands** | ✅ 完整 | 斜杠命令 + 自定义 command |
| **Models** | ✅ 完整 | 75+ 提供商，BYOK，本地模型 |
| **Permissions** | ✅ 完整 | 精细化权限管理（allow/ask/deny） |
| **LSP** | ✅ 完整 | 内置 LSP 服务器支持 |
| **Formatters** | ✅ 完整 | 内置 + 自定义代码格式化 |
| **Themes** | ✅ 完整 | TUI 主题系统 |
| **Share** | ✅ 完整 | 会话分享功能 |
| **ACP** | 🔶 预览 | Agent Client Protocol 支持 |

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

## OpenCode 文档存档

来自 [opencode.ai/docs](https://opencode.ai/docs) 的本地存档：

### 入门与使用

| 文档 | 说明 |
|------|------|
| [概述](opencode/intro.md) | OpenCode 产品总览 — 安装、配置、初始化、基本用法 |
| [配置](opencode/config.md) | `opencode.json` 完整配置参考（位置、schema、变量） |
| [提供商](opencode/providers.md) | 75+ LLM 提供商配置指南 |
| [网络](opencode/network.md) | 代理与自定义证书配置 |
| [企业版](opencode/enterprise.md) | 企业部署：SSO、内部 AI 网关、托管配置 |
| [故障排除](opencode/troubleshooting.md) | 日志、存储、常见问题排查 |
| [Windows (WSL)](opencode/windows-wsl.md) | Windows 上通过 WSL 使用 OpenCode |

### 使用方式

| 文档 | 说明 |
|------|------|
| [Go](opencode/go.md) | 低价订阅方案（$10/月，开源编码模型） |
| [TUI](opencode/tui.md) | 终端用户界面：命令、文件引用、编辑器设置 |
| [CLI](opencode/cli.md) | 命令行工具：完整命令参考与环境变量 |
| [Web](opencode/web.md) | 浏览器 Web 界面使用指南 |
| [IDE](opencode/ide.md) | VS Code / Cursor 等 IDE 扩展 |
| [Zen](opencode/zen.md) | 官方精选模型列表与定价 |
| [Share](opencode/share.md) | 会话分享功能 |
| [GitHub](opencode/github.md) | GitHub Actions 集成 |
| [GitLab](opencode/gitlab.md) | GitLab Duo 集成 |

### 配置

| 文档 | 说明 |
|------|------|
| [Tools](opencode/tools.md) | 工具系统：内置 / MCP / 自定义工具管理 |
| [Rules](opencode/rules.md) | `AGENTS.md` 项目规则与自定义指令 |
| [Agents](opencode/agents.md) | 内置 Agent 与自定义 Agent 配置 |
| [Models](opencode/models.md) | 模型选择与本地模型配置 |
| [Themes](opencode/themes.md) | TUI 主题系统 |
| [Keybinds](opencode/keybinds.md) | 键盘快捷键自定义 |
| [Commands](opencode/commands.md) | 自定义斜杠命令 |
| [Formatters](opencode/formatters.md) | 代码格式化配置 |
| [Permissions](opencode/permissions.md) | 权限管理（allow/ask/deny） |
| [LSP Servers](opencode/lsp.md) | LSP 服务器配置 |
| [MCP Servers](opencode/mcp-servers.md) | MCP 服务器配置 |
| [ACP Support](opencode/acp.md) | Agent Client Protocol 支持 |
| [Agent Skills](opencode/skills.md) | SKILL.md 技能系统 |
| [Custom Tools](opencode/custom-tools.md) | 自定义工具开发 |

### 开发

| 文档 | 说明 |
|------|------|
| [SDK](opencode/sdk.md) | OpenCode SDK 开发指南 |
| [Server](opencode/server.md) | HTTP Server API |
| [Plugins](opencode/plugins.md) | 插件系统：事件、自定义工具、生命周期钩子 |
| [Ecosystem](opencode/ecosystem.md) | 社区生态：插件、工具、集成 |

## Claude Code 文档存档

来自 [code.claude.com/docs](https://code.claude.com/docs) 的本地存档（[llms.txt](https://code.claude.com/docs/llms.txt)）：

### 入门与概览

| 文档 | 说明 |
|------|------|
| [概述](claude-code/overview.md) | Claude Code 产品总览 — 安装、能力、可用平台 |
| [快速入门](claude-code/quickstart.md) | 欢迎使用 Claude Code |
| [高级设置](claude-code/setup.md) | 系统要求、平台安装、版本管理、卸载 |
| [最佳实践](claude-code/best-practices.md) | Tips and patterns for getting the most out of Claude Code |
| [工作原理](claude-code/how-claude-code-works.md) | Agentic loop、内置工具、项目交互方式 |
| [常见工作流](claude-code/common-workflows.md) | 代码探索、调试、重构、测试等步骤指南 |
| [.claude 目录](claude-code/claude-directory.md) | CLAUDE.md、settings.json、hooks、skills 等文件结构 |
| [上下文窗口](claude-code/context-window.md) | 上下文窗口填充模拟器 |
| [检查点](claude-code/checkpointing.md) | 跟踪、回滚、汇总编辑与对话 |
| [记忆](claude-code/memory.md) | CLAUDE.md 持久指令与自动记忆 |
| [并行执行](claude-code/agents.md) | Subagents / Agent View / Agent Teams / Worktrees 对比 |

### 配置与管理

| 文档 | 说明 |
|------|------|
| [设置参考](claude-code/settings.md) | 全局与项目级设置、环境变量 |
| [模型配置](claude-code/model-config.md) | 模型别名 (如 `opusplan`)、模型选择 |
| [权限模式](claude-code/permission-modes.md) | 控制 Claude 是否在执行操作前询问 |
| [权限配置](claude-code/permissions.md) | 精细化权限规则、模式、托管策略 |
| [自动模式](claude-code/auto-mode-config.md) | 自动模式分类器配置与规则 |
| [配置调试](claude-code/debug-your-config.md) | 诊断 CLAUDE.md、settings、hooks 等失效原因 |
| [组织管理](claude-code/admin-setup.md) | 管理员部署决策指南 |
| [服务端托管设置](claude-code/server-managed-settings.md) | 服务器下发配置，无需设备管理 |
| [环境变量](claude-code/env-vars.md) | 环境变量完整参考 |
| [企业网络配置](claude-code/network-config.md) | 代理服务器、自定义 CA、mTLS |
| [沙箱](claude-code/sandboxing.md) | 沙箱 bash tool 提供文件系统与网络隔离 |
| [安全](claude-code/security.md) | 安全防护与最佳实践 |

### 扩展开发

| 文档 | 说明 |
|------|------|
| [扩展总览](claude-code/features-overview.md) | 何时使用 CLAUDE.md / Skills / Subagents / Hooks / MCP / Plugins |
| [Skills](claude-code/skills.md) | 创建、管理、分享 Skills，含自定义命令与绑定 Skill |
| [Subagents](claude-code/sub-agents.md) | 创建专用的 AI Subagent |
| [Plugins 创建](claude-code/plugins.md) | 创建自定义插件扩展 Skills/Agents/Hooks/MCP |
| [Plugins 参考](claude-code/plugins-reference.md) | 插件系统完整技术参考（schema、CLI、组件） |
| [插件依赖](claude-code/plugin-dependencies.md) | 声明插件依赖版本约束 |
| [插件市场](claude-code/plugin-marketplaces.md) | 搭建与分发插件市场 |
| [插件发现](claude-code/discover-plugins.md) | 从市场发现和安装预构建插件 |
| [Hooks 参考](claude-code/hooks.md) | Hook 事件、配置 schema、输入/输出格式完整参考 |
| [Hooks 指南](claude-code/hooks-guide.md) | 用 Hook 自动化工作流 |
| [MCP 指南](claude-code/mcp.md) | 通过 MCP 连接外部工具 |
| [Channels](claude-code/channels.md) | 通过 MCP 向运行中的会话推送消息/告警/Webhook |
| [Channels 参考](claude-code/channels-reference.md) | Channel 合约技术参考 |
| [命令参考](claude-code/commands.md) | 内置命令与捆绑 Skill 完整参考 |
| [Routines](claude-code/routines.md) | 让 Claude Code 自动运行（定时/API/事件触发） |
| [代码审查](claude-code/code-review.md) | 自动化 PR 审查 — 逻辑错误、安全漏洞、回归 |

### 平台与集成

| 文档 | 说明 |
|------|------|
| [VS Code](claude-code/vs-code.md) | VS Code 扩展安装与使用 |
| [GitHub Actions](claude-code/github-actions.md) | GitHub Actions CI/CD 集成 |
| [GitLab CI/CD](claude-code/gitlab-ci-c.md) | GitLab CI/CD 集成 |
| [企业部署](claude-code/third-party-integrations.md) | 第三方服务集成概览 |
| [平台总览](claude-code/platforms.md) | CLI/Desktop/VS Code/JetBrains/Web/移动端对比 |

### 团队与运营

| 文档 | 说明 |
|------|------|
| [分析仪表盘](claude-code/analytics.md) | 查看用量指标、跟踪采纳率 |
| [成本管理](claude-code/costs.md) | Token 跟踪、团队限额、成本优化 |
| [监控](claude-code/monitoring-usage.md) | 启用 OpenTelemetry 监控 |

### Agent 协作

| 文档 | 说明 |
|------|------|
| [Agent Teams](claude-code/agent-teams.md) | 编排多 Claude Code 会话团队协作 |
| [Agent View](claude-code/agent-view.md) | 统一管理多个并行会话 |
| [Worktrees](claude-code/worktrees.md) | Git worktree 隔离并行会话 |
| [无头模式](claude-code/headless.md) | 以编程方式运行 Claude Code |

### Agent SDK

| 文档 | 说明 |
|------|------|
| [SDK 概述](claude-code/agent-sdk-overview.md) | 以库的形式构建生产级 AI Agent |
| [SDK 快速入门](claude-code/agent-sdk-quickstart.md) | Python / TypeScript SDK 快速上手 |
| [Agent 循环](claude-code/agent-sdk-agent-loop.md) | 消息生命周期、工具执行、上下文窗口 |
| [Claude Code 功能](claude-code/agent-sdk-claude-code-features.md) | 在 SDK 中加载项目指令/Skills/Hooks |
| [自定义工具](claude-code/agent-sdk-custom-tools.md) | 使用内进程 MCP 服务器定义自定义工具 |
| [SDK Hooks](claude-code/agent-sdk-hooks.md) | 在关键执行点拦截和自定义行为 |
| [SDK MCP](claude-code/agent-sdk-mcp.md) | 配置 MCP 服务器扩展 Agent |
| [SDK 权限](claude-code/agent-sdk-permissions.md) | 权限模式、Hooks、声明式规则 |
| [SDK Plugins](claude-code/agent-sdk-plugins.md) | 加载自定义插件扩展 Agent |
| [SDK Skills](claude-code/agent-sdk-skills.md) | 在 SDK 中使用 Agent Skills |
| [SDK Slash 命令](claude-code/agent-sdk-slash-commands.md) | 通过 SDK 控制 Claude Code 会话 |
| [SDK Subagents](claude-code/agent-sdk-subagents.md) | 定义和调用 Subagent |
| [工具搜索](claude-code/agent-sdk-tool-search.md) | 按需发现和加载数千工具 |
| [流式输出](claude-code/agent-sdk-streaming-output.md) | 实时获取文本和工具调用响应 |
| [流式 vs 单次](claude-code/agent-sdk-streaming-vs-single-mode.md) | 两种输入模式对比 |
| [结构化输出](claude-code/agent-sdk-structured-outputs.md) | 使用 JSON Schema / Zod / Pydantic 获取结构化数据 |
| [Session 管理](claude-code/agent-sdk-sessions.md) | 持久化会话历史：continue / resume / fork |
| [用户输入处理](claude-code/agent-sdk-user-input.md) | 审批请求与澄清问题处理 |
| [成本追踪](claude-code/agent-sdk-cost-tracking.md) | Token 用量、成本估算、Prompt 缓存 |
| [可观测性](claude-code/agent-sdk-observability.md) | OpenTelemetry 追踪和指标导出 |
| [文件检查点](claude-code/agent-sdk-file-checkpointing.md) | 追踪文件修改、恢复历史状态 |
| [SDK 托管](claude-code/agent-sdk-hosting.md) | 生产环境部署与托管 |
| [安全部署](claude-code/agent-sdk-secure-deployment.md) | 隔离、凭证管理、网络控制 |
| [迁移指南](claude-code/agent-sdk-migration-guide.md) | 从旧版 TypeScript/Python SDK 迁移 |
| [系统提示词修改](claude-code/agent-sdk-modifying-system-prompts.md) | 自定义系统提示词 |
| [Python 参考](claude-code/agent-sdk-python.md) | Python SDK 完整 API 参考 |
| [TypeScript 参考](claude-code/agent-sdk-typescript.md) | TypeScript SDK 完整 API 参考 |

### 参考

| 文档 | 说明 |
|------|------|
| [CLI 参考](claude-code/cli-reference.md) | CLI 命令与标志完整参考 |
| [工具参考](claude-code/tools-reference.md) | 内置工具完整参考（含权限要求） |
| [术语表](claude-code/glossary.md) | Claude Code 术语定义 |

### 更新日志

| 文档 | 说明 |
|------|------|
| [更新日志](claude-code/changelog.md) | 按版本发布的完整更新日志 |
| [What's New](claude-code/whats-new-index.md) | 每周新功能摘要索引 |

## Skills 市场

| 市场 | 说明 |
|------|------|
| [**anthropics/skills**](https://github.com/anthropics/skills) | Claude Code 官方技能 (16+) |
| [**agentskills/agentskills**](https://github.com/agentskills/agentskills) | 规范 (14.5k stars) |
| [**skillhub.club**](https://skillhub.club) | 36k+ Skills 语义搜索 |
| [**awesome-agent-skills**](https://github.com/VoltAgent/awesome-agent-skills) | 1000+ VoltAgent 整理 |
