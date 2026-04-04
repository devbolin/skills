# Phase 3：多平台多模式适配

> 状态：规划中

## 目标

在 Phase 2 基础上，支持 Agent 通过不同消费模式（Prompt/Tool/MCP）调用 Skill，实现跨平台统一消费。

## 核心能力

| 能力 | 说明 |
|------|------|
| Prompt 模式（Copilot） | 通过 SKILL.md 直接驱动 Agent 行为 |
| Tool 模式（OpenAI） | 通过 tool.json 注入 Agent tools[] |
| MCP 模式 | 通过 MCP server 接入外部工具 |
| 统一 Adapter 层 | Skill 定义的 adapters/ 目录支持多平台适配 |

## 消费模式架构

```
                    ┌─────────────┐
                    │   Skill     │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │  Prompt  │ │   Tool   │ │   MCP    │
        │  (SKILL) │ │ (JSON)   │ │ (Server) │
        └──────────┘ └──────────┘ └──────────┘
              │            │            │
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │ Copilot  │ │ OpenAI   │ │  MCP     │
        │ Agent    │ │ Runtime  │ │ Clients  │
        └──────────┘ └──────────┘ └──────────┘
```

## 关键设计点

### Adapter 规范

每个 Skill 的 `adapters/` 目录：

```
skills/<skill-id>/
├── SKILL.md
├── adapters/
│   ├── prompt/           # Prompt 模式入口（默认）
│   ├── tool/
│   │   └── tool.json    # OpenAI tool 定义
│   └── mcp/
│       └── server.json   # MCP server 配置
```

### 模式选择策略

| 条件 | 默认模式 | 说明 |
|------|---------|------|
| Agent 支持 Prompt | Prompt | Phase 1 已在用 |
| Agent 使用 OpenAI tools[] | Tool | Phase 3 新增 |
| Agent 使用 MCP 协议 | MCP | Phase 3 新增 |

## 与 Phase 1/2 的关系

- Phase 1: Prompt-only，Plugin-first
- Phase 2: Registry + 多 Pack，Prompt 模式
- Phase 3: 扩展 Tool/MCP 模式支持，多平台统一消费

## 里程碑（待定）

- Sprint 1: Tool 模式 adapter 规范与实现
- Sprint 2: MCP 模式 adapter 规范与实现
- Sprint 3: 多平台集成验证

## 相关文档

- Phase 1: [../phase1/README.md](../phase1/README.md) - 单 Pack e2e 闭环
- Phase 2: [../phase2/README.md](../phase2/README.md) - 多 Pack 管理
- Phase 4: [../phase4/README.md](../phase4/README.md) - 企业级治理
