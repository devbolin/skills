# 模板指南

本目录包含阶段一实现的完整模板。

## 目录结构

```
templates/phase1/
├── skills-finance/              # 领域仓库模板
│   ├── repo.yaml               # 仓库元数据
│   ├── .github/
│   │   └── workflows/
│   │       ├── ci.yml         # PR 校验
│   │       └── release.yml    # 发布流程
│   └── skills/
│       └── invoice-extractor/ # 示例技能
│           ├── skill.yaml
│           ├── SKILL.md
│           ├── scripts/
│           ├── adapters/
│           └── tests/
├── schemas/                     # JSON Schema
│   ├── repo.schema.json
│   └── skill.schema.json
└── catalog/                     # Skill Catalog 模板
    ├── index.json
    └── skills/
```

## 使用方法

### 1. 创建领域仓库

```bash
# 复制模板
cp -r templates/phase1/skills-finance my-skills-<domain>

# 修改 repo.yaml 中的 repo_id 和 name
```

### 2. 创建新 Skill

```bash
# 在 skills/ 下创建目录
mkdir -p skills/<skill-id>/{scripts,adapters,tests}

# 创建必需文件
touch skills/<skill-id>/skill.yaml
touch skills/<skill-id>/SKILL.md
touch skills/<skill-id>/scripts/run.py
```

### 3. 配置 CI/CD

模板中的 GitHub Actions workflow 已配置好：
- `ci.yml` - PR 时自动校验 schema 和测试
- `release.yml` - 合并后自动发布

## Schema 文件（阶段二预置）

| 文件 | 用途 |
|------|------|
| `repo.schema.json` | 校验 `repo.yaml` |
| `skill.schema.json` | 校验 `skill.yaml` |

> 注：Schema 校验为阶段二特性，Phase 1 模板中预置但暂不强制启用。

## 示例

参考 `skills-finance/skills/invoice-extractor/` 了解完整结构。

## 自定义

- 修改 `repo.yaml` 中的 owners 和 visibility
- 修改 `skill.yaml` 中的权限和兼容性配置
- 更新 `.github/CODEOWNERS` 设置审核人
