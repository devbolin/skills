# 贡献指南

欢迎贡献！请遵循以下规范。

## 文档规范

- 所有项目文档使用中文
- 文档文件使用 Markdown 格式
- 文件名使用 kebab-case（如 `skill-authoring.md`）

## Skill 编写规范

### 必需文件

每个 Skill 必须包含：
- `SKILL.md` - Agent 使用文档（YAML frontmatter + Markdown）
- `pack.yaml` - Pack 级清单（阶段一唯一真相源）
- `scripts/` - 实现代码目录（可选）

> 注意：`skill.yaml` 是 generated manifest（构建期产物），不作为手工维护真相源。

### SKILL.md 必需 frontmatter

依据 [Agent Skills 规范](https://agentskills.io/specification)，仅 `name` 和 `description` 为必需字段：

```yaml
---
name: <skill-name>         # 必需：小写字母、数字、连字符
description: <description> # 必需：描述何时激活
license: <license>         # 可选：许可证
metadata:                  # 可选：附加元数据
  version: "1.0"
  author: "<author>"
  tags: "tag1, tag2"
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
