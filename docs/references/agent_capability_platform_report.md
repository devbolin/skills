# 多 Agent 能力包管理方案汇报

## 1. 背景与目标

我们当前要解决的问题，不是“如何存几份 prompt”，而是如何用一套统一方式管理一组可复用能力，并让不同 agent/runtime 共同消费。这些能力至少包括两层：第一层是 **skill**，代表最小可复用能力；第二层是 **agent**，代表带角色与技能组合的执行单元。第一阶段要求尽量轻量，只通过 GitHub 落地，同时为后续接入 VS Code Copilot、Claude Code、Gemini CLI 以及更多 MCP 客户端保留演进空间。

这套方案的目标有四个：

- 在 GitHub 中建立一套稳定、可复用、可审查的能力包源码规范。
- 让同一份源码能够被多个 agent 平台消费。
- 第一阶段尽快落地，不引入复杂控制面。
- 后续平滑演进到 plugin/extension、catalog、registry 和运行治理。

---

## 2. 调研结论

### 2.1 GitHub Copilot / VS Code 生态

GitHub Copilot 已明确支持 **Agent Skills**。官方定义里，skills 是由 instructions、scripts、resources 构成的目录；项目级 skills 存放在 `.github/skills` 或 `.claude/skills`，个人级 skills 存放在 `~/.copilot/skills` 或 `~/.claude/skills`。GitHub 还明确说明，这套 Agent Skills 是开放标准，并且适用于 Copilot coding agent、Copilot CLI 以及 VS Code Insiders 的 agent mode。

VS Code / Copilot 还支持 **Agent Plugins**。VS Code 文档把 agent plugins 定义为预打包的 chat customization bundle，一个插件里可以包含 skills、agents、hooks、slash commands、MCP servers，并支持从 marketplace、Git 仓库 URL 或本地目录安装、启用和禁用。

这意味着：Copilot/VS Code 既支持仓库内原生消费，也支持插件级分发。

### 2.2 Claude Code 生态

Claude Code 的 plugin 能力目前最完整。官方文档明确列出了插件可以提供的五类组件：**commands、agents、skills、hooks、MCP servers**。其中 skills 位于 `skills/`，每个 skill 是一个带 `SKILL.md` 的目录；agents 位于 `agents/`；hooks 位于 `hooks/hooks.json`；MCP servers 位于 `.mcp.json`。Claude Code 还支持通过插件市场安装插件，并支持用技能和 agent 自动匹配任务上下文。

这意味着：Claude Code 很适合作为“能力包”的正式分发目标，但它的 plugin 结构应被视为消费形态，而不是源码真相源。

### 2.3 Gemini CLI 生态

Gemini CLI 已有正式的 **extensions** 机制。官方说明里，extensions 用于打包 **prompts/context、MCP servers、custom commands**，支持安装、启用、更新，还支持从 GitHub URL、本地路径安装，并支持通过 GitHub Releases 做发布。

因此，Gemini CLI 是正式支持对象，适合作为轻量扩展包分发目标；但它的扩展模型比 Claude/VS Code 更轻，更偏 context、commands、MCP。

### 2.4 其他 agent / CLI

Cursor CLI、Amazon Q Developer CLI、Codex CLI 这类工具，目前最稳定的共同点是 **MCP**。对这类工具，更合理的策略不是给它们单独设计源码结构，而是通过统一 MCP server 接入能力。

### 2.5 总调研结论

源码层应只保留跨平台稳定共性：**skills、agents、可选的 MCP 配置**；平台专属的 plugin/extension 应在构建阶段生成。

---

## 3. 为什么最终要这样设计

### 3.1 必须支持多 agent，而不是单平台

如果一开始直接按 VS Code plugin、Claude plugin 或 Gemini extension 的某一种目录来组织源码，后面支持其他平台时一定会被格式绑死。因此，源码层必须平台中立，分发层再面向具体平台做转换。

### 3.2 第一阶段必须轻量落地

第一阶段已经明确要求：先只通过 GitHub 实现。这意味着不适合一开始就引入太多平台专属概念，也不适合先建很重的控制面。GitHub 已经足够承担源码、评审和发布角色。

### 3.3 后续必须能平滑扩展

