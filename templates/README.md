# 模板指南

本目录包含阶段一 Plugin-first 模板。

## 目录结构

```text
templates/phase1/
├── skills-finance/
│   ├── pack.yaml                  # Pack 级清单（唯一真相源）
│   ├── repo.yaml                  # 兼容文件（可选，建议迁移后删除）
│   ├── .github/workflows/
│   │   ├── ci.yml
│   │   └── release.yml
│   └── skills/
│       └── invoice-extractor/
│           ├── SKILL.md
│           ├── skill.yaml         # generated manifest 示例（非手工维护）
│           └── adapters/
├── schemas/
└── catalog/
```

## 使用方法

### 1. 创建领域仓库
```bash
cp -r templates/phase1/skills-finance my-skills-<domain>
```

### 2. 维护 Pack 清单
编辑 `pack.yaml`：
- `distribution.default: plugin`
- `distribution.enable_skill_artifacts: false`
- `skills[]` 中的入口和适配器声明

### 3. 维护 Skill 文档
- 编辑 `skills/<skill-id>/SKILL.md`
- 按需维护 `adapters/` 与 `scripts/`

### 4. 发布
- 打 tag 触发 `release.yml`
- 默认仅生成 plugin artifact
- 可选开启单 Skill artifact
