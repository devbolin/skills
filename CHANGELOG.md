# 文档变更日志

## [v2.1] - 2026-04-06

### 修复
- 修复外部链接 404 问题（VS Code Copilot、GitHub Copilot Extensions、Cursor Rules）
- 在 CONCEPTS.md 术语表中添加 Hook 定义
- 澄清 CONFIG.md 中 skill_ref 字段的条件生成特性

### 文档质量
- 统一 SKILL_AUTHORING.md 和 CONTRIBUTING.md 中的模板占位符为英文
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
