# 贡献指南

欢迎贡献！请遵循以下规范。

## 文档规范

- 所有项目文档使用中文
- 文档文件使用 Markdown 格式
- 文件名使用 kebab-case（如 `SKILL_AUTHORING.md`）

## Skill 编写规范

### 必需文件

每个 Skill 必须包含：
- `skill.yaml` - 技能元数据
- `SKILL.md` - Agent 使用文档
- `scripts/` - 实现代码目录

### skill.yaml 必需字段

```yaml
id: <唯一标识>
name: <显示名称>
version: <语义版本>
summary: <简短描述>
```

### SKILL.md 必需 frontmatter

```yaml
---
name: <技能名>
description: <描述>
license: <许可证>
---
```

## 分支规范

- `main` - 主分支，稳定版本
- 功能开发使用特征分支

## 提交流程

1. Fork 仓库
2. 创建特征分支
3. 提交更改
4. 发起 Pull Request

## 审核标准

- Schema 校验通过
- 文档完整清晰
- 测试覆盖实现代码
