# 参考资料

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
| **Copilot Extensions** | ✅ GA | Marketplace 数十种扩展 |
| **Agent Mode** | ✅ 预览 | VS Code Insiders 1.98+ |
| **copilot-instructions** | ✅ 支持 | `.github/copilot-instructions.md` |
| **.github/skills** | ✅ 支持 | 项目级 Skills |
| **MCP** | 🔶 有限 | 通过 Extensions |

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

## Skills 市场

| 市场 | 说明 |
|------|------|
| [**anthropics/skills**](https://github.com/anthropics/skills) | Claude Code 官方技能 (16+) |
| [**agentskills/agentskills**](https://github.com/agentskills/agentskills) | 规范 (14.5k stars) |
| [**skillhub.club**](https://skillhub.club) | 36k+ Skills 语义搜索 |
| [**awesome-agent-skills**](https://github.com/VoltAgent/awesome-agent-skills) | 1000+ VoltAgent 整理 |
