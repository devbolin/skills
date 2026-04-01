# 跨平台 AI Agent Skills 管理

> 版本：v2.0
> 更新：2026-04-01

以 **GitHub 为唯一控制面**，通过 **Domain Polyrepo** + **GitHub Actions** + **Release** + **Skill Catalog** 实现多 Agent 技能复用，支持 Claude Code、Copilot、Cursor、Gemini CLI 等主流工具。

**平台原则**：
- **Repo** 是维护与发布边界
- **Skill** 是解析与调用边界
- **Agent** 是 Skill 的消费者
- **Adapter** 是工具接入的统一契约

---

## 快速开始

- [模板使用指南](./templates/README.md) - 如何使用项目模板
- [阶段一设计](./docs/phase1/DESIGN.md) - GitHub-only MVP 架构
- [技能编写指南](./docs/guides/SKILL_AUTHORING.md) - 如何编写和发布 Skill
- [最佳实践](./docs/references/SKILL_BEST_PRACTICES.md) - SKILL.md 编写规范

---

## 文档结构

```
skill-management/
├── README.md                      # 项目入口
├── ARCHITECTURE.md                # 架构概述
├── templates/                      # 实现模板
│   └── README.md                  # 模板使用指南
│   └── phase1/                    # 阶段一模板
│
└── docs/
    ├── phase1/                   # 阶段一实现文档
    │   ├── DESIGN.md             # 阶段一设计
    │   ├── FLOW.md              # 开发到发布流程
    │   ├── CONFIG.md            # 配置指南
    │   └── VALIDATION.md        # 验证报告
    ├── guides/                   # 用户指南
    │   └── SKILL_AUTHORING.md   # 技能编写指南
    └── references/               # 参考资料
        ├── SKILL_BEST_PRACTICES.md  # 最佳实践
        └── AGENT_PLUGINS.md      # 各工具插件支持
```

---

## 核心概念

### Skill

Skill 是 AI Agent 的可复用能力单元，包含：
- **定义**：`skill.yaml` - 元数据、输入输出、权限
- **实现**：`scripts/` - 可执行代码
- **文档**：`SKILL.md` - 人类可读说明
- **适配器**：`adapters/` - 各工具专用格式

### Adapter

Adapter 是 Skill 对特定工具的适配层：

| Adapter 类型 | 用途 | 示例 |
|-------------|------|------|
| `prompt` | 指令型 | Copilot `.github/skills/` |
| `tool` | 工具型 | OpenAI Tool, LangChain |
| `workflow` | 流程型 | LangGraph, Dify |
| `mcp` | 协议型 | MCP 客户端 |

---

## Agent 支持对比

| 功能 | Claude Code | Copilot Chat | Gemini CLI | Cursor |
|------|------------|--------------|------------|--------|
| **原生 Skills** | `.claude/skills/` | `.github/skills/` | ❌ | `.cursor/rules/` |
| **MCP 支持** | ✅ | ✅ | ✅ | ✅ |
| **Plugin 市场** | ✅ | ✅ | 🔶 有限 | ✅ |
| **Hooks** | ✅ | ❌ | ❌ | ❌ |
| **Subagents** | ✅ | ❌ | ❌ | ❌ |

---

## 分阶段路线

| 阶段 | 目标 | 状态 |
|------|------|------|
| Phase 1 | GitHub-only MVP | 进行中 |
| Phase 2 | GitHub 自动化增强 | 规划中 |
| Phase 3 | 引入轻量 Registry | 规划中 |
| Phase 4 | 运行治理与 MCP | 规划中 |

---

## 关键链接

| 资源 | 链接 |
|------|------|
| Claude Code 文档 | code.claude.com/docs |
| GitHub Copilot | copilot.github.com |
| MCP 官方 | modelcontextprotocol.io |
| Anthropic Skills | github.com/anthropics/skills |
| Agent Skills 规范 | agentskills.io |

---

## 相关资源

- [Anthropics Skills](https://github.com/anthropics/skills) - Claude Code 官方技能
- [MCP Servers](https://github.com/modelcontextprotocol/servers) - 官方 MCP 服务器
- [SkillHub](https://skillhub.club) - Skills 市场
