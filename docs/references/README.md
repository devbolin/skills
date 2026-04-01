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
| 官网 | modelcontextprotocol.io |
| 官方 SDKs | Python, TypeScript, Go, Java, C# |
| MCP Servers | github.com/modelcontextprotocol/servers |
| MCP Directory | model-context-protocol.com |

## Skills 市场

| 市场 | 说明 |
|------|------|
| **anthropics/skills** | Claude Code 官方技能 (16+) |
| **agentskills/agentskills** | 规范 (14.5k stars) |
| **skillhub.club** | 36k+ Skills 语义搜索 |
| **awesome-agent-skills** | 1000+ VoltAgent 整理 |