方案不能只是“现在能跑”，还必须保证后续能够平滑演进到：

- plugin / extension
- 多 agent
- catalog
- registry
- MCP
- 运行治理

因此，第一阶段的设计必须同时满足两件事：**今天能做，明天不推翻**。

---

## 4. 最终设计方案

### 4.1 核心概念

第一阶段只保留三个层次：

- **Pack**：一个 GitHub 仓库，代表一个领域的一组能力。
- **Skill**：最小可复用能力单元。
- **Agent**：带角色和技能组合的执行单元。

这样设计的原因是：GitHub Copilot 生态天然区分 skills 和 custom agents，Claude Code plugin 也天然区分 skills 和 agents。这是一条有外部生态依据的最小模型。

### 4.2 仓库单元：Pack

一个仓库就是一个 **Pack**，例如：

- `finance-pack`
- `crm-pack`
- `ops-pack`

Pack 的职责是把相关的一组 skills 和 agents 组织在一起，并作为 GitHub 上的最小维护与发布单元。

### 4.3 第一阶段固定目录结构

第一阶段固定采用：

```text
<pack-root>/
  pack.yaml

  skills/
    <skill-id>/
      skill.yaml
      SKILL.md

  agents/
    <agent-id>/
      agent.yaml
      AGENT.md

  shared/
    references/
    prompts/
    assets/

  scripts/
    validate.py
    build.py

  mcp/            # 可选
    servers.json
```

### 4.4 每个文件的唯一职责

#### `pack.yaml`

位置固定：`<pack-root>/pack.yaml`

职责只有三件事：

- 说明这个 pack 是谁
- 列出 pack 里有哪些 skills
- 列出 pack 里有哪些 agents

最小示例：

```yaml
id: finance-pack
name: Finance Pack
version: 0.1.0

owners:
  - finance-platform

skills:
  - id: invoice-extractor
    path: skills/invoice-extractor
  - id: expense-auditor
    path: skills/expense-auditor

agents:
  - id: finance-analyst
    path: agents/finance-analyst
```

#### `skill.yaml`

位置固定：`<pack-root>/skills/<skill-id>/skill.yaml`

第一阶段只保留最小字段：

- `id`
- `name`
- `version`
- `description`
- `entry`
- `inputs`
- `outputs`

最小示例：

```yaml
id: invoice-extractor
name: Invoice Extractor
version: 0.1.0
description: Extract structured fields from invoice files.
entry: SKILL.md

inputs:
  - name: file
    type: file
    required: true

outputs:
  - name: invoice
    type: json
```

#### `agent.yaml`

位置固定：`<pack-root>/agents/<agent-id>/agent.yaml`

第一阶段只保留最小字段：

- `id`
- `name`
- `version`
- `description`
- `entry`
- `skills`

最小示例：

```yaml
id: finance-analyst
name: Finance Analyst
version: 0.1.0
description: Analyze finance-related tasks by selecting and using finance skills.
entry: AGENT.md

skills:
  - invoice-extractor
  - expense-auditor
```

#### `SKILL.md` 与 `AGENT.md`

- `SKILL.md`：给模型或 agent 看的 skill 说明，说明这个能力何时用、怎么用。
- `AGENT.md`：给模型或 runtime 看的 agent 说明，说明角色、边界、工作方式。

### 4.5 第一阶段明确不做的内容

第一阶段不做：

- `routing.yaml`
- schema
- hooks
- commands
- registry
- dependency graph
- 平台专属 plugin 目录

理由是：这些要么不是主流 agent 平台的通用原生标准，要么不是第一阶段跑通闭环的必要条件。

---

## 5. GitHub-only 的第一阶段落地方式

第一阶段完全可以只靠 GitHub：

### 5.1 源码仓库

每个领域一个 pack repo：

- `finance-pack`
- `crm-pack`

### 5.2 变更与审核

通过 PR 管理所有：

- skills 变更
- agents 变更
- shared 资源变更

### 5.3 发布

通过 GitHub Release 发布 pack 版本。第一阶段可以先不拆 skill/agent 单独产物，直接发布整个 pack 版本。

### 5.4 发现

