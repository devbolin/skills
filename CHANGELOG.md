# 文档变更日志

## [v2.6] - 2026-05-14

### 修复

#### 文件命名一致性
- 统一 `docs/guides/` 下文件名从 SCREAMING_SNAKE_CASE 改为 kebab-case
- 修复 README.md 中 3 处损坏的文件引用链接
- 在历史调研文档中补充结构差异注记
- 更新所有 12 个文件中的对应路径引用

---

## [v2.5] - 2026-04-08

### 优化

#### 图表可读性
- 简化 mermaid 图表：节点文字更短、分支标注更清晰
- 统一风格：DESIGN.md、FLOW.md、AGENT_CONSUMPTION.md、HUMAN_WORKFLOW.md
- 提升 GitHub 渲染兼容性

---

## [v2.4] - 2026-04-07

### 修复

#### 文档一致性
- `templates/README.md`: 重写为 packs 概览，同时介绍 skills-devtools 和 slc-pack
- `ci.yml`: 改为遍历所有 pack（`templates/phase1/*/pack.yaml`）
- `catalog/index.json`: 添加 packs[] 数组，包含 slc-pack 的 8 个 skill
- `catalog/skills/`: 新增 8 个 slc-pack skill 的 JSON 文件
- `DESIGN.md`: `skills[].adapters` 标记为"（未来版本）"

---

## [v2.3] - 2026-04-07

### 新增

#### Phase 1 模板
- `templates/phase1/slc-pack/`: 新增 Software Development Lifecycle Pack
  - 8 个 Skill：requirements, architecture-design, code-implementation, code-review, test-plan, deployment, operations, documentation
  - 7 个 Agent：requirements-analyst, architect, developer, reviewer, test-engineer, sre, tech-writer
  - 覆盖软件开发生命周期全阶段

### 文档质量
- 所有 Skill 和 Agent 内容统一为英文
- 添加输出格式模板和具体示例
- 修复触发词重叠问题

---

## [v2.2] - 2026-04-07

### 修复

#### Critical
- `release.yml`: 移除硬编码 `ENABLE_SKILL_ARTIFACTS`，修复 skill artifact 打包逻辑
- `ci.yml`: 添加 `catalog_entry` 路径存在性校验
- `CONCEPTS.md`/`skill-authoring.md`: 修正 L1/L2/L3 渐进披露描述（实现方式是文件分离+显式引用）

#### High
- `FLOW.md`/`DESIGN.md`/`AGENT_CONSUMPTION.md`: 移除未定义的 Policy 机制，简化为 "有 skill_ref 且 enable_skill_artifacts=true"
- `release.yml`: 添加 `concurrency` 组防止并发发布竞态条件
- `CONCEPTS.md`: 添加术语说明，澄清 `agents/` 路径与 Subagent 概念对应
- `release.yml`: catalog 写入时更新 `generated_at`

#### Medium
- `ci.yml`: 添加 `PACK_ID` 格式校验（`^[a-zA-Z0-9_-]+$`），防止路径遍历
- `FLOW.md`: 新增 "Channel 管理" 章节，说明 Phase 1 语义（stable = latest tag）
- `release.yml`: `enable_skill_artifacts=false` 时主动清理 catalog 中的 `skill_ref`
- `CONCEPTS.md`/`AGENT_CONSUMPTION.md`: 修复损坏的跨引用（`#agent-consumption-mode`）
- `ci.yml`: 强制 Subagent 路径格式 `agents/<id>.md`
- `AGENT_CONSUMPTION.md`: 移除内联 Phase 3 特性引用

### 文档质量

- `ARCHITECTURE.md`: 分离架构设计与模板结构，移除 CI/Release 基线描述
- `templates/README.md`: 扩展为完整模板结构文档

---

## [v2.1] - 2026-04-06

### 架构澄清
- 明确 pack.yaml 与 catalog/ 的分层职责与架构关系
- pack.yaml：每个 pack 内一份，真相源
- catalog/：外层聚合索引，Phase 2 Registry 替代

### 文档结构
- templates/README.md 更新目录结构（展示全部 3 个 skill 和 agents/）
- .claude/agents/ 添加缺失的 frontmatter

### 修复
- 修复外部链接 404 问题（VS Code Copilot、GitHub Copilot Extensions、Cursor Rules）
- 在 CONCEPTS.md 术语表中添加 Hook 定义
- 澄清 CONFIG.md 中 skill_ref 字段的条件生成特性

### 文档质量
- 统一 skill-authoring.md 和 CONTRIBUTING.md 中的模板占位符为英文
- 模板示例与实际 templates/phase1/ 保持一致

---

## [v2.0] - 2026-04-01

### 更新
- 重构项目文档结构
- 新增 `README.md` 作为项目入口
- 新增 `ARCHITECTURE.md` 架构文档
- 新增 `CHANGELOG.md` 变更日志
- 新增 `CONTRIBUTING.md` 贡献指南
- 建立 `docs/` 文档层级结构

### 文档结构变更
- `docs/phase1/` - 阶段一实现文档
- `docs/guides/` - 用户指南
- `docs/schemas/` - Schema 参考
- `docs/references/` - 参考资料

### 废弃文档
以下文档已废弃，内容整合到新结构中：
- `AGENT_SKILLS_COMPREHENSIVE_REPORT.md`
- `CROSS_PLATFORM_AGENT_SKILLS_PLAN.md`
- `skill_management_plan_cn_v3.md`
- `skill_management_plan_cn_v5.md`
- `multi-agent-skills-analysis.md`

---

## [v1.0] - 2026-03-31

### 初始版本
- 提出跨平台 AI Agent Skills 管理方案
- 确定 GitHub 作为唯一控制面
- 定义 Domain Polyrepo 架构
- 设计阶段一路线图
