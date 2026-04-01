# 技能编写与发布指南

## 一、概述

本文档说明如何创建、编写和发布 Skill。

---

## 二、Skill 三层架构（Progressive Disclosure）

Skill 采用三层渐进式架构，控制 Token 成本：

| 层级 | 组件 | 加载时机 | 内容 |
|------|------|---------|------|
| **L1** | Metadata | 始终加载 | name + description（最小 Token 消耗） |
| **L2** | Core Instructions | 触发时加载 | SKILL.md 正文 - 工作流、步骤、示例 |
| **L3** | Resources | 按需加载 | Scripts、模板、参考文档 |

### 设计原则

- **L1 保持简洁**：description 是 Claude 决定何时激活的关键
- **L2 聚焦清晰**：明确的边界，什么时候用，什么时候不用
- **L3 按需加载**：避免一次性加载所有资源

---

## 三、Skill 目录结构

```
<skill-id>/
├── skill.yaml               # 必需：技能定义
├── SKILL.md                 # 必需：Agent 文档
├── scripts/                 # 必需：实现代码
│   └── run.py
├── adapters/                # 适配器
│   ├── tool/tool.json       # OpenAI/LangChain
│   ├── prompt/SKILL.md      # Copilot
│   └── workflow/graph.yaml  # Dify/n8n
└── tests/                   # 测试
```

---

## 四、skill.yaml 格式

```yaml
id: <唯一标识>
name: <显示名称>
version: <语义版本>
status: active | deprecated
summary: <简短描述>

inputs:
  - name: <输入名>
    type: <类型>
    required: true | false
    description: <描述>

outputs:
  - name: <输出名>
    type: <类型>
    description: <描述>

permissions:
  network: true | false
  connectors:
    - <连接器>

compatibility:
  agents:
    - name: <agent名>
      mode: prompt | tool | workflow | mcp
      entry: <入口路径>
```

---

## 五、SKILL.md 格式

### 5.1 Frontmatter 格式

```yaml
---
name: <技能名>              # 必须：小写、连字符
description: <描述>         # 关键：包含触发词
version: "1.0"              # 可选
author: "<作者>"            # 可选
license: "Apache-2.0"      # 可选
---
```

### 5.2 Description 编写模板

```
[做什么]。[何时使用]。[关键词/触发词]。
```

**优秀示例**：

```
从发票 PDF 或图片中提取结构化信息。当用户说"提取发票"、"报销扫描"时触发。
Triggers: 发票提取、报销扫描、发票识别
```

### 5.3 常见错误

| 错误 | 问题 | 修正 |
|------|------|------|
| "helps with coding" | 太模糊 | 具体说明做什么 |
| 范围过大 | 一个 Skill 做所有事 | 拆分成多个专注 Skill |
| 缺少激活上下文 | Claude 无法判断何时触发 | 列出具体触发词 |

### 5.4 SKILL.md 正文结构

```markdown
# <技能名>

<详细描述>

## 使用场景
- <场景1>
- <场景2>

## 不适用场景
- <不适用情况1>
- <不适用情况2>

## 使用方法

```<语言>
<代码示例>
```

## 注意事项
- <注意点1>
- <注意点2>
```

---

## 六、发布流程

### 6.1 开发流程

1. 在 `skills/<skill-id>/` 创建或修改文件
2. 更新 `skill.yaml` 版本号
3. 提交 PR

### 6.2 CI 校验

PR 触发 `ci.yml`：
- 结构校验（目录、必需文件）
- JSON Schema 校验
- 测试执行

### 6.3 发布流程

1. Codeowners 审核通过
2. 合并到 main
3. 创建 Git Tag (`v*.*.*`)
4. 触发 `release.yml`
5. 打包 artifact + manifest + checksums
6. 创建 GitHub Release
7. 更新 Skill Catalog

---

## 七、Adapter 类型

| 类型 | 用途 | 文件位置 |
|------|------|---------|
| prompt | 指令型 | `adapters/prompt/SKILL.md` |
| tool | 工具型 | `adapters/tool/tool.json` |
| workflow | 流程型 | `adapters/workflow/graph.yaml` |
| mcp | 协议型 | `adapters/mcp/*` |

---

## 八、编写检查清单

### SKILL.md

- [ ] frontmatter 格式正确（name 匹配目录名）
- [ ] description 清晰、具体、包含触发词
- [ ] 列出明确的使用场景
- [ ] 列出不适用场景（边界）
- [ ] 提供代码示例
- [ ] 说明注意事项和限制

### skill.yaml

- [ ] id 和 name 正确
- [ ] version 遵循语义化版本
- [ ] inputs/outputs 完整
- [ ] permissions 最小化
- [ ] compatibility 声明完整

---

## 九、最佳实践

- 每个 Skill 保持**单一职责**
- 版本号遵循**语义化版本**
- 权限默认**最小化**
- 提供**完整测试用例**
- SKILL.md 要**清晰描述使用场景**
- 使用 **Progressive Disclosure** 控制 Token 成本