如果 pack 数量不多，第一阶段甚至可以先不建 catalog，直接由消费端读取 `pack.yaml`。当 pack 数量上来后，再加一个静态 `capability-catalog` 仓库即可。

---

## 6. 各平台怎么消费这份源码

### 6.1 VS Code Copilot

第一阶段最稳妥的做法，是从 pack 构建仓库内原生形态：

- skills 生成到 `.github/skills/...`
- agents 生成到 `.github/agents/...` 或对应 custom agent 文件

第二阶段再从同一份源码构建 VS Code Agent Plugin。

### 6.2 Claude Code

第一阶段从 pack 构建：

- `skills/`
- `agents/`
- 可选 `.mcp.json`

第二阶段再构建 Claude plugin。

### 6.3 Gemini CLI

第一阶段先保留适配空间；第二阶段再把 pack 构建成 Gemini extension，对应：

- context/prompts
- custom commands
- MCP servers

### 6.4 其他工具

Cursor、Amazon Q、Codex CLI 这类工具，第一阶段不需要专门的 pack 转换格式。更合理的策略是第二阶段起通过 MCP 暴露统一能力，让它们作为 MCP client 消费。

---

## 7. 长期演进规划

### 阶段 1：最小可落地

目标：定下稳定的能力包源码规范，并让至少 1–2 个平台能消费。

交付：

- `pack.yaml`
- `skills/<id>/skill.yaml + SKILL.md`
- `agents/<id>/agent.yaml + AGENT.md`
- `shared/`
- `scripts/validate.py`
- `scripts/build.py`

特点：

- 只用 GitHub
- 不引入额外控制面
- 先把“源码如何组织”跑通

### 阶段 2：工程化增强

目标：在模型稳定后，增加工程保障与分发产物。

引入：

- `schemas/`：给 `pack.yaml` / `skill.yaml` / `agent.yaml` 做 JSON Schema 校验
- `commands/`
- `hooks/`
- 静态 `catalog` 自动生成
- 构建产物：
  - VS Code Agent Plugin
  - Claude Code Plugin
  - Gemini CLI Extension
- checksums 与发布通道

### 阶段 3：Capability Registry

目标：把静态 catalog 升级成真正控制面。

引入能力：

- pack search
- skill resolve
- agent resolve
- 版本与兼容矩阵
- 分发目标选择

### 阶段 4：统一运行治理

目标：真正平台化。

增加：

- 权限策略
- 审计
- 多租户分发
- 灰度与回滚
- 依赖图
- 运行观测

---

## 8. 风险与取舍

### 8.1 为什么第一阶段不直接做 plugin

因为 plugin 结构是平台专属消费形态，而不是源码真相源。第一阶段直接按 plugin 目录来组织源码，会把整个模型绑死在某一家平台上。

### 8.2 为什么第一阶段不做 registry

因为 registry 会显著增加系统复杂度，但在 pack 数量较少时，GitHub 已经足够承担源码、审核和发布角色。先跑通 pack 模型，比先做控制面更重要。

### 8.3 为什么第一阶段不加更多字段

因为没有明确用途的字段只会制造噪音。先把 `pack.yaml`、`skill.yaml`、`agent.yaml` 的最小字段稳定下来，比一开始塞进十几个“可能以后有用”的字段更可靠。

---

## 9. 最终建议

如果只保留一句话，建议这样汇报：

**第一阶段先建设“能力包源码规范”，而不是完整平台。**
以 GitHub 仓库作为一个 Pack 的真相源，根目录使用 `pack.yaml`，包内只保留 `skills/` 与 `agents/` 两类核心对象；通过 GitHub Release 管发布，通过仓库原生形态先接入 Copilot 与 Claude，再在第二阶段构建 VS Code Agent Plugin、Claude Code Plugin 和 Gemini CLI Extension；更广泛的 agent 统一通过 MCP 作为长期共同扩展底座。

---

## 10. 附录：第一阶段最小示例

```text
finance-pack/
  pack.yaml

  skills/
    invoice-extractor/
      skill.yaml
      SKILL.md

    expense-auditor/
      skill.yaml
      SKILL.md

  agents/
    finance-analyst/
      agent.yaml
      AGENT.md

  shared/
    references/
      field-dictionary.md

  scripts/
    validate.py
    build.py
```
