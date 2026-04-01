# Agent 配置指南（可直接操作）

目标：让你按步骤把 Skill 接到 Agent 侧并完成一次可验证调用。

## 1. 先做前置检查

1. 检查 `pack.yaml` 里有目标 skill：
```bash
sed -n '1,240p' <your-pack>/pack.yaml
```
必须有最小字段：`id/path/mode/entry`。

2. 检查 catalog 有可消费版本：
```bash
sed -n '1,260p' templates/phase1/catalog/index.json
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
- `mode=tool`：读取 `/opt/skills/plugins/<pack-id>/<version>/<entry>`
- `mode=mcp`：读取 `/opt/skills/plugins/<pack-id>/<version>/<entry>`

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

## 4. OpenAI Tool（tools[] 注入）

适用模式：`mode=tool`，入口一般是 `skills/<id>/adapters/tool/tool.json`。

### 4.1 配置
把 `tool.json` 原样读入并注入 Agent runtime 的 `tools[]`。

示例（Python）：
```python
import json

with open("skills/<id>/adapters/tool/tool.json", "r", encoding="utf-8") as f:
    tool_def = json.load(f)

tools = [{
    "type": "function",
    "name": tool_def["name"],
    "description": tool_def.get("description", ""),
    "parameters": tool_def["input_schema"]
}]
```

如果你走 plugin-first，建议直接从已解压目录读取：
```python
plugin_root = "/opt/skills/plugins/<pack-id>/<version>"
entry = "skills/<id>/adapters/tool/tool.json"
with open(f"{plugin_root}/{entry}", "r", encoding="utf-8") as f:
    tool_def = json.load(f)
```

### 4.2 验证
1. 发起一次最小调用，参数覆盖 `required` 字段。
2. 如果报 schema 错误，先对齐 `input_schema` 再重试。

### 4.3 回滚
1. 新版 `tool.json` 不兼容时，回切上一版 artifact。
2. 调用侧参数未升级前，不要切换到新 schema。

## 5. MCP 客户端（Claude/Gemini 等通用）

适用模式：`mode=mcp`，入口一般是 `skills/<id>/adapters/mcp/server.json`。

### 5.1 先确认
当前模板仓库默认没有完整 MCP server 样例。  
如果你没有 `server.json` 和启动命令，先补齐，再做客户端配置。

### 5.2 配置
在 MCP 客户端配置里登记 server（示例）：
```json
{
  "mcpServers": {
    "your-skill-server": {
      "command": "node",
      "args": ["/opt/skills/plugins/<pack-id>/<version>/skills/<id>/adapters/mcp/server.js"]
    }
  }
}
```

如果你的 `entry` 指向 `server.json`，则按该文件里的命令/参数映射到客户端配置。

### 5.3 验证
1. 客户端能发现工具列表。
2. 最小调用能返回结果。

### 5.4 回滚
1. 新版 server 异常，切回上一版 artifact。
2. 保留 `plugin_ref` 默认路径，`skill_ref` 仅可选。

## 6. 常见故障

| 现象 | 处理 |
|---|---|
| Agent 找不到 skill | 先看 catalog 是否有该 `skill_id` 与版本，再确认 plugin 是否已解压到目标目录 |
| 能找到 skill 但执行失败 | 查 `entry` 文件是否存在、是否与 `mode` 匹配 |
| 只有 `plugin_ref` 没 `skill_ref` | 正常，phase1 默认 plugin-first |
| Tool 参数总报错 | 严格按 `tool.json.input_schema` 传参 |
| MCP 无法发现工具 | 先检查 server 进程是否启动成功 |

## 7. `pack.yaml` 和 Agent 配置的关系

- `pack.yaml` 只定义“发布后可消费的入口元数据”（`id/path/mode/entry`）。
- Agent 真正运行时配置在各客户端（Copilot/OpenAI runtime/MCP client）完成。
