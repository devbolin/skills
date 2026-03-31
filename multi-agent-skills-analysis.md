# Skill 管理方案（整理版）

## 主题
多 Skill Polyrepo、分阶段建设、主流工具集成，以及 VS Code Copilot Chat Agent Mode 示例。

## 一句话结论
先以 GitHub 为唯一控制面落地第一阶段，使用 domain polyrepo + GitHub Agent Skills + GitHub Actions + Release + skill catalog。

**平台原则**：Repo 是维护与发布边界；Skill 是解析与调用边界；工具集成统一依赖 adapter 契约，不直接依赖仓库内部结构。

## 1. 方案目标与核心原则
- 支持一个 polyrepo 存放一组同领域、同 owner、同权限等级、同发布节奏的 skill。
- 支持多种 agent / tool 复用同一批 skill，但各工具不直接读取 repo 内部结构。
- 阶段 1 先只通过 GitHub 实现；后续再引入 Registry、MCP 和运行治理。
- 统一通过 Prompt Adapter / Tool Adapter / MCP Adapter / Workflow Adapter 暴露能力。

## 2. 推荐仓库形态：Domain Polyrepo + Per-Skill 管理
- 一个 repo = 一个领域的一组 skill，例如 skills-finance、skills-crm、skills-ops。
- repo 负责源码、评审、CI/CD、发布与共享资源；skill 负责被 agent 解析、调用。
- Registry（后续阶段）仍按单个 skill_id 建索引；agent 消费时按 skill_id resolve。

示例目录：
```text
skills-finance/
  repo.yaml
  .github/workflows/
  shared/
  skills/
    invoice-extractor/
      skill.yaml
      adapters/openai/SKILL.md
      adapters/workflow/graph.yaml
    expense-auditor/
      skill.yaml
```

## 3. 分阶段建设路线

### 阶段 1：GitHub-only MVP
- 仅依赖 GitHub 落地：domain polyrepo、GitHub Actions、GitHub Release、skill-catalog。
- 对 VS Code Copilot Chat Agent Mode，直接使用 GitHub 原生 Agent Skills：`.github/skills` + `.github/copilot-instructions.md`。
- 不引入独立 Registry；以仓库内技能与静态索引先跑通闭环。

### 阶段 2：GitHub-only 自动化增强
- 继续不引入独立服务，但自动化 skill-catalog 更新、stable/beta/deprecated 通道、schema 校验与 checksums。
- 把集中维护的 skill repo 产物同步到业务仓库的 `.github/skills`，保持 Copilot 原生消费方式不变。

### 阶段 3：引入轻量 Registry
- GitHub 退回为源码与发布源；Registry 负责 search / resolve / policy。
- 对 Copilot 保留 GitHub 原生入口，Registry 作为控制面决定哪些版本物化到目标仓库。

### 阶段 4：运行治理与通用平台化
- 补齐 MCP、租户级启停、权限策略、灰度、审计、观测与回滚。
- 本地可执行流程继续由 `.github/skills` 提供，外部系统能力通过 MCP Server 提供。

## 4. 发布流程（简化版）
- 作者在 domain polyrepo 中新增或修改 `skills/<skill-id>/`。
- 提交 PR 后，GitHub Actions 校验 `repo.yaml`、`skill.yaml`、`adapters`、`tests`，并检测本次变更涉及哪些 skill。
- Codeowners 审核通过后合并 `main`，触发 Release workflow。
- 按变更 skill 生成独立 artifact 与 manifest，创建 Git tag 与 GitHub Release。
- 若处于阶段 1/2，则同步更新 skill-catalog，或将选定版本同步到目标业务仓库的 `.github/skills`。

## 5. 执行流程（简化版）
- Agent 根据用户请求选择目标 `skill_id`。
- 阶段 1/2：通过 GitHub 上的 skill-catalog 与仓库内 `.github/skills` 解析到目标 skill；阶段 3/4：通过 Registry resolve。
- 拿到具体版本、artifact、adapter 与权限策略后，装载 skill 并进入 runtime 执行。
- 执行完成后回传结果；阶段 4 再统一沉淀成功率、时延、错误码与回滚策略。

## 6. 主流工具接入规范
统一原则：所有工具都通过 adapter 契约接入 skill，不直接依赖 repo 内部结构。

| 工具/框架 | 阶段 1 集成方式 | 后续演进 | 建议主适配器 |
|---|---|---|---|
| OpenAI / Responses API | 通过 Tool Adapter 暴露 `run_skill / list_skills`；由应用侧 resolver 调用 GitHub catalog。 | 阶段 2/3 补 MCP 或 Registry-backed tool。 | Tool / MCP |
| LangChain / LangGraph | 把 skill 封装为统一 SkillTool，不直接解析 repo。 | 后续可接 Registry 或 Workflow Adapter。 | Tool / Workflow |
| CrewAI | 知识/指令型 skill 走 Prompt Adapter；动作型走 Tool Adapter。 | 保持目录包方式，后续补 Registry 解析。 | Prompt / Tool |
| Dify / n8n | 优先通过 Tool Adapter 生成可安装或 HTTP 调用的执行入口。 | 后续补 MCP 与统一网关。 | Tool / MCP |
| Claude / MCP 客户端 | 阶段 1 可不重点适配。 | 阶段 2 起通过 MCP Adapter 标准接入。 | MCP |

## 7. 以 VS Code Copilot Chat Agent Mode 为例的分阶段集成

### 阶段 1：仓库内原生 Skill
- 直接在业务仓库中落地 `.github/skills/<skill-id>/SKILL.md`。
- 用 `.github/copilot-instructions.md` 提供团队级约束与默认行为。
- 开发者在 VS Code Copilot Chat 的 Agent Mode 中直接调用这些 skill。

### 阶段 2：集中维护、仓库内分发
- skill 源码集中维护在 domain skill repo 中。
- 通过 GitHub Actions 将已发布版本同步到各业务仓库的 `.github/skills`。
- Copilot 的消费方式保持不变，开发者无感。

### 阶段 3：Registry 控制面
- Registry 管理 skill 的版本、状态、适用仓库与分发策略。
- 最终仍把指定版本物化到业务仓库 `.github/skills` 中，Copilot 继续走原生入口。

### 阶段 4：本地技能 + MCP 组合
- 纯流程/规范类 skill 继续放在 `.github/skills` 中。
- 外部系统、统一服务、敏感权限相关能力通过 MCP Server 提供。
- Copilot Agent Mode 通过原生 skill + MCP 组合使用。

## 8. 建议优先落地的最小集合（MVP）
- 仓库：`skills-finance`、`skills-crm`、`skill-catalog`、`skill-workflows`。
- 关键文件：`repo.yaml`、每个 skill 的 `skill.yaml`、`.github/copilot-instructions.md`、`.github/skills/*/SKILL.md`。
- 流程：PR 校验 -> Codeowners 审核 -> GitHub Release -> 更新 catalog / 同步 skill。
- 工具接入：先把 Tool Adapter 作为统一集成面；Copilot 场景优先使用 GitHub 原生 Agent Skills。

## 9. 结论
最推荐的落地路径是：先用 GitHub 原生能力把 Skill 内容、发布与仓库内消费跑通，再逐步把“集中治理、版本解析、运行策略”抽到 Registry 和 MCP 上。对 VS Code Copilot Chat Agent Mode 而言，GitHub Agent Skills 是阶段 1 最自然、最低成本且演进最顺的入口。
