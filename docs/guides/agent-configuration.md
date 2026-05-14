# Agent 配置指南（可直接操作）

目标：让你按步骤把 Skill 接到 Agent 侧并完成一次可验证调用。

## 1. 先做前置检查

1. 检查 `pack.yaml` 里有目标 skill：
```bash
cat <your-pack>/pack.yaml
```
必须有最小字段：`id/path/mode/entry`。

2. 检查 catalog 有可消费版本：
```bash
cat templates/phase1/catalog/index.json
```
默认必须有 `plugin_ref`。`skill_ref` 可选。

3. 检查入口文件存在：
```bash
ls -l <your-pack>/<entry-path>
```

## 2. Plugin-first 配置（默认分发方式）

phase1 默认分发是 plugin artifact。Agent 侧应先消费 plugin 包，再按 `mode/entry` 选入口。

### 2.1 本地落盘约定（建议）
1. 下载/获取 catalog 里的 `plugin_ref` 对应压缩包。
2. 解压到固定版本目录，例如：
   `/opt/skills/plugins/<pack-id>/<version>/`
3. 保持目录内相对路径不变（`skills/...`、`pack.yaml`）。

示例：
```bash
mkdir -p /opt/skills/plugins/<pack-id>/<version>
unzip <plugin-zip> -d /opt/skills/plugins/<pack-id>/<version>
```

### 2.2 最小校验
```bash
ls -l /opt/skills/plugins/<pack-id>/<version>/pack.yaml
ls -l /opt/skills/plugins/<pack-id>/<version>/skills
```

### 2.3 入口定位规则
- `mode=prompt`：读取 `/opt/skills/plugins/<pack-id>/<version>/<entry>`

`<entry>` 就是 `pack.yaml` 中该 skill 的 `entry` 字段。

## 3. Copilot（VS Code Agent Plugins）

适用模式：`mode=prompt`，入口一般是 `skills/<id>/SKILL.md`。

### 3.1 配置
在 VS Code `settings.json` 增加（或确认）：
```json
{
  "chat.plugins.enabled": true,
  "chat.pluginLocations": {
    "/absolute/path/to/your-plugin-or-pack": true
  }
}
```

如果你是按 plugin artifact 分发，先解压到本地目录，再把该目录加入 `chat.pluginLocations`。

### 3.2 验证
1. 在 Copilot Chat 触发一次技能请求。
2. 观察是否命中 `SKILL.md` 指令行为（而不是通用回答）。

### 3.3 回滚
1. 配置失效或效果变差时，切回上一版本 artifact 目录。
2. 保持 `plugin_ref` 路径，不依赖 `skill_ref`。

## 4. 常见故障

| 现象 | 处理 |
|---|---|
| Agent 找不到 skill | 先看 catalog 是否有该 `skill_id` 与版本，再确认 plugin 是否已解压到目标目录 |
| 能找到 skill 但执行失败 | 查 `entry` 文件是否存在、是否与 `mode` 匹配 |
| 只有 `plugin_ref` 没 `skill_ref` | 正常，phase1 默认 plugin-first |

## 5. `pack.yaml` 和 Agent 配置的关系

- `pack.yaml` 只定义”发布后可消费的入口元数据”（`id/path/mode/entry`）。
- Agent 真正运行时配置在各客户端（Copilot 等）完成。
