# Agent 插件文档参考

本文档收集各主流 AI Agent 工具的插件/Skills 系统文档。

---

## Claude Code 插件

**官方文档**: https://code.claude.com/docs/en/plugins

### 最低版本要求

- **插件功能**: Claude Code **v1.0.33** 或更高版本
- 运行 `claude --version` 检查版本

### 概述

Claude Code 插件是扩展 Claude Code 功能的打包方案，可包含：slash commands、agent skills、custom agents、hooks 和 MCP servers。

### 插件 vs 独立配置

| 方式 | Skill 名称 | 适用场景 |
|------|-----------|---------|
| **独立** (`.claude/` 目录) | `/hello` | 个人工作流、项目特定定制、快速实验 |
| **插件** (含 `.claude-plugin/plugin.json`) | `/plugin-name:hello` | 团队共享、社区分发、版本控制 |

### 目录结构

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json          # 必需：插件清单
├── commands/                # Slash commands
├── agents/                  # 自定义 agents
├── skills/                  # Agent Skills
├── hooks/                   # 事件钩子
├── .mcp.json               # MCP 服务器配置
├── .lsp.json               # LSP 服务器配置
└── settings.json            # 默认设置
```

### plugin.json 示例

```json
{
  "name": "my-plugin",
  "description": "A greeting plugin",
  "version": "1.0.0",
  "author": { "name": "Your Name" }
}
```

### 安装方式

```bash
# 从源码安装
claude --plugin-dir ./my-plugin

# 官方市场
/plugin marketplace add anthropics/skills
/plugin install document-skills@anthropic-agent-skills
```

---

## VS Code Copilot Agent 插件

**官方文档**: https://code.visualstudio.com/docs/copilot/customization/agent-plugins

### 最低版本要求

- **Agent Mode**: VS Code **v1.99+** (v1.98 为预览版，仅 Insider)
- **Agent 插件**: VS Code **v1.98+** (预览)

### 概述

Agent 插件是预打包的聊天定制集合，单个插件可提供任意组合的 slash commands、agent skills、custom agents、hooks 和 MCP servers。

### 浏览与安装

在 Extensions 视图中搜索 `@agentPlugins` 可找到可用插件。

### 从源码安装

1. 运行 **Chat: Install Plugin From Source**
2. 输入 Git 仓库 URL

### 配置选项

```json
// 启用/禁用插件
"chat.plugins.enabled": true

// 添加市场
"chat.plugins.marketplaces": ["anthropics/claude-code"]

// 注册本地插件
"chat.pluginLocations": {
    "/path/to/my-plugin": true
}

// 工作区推荐插件
"enabledPlugins": { "code-formatter@company-tools": true }
```

---

## GitHub Copilot Extensions

**官方文档**: https://docs.github.com/en/copilot/building-copilot-extensions

### 发布状态

- **普遍可用 (GA)**: 2025年3月

### 概述

Copilot Extensions 允许扩展 GitHub Copilot 的功能，可与 Copilot Chat、Copilot CLI 等集成。

### 构建扩展

1. 创建 GitHub App 或使用 OAuth app
2. 定义扩展的命令和响应格式
3. 在 GitHub Marketplace 发布或内部分发

---

## Cursor Rules

**官方文档**: https://cursor.com/rules

### 最低版本要求

- **Project Rules (.mdc)**: Cursor **v0.46+**
- **User Rules**: Cursor **v0.45+** (原名 "Rules for AI")

### 概述

Cursor Rules 用于定制 Cursor AI 生成代码的行为，可定义代码风格、项目结构、技术栈要求等。

### 规则级别

| 级别 | 位置 | 范围 |
|------|------|------|
| User Rules | Cursor Settings -> Rules | 全局，所有项目通用 |
| Project Rules | `.cursor/rules/*.mdc` | 仅当前项目 |

### 目录结构

```
.cursor/
└── rules/
    ├── project-guidelines.mdc
    ├── naming-conventions.mdc
    └── tech-stack.mdc
```

### .mdc 文件格式

```markdown
---
description: 项目规范
scope: "src/**/*.ts"
priority: 5000
---

# 代码风格约束
1. 必须使用严格模式
2. 禁止 any 类型声明
```

### Rule Type 模式

- **Always**: 每次对话都自动携带
- **Auto Attached**: 根据文件类型自动匹配
- **Agent Request**: 由 Agent 判断是否生效
- **Manual**: 需手动 @

---

## Gemini CLI

**官方文档**: https://github.com/google-gemini/gemini-cli

### 发布信息

- **首次发布**: 2025年6月25日
- **当前版本**: v0.35+ (预览版)

### MCP 支持

Gemini CLI 原生支持 MCP 协议：

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

### 安装

```bash
npm install -g @google/gemini-cli
```

---

## 工具支持对比

| 工具 | 插件/Skills 系统 | 最低版本 | MCP 支持 | 发布时间 |
|------|-----------------|---------|----------|---------|
| Claude Code | Plugins | v1.0.33 | ✅ 原生 | 2024 |
| VS Code Copilot | Agent Plugins | v1.99 | ✅ | 2025-04 |
| GitHub Copilot | Extensions | GA | 🔶 有限 | 2025-03 |
| Cursor | Rules | v0.46 | ✅ | 2025-02 |
| Gemini CLI | MCP 扩展 | v0.1+ | ✅ 原生 | 2025-06 |

---

## 相关资源

| 工具 | 文档链接 |
|------|---------|
| Claude Code | code.claude.com/docs |
| VS Code Copilot | code.visualstudio.com/docs/copilot |
| GitHub Copilot Extensions | docs.github.com/copilot/building-copilot-extensions |
| Cursor Rules | cursor.com/rules |
| Gemini CLI | github.com/google/gemini-cli |
| MCP | modelcontextprotocol.io |
