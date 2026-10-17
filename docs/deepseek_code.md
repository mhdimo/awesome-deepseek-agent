[English](./deepseek_code.md) | [简体中文](./deepseek_code.zh-CN.md) · [← Back](../README.md)

# Integrate with DeepSeek Code

DeepSeek Code is a terminal-native AI coding agent built on a native C++ engine (ai-sdk-cpp). It runs DeepSeek-V4 models with a 1M-token context window, adjustable reasoning effort, a mouse-driven Ink interface, built-in coding tools, Agent Skills, MCP servers, and first-party web search and fetch through the DeepSeek API.

- **GitHub:** <https://github.com/mhdimo/deepseek-code>
- **npm:** [`@mihaldmo/deepseek-code`](https://www.npmjs.com/package/@mihaldmo/deepseek-code)

#### 1. Install DeepSeek Code

DeepSeek Code runs on [Bun](https://bun.sh/).

```sh
# Install Bun (skip if already installed)
curl -fsSL https://bun.sh/install | bash

# Install DeepSeek Code
bun install -g @mihaldmo/deepseek-code

# Verify the installation
deepseek-code --version
```

`npm install -g @mihaldmo/deepseek-code` works too, but Bun must still be on `PATH` at runtime.

#### 2. Configure DeepSeek Code

Get an API key from the [DeepSeek Platform](https://platform.deepseek.com/api_keys), then either export it:

```sh
export DEEPSEEK_API_KEY="sk-..."
```

or create `.deepseek-code.json` in your project (or `~/.config/deepseek-code/config.json` for every project):

```json
{
  "provider": "deepseek",
  "model": "deepseek-v4-pro",
  "apiKey": "env:DEEPSEEK_API_KEY",
  "effort": "max"
}
```

**Configuration options:**

| Option | Description |
|--------|-------------|
| `model` | `deepseek-v4-pro` for frontier reasoning, or `deepseek-v4-flash` for the fast tier (the app's default `deepseek-flash` selects it) |
| `apiKey` | Your API key. `env:VAR_NAME` reads it from the environment |
| `baseURL` | Optional endpoint override, defaults to `https://api.deepseek.com/v1` |
| `effort` | Reasoning effort: `off`, `low`, `medium`, `high`, `xhigh`, or `max` |
| `maxSteps` | Maximum tool-call steps per turn (default 25) |
| `mcpServers` | MCP servers to connect, using the same schema as Claude Code |

DeepSeek V4 models support up to **1M tokens** of context. DeepSeek Code assumes that window for DeepSeek models, shows live usage in the status bar, and the native session compacts the conversation automatically as it fills.

#### 3. Enter a project directory and launch DeepSeek Code

```sh
cd /path/to/my-project
deepseek-code
```

On first launch, `/setup` or `/apikey` stores your key. Use `/model` to switch models and `/effort max` to turn reasoning up.

#### Everyday commands

| Command | Action |
|---------|--------|
| `/help` | List every command |
| `/model` | Switch model |
| `/effort` | Set reasoning effort |
| `/agent` | Switch agent: `code`, `plan`, or `review` |
| `/skills` | List available Agent Skills |
| `/compact` | Compact the conversation (also happens automatically) |
| `/rewind` | Restore files to an earlier message |
| `/sessions`, `/resume` | Browse and resume saved sessions |
| `!` | Shell mode: run a command and have the model answer its output |
| `deepseek-code update` | Update to the latest published version |
| `deepseek-code changelog` | Show the release notes |

#### Key shortcuts

| Key | Action |
|-----|--------|
| `Enter` | Send the prompt |
| `Shift+Enter` / `Ctrl+J` | Insert a newline |
| `Esc` | Interrupt the current turn |
| `Ctrl+O` | Expand thinking blocks / toggle transcript mode |
| `Tab` | Add a note to a permission prompt |
| Mouse | Hover highlights a row, click activates it; right-click pastes into the prompt |

#### Agent Skills

Skills are `SKILL.md` files discovered from:

- **Project-level:** `.claude/skills/<name>/SKILL.md`
- **User-level:** `~/.claude/skills/<name>/SKILL.md`
- **Bundled:** shipped with the app (code review, git commit, debugging)

Type `/skills` to list them; the model can invoke a skill by name.

#### Web search and fetch

`WebSearch` and `WebFetch` call DeepSeek's first-party Search and Fetch APIs, returning structured results and page content as Markdown. DuckDuckGo search and direct downloads remain as fallbacks when no API key is configured.
