# 技能编写与发布指南

## 一、适用范围
本指南用于阶段一（GitHub-only + Plugin-first）下的 Skill 编写与维护。

当前规范要点：
- 仓库级真相源是 `pack.yaml`
- Skill 的模型消费入口是 `SKILL.md`
- `skill.yaml` 不是手工维护必需项，可作为构建期 generated manifest
- Subagent 规范已拆分到 [`SUBAGENT_AUTHORING.md`](./SUBAGENT_AUTHORING.md)
- Agent 侧接入与配置步骤见 [`AGENT_CONFIGURATION.md`](./AGENT_CONFIGURATION.md)

## 二、Skill 目录结构

```text
<skill-id>/
├── SKILL.md                  # 必需：元数据 + 指令
├── scripts/                  # 可选：脚本实现
├── adapters/                 # 可选：工具适配层
│   ├── tool/tool.json
│   ├── prompt/SKILL.md
│   └── workflow/graph.yaml
├── references/               # 可选：模板、参考资料
└── tests/                    # 可选：测试
```

## 三、SKILL.md 规范

### 3.1 Frontmatter（推荐字段）

```yaml
---
name: skill-name            # 必需：小写、数字、连字符
description: 清晰描述何时激活此技能
version: "1.0"              # 可选
author: "team-name"         # 可选
license: "Apache-2.0"      # 可选
tags: ["tag1", "tag2"]    # 可选
---
```

### 3.2 Description 写法（关键）
模板：

```text
[做什么]。[何时使用]。[关键词/触发词]。
```

示例：

```text
从发票 PDF 或图片中提取结构化信息。当用户说“提取发票信息”“报销扫描”“发票识别”时触发。
```

常见错误：
- `helps with coding`：过于模糊，无法触发
- 一个 Skill 覆盖所有场景：边界不清，调用不稳定
- 没有触发词：模型难以判断是否该调用

### 3.3 正文结构（推荐）

```markdown
# <技能名>

## 使用场景
- <场景1>
- <场景2>

## 不适用场景
- <边界1>
- <边界2>

## 使用方法
~~~<语言>
<示例代码>
~~~

## 注意事项
- <限制1>
- <限制2>
```

## 四、Progressive Disclosure 实践

| 层级 | 组件 | 加载时机 | 内容 |
|------|------|---------|------|
| L1 | Metadata | 始终加载 | `name + description` |
| L2 | Core Instructions | 触发时加载 | SKILL.md 主体 |
| L3 | Resources | 按需加载 | scripts/references/tests |

执行建议：
- L1 保持短且具体，优先提高“是否触发”的准确率
- L2 明确“何时用/何时不用”，减少误调用
- L3 不在初始提示中全量展开，按任务再读

## 五、pack.yaml 中的 Skill 声明
Pack 通过 `pack.yaml` 统一声明 Skill 入口，最小必需字段：
- `id`
- `path`
- `mode`
- `entry`

可选扩展字段：
- `description`
- `adapters`

示例：

```yaml
skills:
  - id: invoice-extractor
    path: skills/invoice-extractor
    description: Extract structured fields from invoices
    mode: prompt
    entry: skills/invoice-extractor/SKILL.md
    adapters:
      tool: skills/invoice-extractor/adapters/tool/tool.json
```

## 六、发布流程（Plugin-first）
1. 修改 `pack.yaml` 与 `skills/<skill-id>/SKILL.md`（按需改 adapters/scripts）
2. 提交 PR，触发 CI（结构、入口、测试）
3. 合并后创建 tag，触发 release workflow
4. 默认发布 plugin artifact
5. 若显式开启 `enable_skill_artifacts=true`，额外产出单 Skill artifact
6. 更新 catalog（默认 `plugin_ref`，可选 `skill_ref`）

## 七、编写检查清单

### SKILL.md
- [ ] frontmatter 正确（`name` 与目录匹配）
- [ ] `description` 包含触发词与激活上下文
- [ ] 列出使用场景与不适用场景
- [ ] 提供可执行示例或步骤
- [ ] 明确注意事项和限制

### pack.yaml 集成
- [ ] `skills[]` 包含该 skill 的入口与 mode
- [ ] `entry` 路径存在且可解析
- [ ] `adapters` 路径存在且与目标工具匹配

### 质量与治理
- [ ] 单一职责，不把多个任务混在一个 Skill
- [ ] 权限最小化（默认不放宽）
- [ ] 变更包含测试或最小验证记录
