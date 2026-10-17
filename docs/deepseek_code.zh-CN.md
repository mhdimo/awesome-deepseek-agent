[English](./deepseek_code.md) | [简体中文](./deepseek_code.zh-CN.md) · [← Back](../README.zh-CN.md)

# 集成 DeepSeek Code

DeepSeek Code 是一款基于原生 C++ 引擎（ai-sdk-cpp）的终端 AI 编程助手。它支持 DeepSeek-V4 系列模型、100 万 token 上下文、可调节的推理强度、鼠标交互的 Ink 界面，内置编程工具、Agent Skills、MCP 服务器，以及通过 DeepSeek API 提供的原生网页搜索与抓取能力。

- **GitHub：** <https://github.com/mhdimo/deepseek-code>
- **npm：** [`@mihaldmo/deepseek-code`](https://www.npmjs.com/package/@mihaldmo/deepseek-code)

#### 1. 安装 DeepSeek Code

DeepSeek Code 运行于 [Bun](https://bun.sh/)。

```sh
# 安装 Bun（已安装可跳过）
curl -fsSL https://bun.sh/install | bash

# 安装 DeepSeek Code
bun install -g @mihaldmo/deepseek-code

# 验证安装是否成功
deepseek-code --version
```

也可以使用 `npm install -g @mihaldmo/deepseek-code`，但运行时仍需确保 Bun 在 `PATH` 中。

#### 2. 配置 DeepSeek Code

从 [DeepSeek 开放平台](https://platform.deepseek.com/api_keys) 获取 API Key，然后选择导出环境变量：

```sh
export DEEPSEEK_API_KEY="sk-..."
```

或在项目目录创建 `.deepseek-code.json`（全局配置可放在 `~/.config/deepseek-code/config.json`）：

```json
{
  "provider": "deepseek",
  "model": "deepseek-v4-pro",
  "apiKey": "env:DEEPSEEK_API_KEY",
  "effort": "max"
}
```

**配置选项：**

| 选项 | 说明 |
|------|------|
| `model` | `deepseek-v4-pro`（旗舰推理）或 `deepseek-v4-flash`（快速档位；应用默认的 `deepseek-flash` 即选择它） |
| `apiKey` | API Key，使用 `env:变量名` 可从环境变量读取 |
| `baseURL` | 自定义 API 地址，默认为 `https://api.deepseek.com/v1` |
| `effort` | 推理强度：`off`、`low`、`medium`、`high`、`xhigh` 或 `max` |
| `maxSteps` | 每轮最大工具调用步数（默认 25） |
| `mcpServers` | 要连接的 MCP 服务器，配置格式与 Claude Code 相同 |

DeepSeek V4 模型支持最高 **100 万 token** 的上下文。DeepSeek Code 对 DeepSeek 模型默认按该窗口计算，在状态栏实时显示用量，并由原生会话在接近上限时自动压缩对话。

#### 3. 进入项目目录并启动 DeepSeek Code

```sh
cd /path/to/my-project
deepseek-code
```

首次启动时，使用 `/setup` 或 `/apikey` 保存 API Key。使用 `/model` 切换模型，使用 `/effort max` 提升推理强度。

#### 常用命令

| 命令 | 作用 |
|------|------|
| `/help` | 查看所有命令 |
| `/model` | 切换模型 |
| `/effort` | 设置推理强度 |
| `/agent` | 切换 Agent：`code`、`plan` 或 `review` |
| `/skills` | 列出可用的 Agent Skills |
| `/compact` | 压缩对话（也会自动进行） |
| `/rewind` | 将文件恢复到某条历史消息的状态 |
| `/sessions`、`/resume` | 浏览并恢复已保存的会话 |
| `!` | Shell 模式：执行一条命令，并由模型解读输出 |
| `deepseek-code update` | 更新到最新发布版本 |
| `deepseek-code changelog` | 查看版本更新日志 |

#### 快捷键

| 按键 | 作用 |
|------|------|
| `Enter` | 发送提示词 |
| `Shift+Enter` / `Ctrl+J` | 插入换行 |
| `Esc` | 中断当前回合 |
| `Ctrl+O` | 展开思考块 / 切换完整对话模式 |
| `Tab` | 在权限确认框中附加备注 |
| 鼠标 | 悬停高亮行，点击激活；右键将内容粘贴到输入框 |

#### Agent Skills

Skills 是 `SKILL.md` 文件，可从以下位置发现：

- **项目级：** `.claude/skills/<name>/SKILL.md`
- **用户级：** `~/.claude/skills/<name>/SKILL.md`
- **内置：** 随应用发布（代码审查、Git 提交、调试）

输入 `/skills` 可查看列表；模型也可以按名称调用 Skill。

#### 网页搜索与抓取

`WebSearch` 和 `WebFetch` 调用 DeepSeek 原生的 Search 与 Fetch API，返回结构化结果和 Markdown 格式的网页内容。未配置 API Key 时，仍会回退到 DuckDuckGo 搜索和直接下载。
