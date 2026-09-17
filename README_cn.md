<div align="center">
  <img src="logo/aweshelf.png" alt="aweshelf" width="860">
  <h1>aweshelf: AI Agents会话收藏管理器 <a href="https://github.com/wehuman01/aweskill"><img src="https://raw.githubusercontent.com/wehuman01/aweskill/main/logo/aweskill-badge2.svg" alt="aweskill companion"></a></h1>
  <p><strong>收藏、分类、恢复 AI 编程会话，支持 aweswitch 配置恢复。</strong></p>
  <p>轻量 CLI-first 工具，支持 Claude Code 和 Codex。</p>
  <p>
    <a href="./README.md">English</a> ·
    <strong>简体中文</strong> ·
    <a href="https://www.webioinfo.top/">Webioinfo</a>
  </p>
  <p>
    <a href="https://ko-fi.com/mugpeng"><img src="https://img.shields.io/badge/Ko--fi-Buy%20me%20a%20coffee-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white" alt="Ko-fi"></a>
  </p>
  <p>
    <img src="https://img.shields.io/badge/version-0.2.1-7C3AED?style=flat-square" alt="Version">
    <img src="https://img.shields.io/badge/python-%E2%89%A53.10-0EA5E9?style=flat-square" alt="Python">
  </p>
  <p>
    <img src="https://img.shields.io/badge/status-beta-c96a3d?style=flat-square" alt="Status">
    <img src="https://img.shields.io/badge/install-pip-22C55E?style=flat-square" alt="pip install">
    <img src="https://img.shields.io/badge/platform-terminal-334155?style=flat-square" alt="Platform">
    <img src="https://img.shields.io/pepy/dt/aweshelf?style=flat-square" alt="PyPI downloads">
    <img src="https://img.shields.io/github/stars/wehuman01/aweshelf?style=flat-square" alt="GitHub stars">
  </p>
</div>

## 快速开始

### 1. 安装和使用 aweshelf

如果你在 Claude Code、Codex、Cursor 等 coding agent 中工作，直接告诉它：

```text
Read https://github.com/wehuman01/aweshelf/blob/main/README.ai.md and follow it to install aweshelf for this agent.
```

Agent 会先安装 `aweshelf` CLI，然后在下面两种 skill 管理方式中选择一种：

1. **通过 [aweskill](https://aweskill.wehuman.top/)** — 从 GitHub 安装和管理 skill，支持更新、投影和备份。需要 Node.js。
2. **直接复制** — 将 `SKILL.md` 下载到 agent 的 skill 目录。除 Python 外无需额外依赖，但后续更新需要手动重新复制。

引导完成后就可以直接用了。收藏、搜索、整理都通过自然语言完成，上手可以先试试：

> “收藏当前会话。”

> “列出 backend 分类下的书签。”

> “搜索和 auth 相关的书签。”

Agent 通过 [SKILL.md](resources/skills/aweshelf/SKILL.md) 理解所有可用命令和工作流。

<details>
<summary>手动安装 — pip 与 skill 配置</summary>

从 PyPI 安装：

```bash
pip install aweshelf
```

然后把 skill 放到 agent 能找到的地方。通过 aweskill（需要 Node.js）：

```bash
aweskill install wehuman01/aweshelf
aweskill agent add skill aweshelf --global --agent <agent-id>   # <agent-id>：claude-code、codex、cursor 等
```

或者直接复制 — 把 `SKILL.md` 下载到 agent 的 skill 目录，目录对照表见 [README.ai.md](README.ai.md)。

</details>

### 2. 给 agent 装上管理能力

第 1 步的引导 prompt 通常会顺带装好 `aweshelf` skill — `README.ai.md` 把它作为独立的一步。如果你是手动装的 CLI，或者 skill 缺失，投影一次即可，agent 就能在本次和以后的会话里帮你管理书签：

<details>
<summary>等价的 CLI 命令</summary>

```bash
aweskill install wehuman01/aweshelf
aweskill agent supported                          # 找到当前 agent id（标 ✓ 的行）
aweskill agent add skill aweshelf --global --agent <agent-id>
aweskill agent list --global --agent <agent-id>   # aweshelf 显示为 linked 即成功
```

没有 aweskill（也不想装 Node.js）？把 `SKILL.md` 直接复制到 agent 的 skill 目录即可，步骤见 [README.ai.md](README.ai.md)。

</details>

### 3. 开始用自然语言管理书签

日常使用不需要记命令，直接描述意图就行（完整 CLI 参考见下方[命令](#命令)）：

#### 收藏与整理

可以直接对 agent 说：

```text
把当前会话收藏为「修复 auth 中间件 bug」，分类放到 backend。
```

<details>
<summary>等价的 CLI 命令</summary>

```bash
aweshelf bookmark -t “修复 auth 中间件 bug” -c backend   # 收藏当前会话
aweshelf edit aweshelf_0001 -t “新标题” -c frontend      # 之后改标题或分类
aweshelf rm aweshelf_0002                               # 删除书签
```

</details>

#### 查找会话

可以直接对 agent 说：

```text
搜索和 auth 相关的书签，并把最近的几条列给我。
```

<details>
<summary>等价的 CLI 命令</summary>

```bash
aweshelf list -c backend             # 列出某个分类下的书签
aweshelf search “auth”               # 按标题、分类、会话、项目、首条提示词、profile 搜索
aweshelf recent -n 10                # 最近的收藏
aweshelf show aweshelf_0001          # 查看某条书签详情
```

</details>

#### 恢复会话

想接着上次的会话继续？恢复是唯一一件 agent 不会替你做的事：`aweshelf resume` 会启动一个新的 agent 进程，和你正在对话的会话冲突。先退出当前 agent，然后在自己的终端里运行：

```bash
aweshelf resume aweshelf_0001                    # 用存储的 profile 恢复
aweshelf resume aweshelf_0001 --profile cc-glm   # 也可以指定别的 profile
```

#### 配合 aweswitch 自动收藏

如果你用 [aweswitch](https://github.com/wehuman01/aweswitch) 启动会话，可以做到启动即收藏 — 每条书签会记住启动时的 profile，`resume` 时恢复原始 provider（如 Claude Code 官方 API），也可以切换到其他已配置的 profile，比如 `cc-xiaomi`、`cc-glm`。在你的终端运行（aweswitch 会启动新的 agent 会话，agent 不会替你启动）：

```bash
aweswitch -c                    # 启动 + 自动收藏
aweswitch -c --profile cc-glm   # 指定配置启动 + 自动收藏
```

之后用相同配置恢复：

```bash
aweshelf resume aweshelf_0001   # 用存储的 aweswitch profile 恢复
```

<details>
<summary>安装 aweswitch</summary>

```bash
pip install aweswitch
```

不装 aweswitch 也不影响使用 — 只是恢复会话时不会自动切换 profile。

</details>

> **提示：** 更喜欢直接操作？`aweshelf browse` 打开 TUI，浏览、搜索、编辑、恢复书签都不用记命令 — 见[浏览模式 (TUI)](#浏览模式-tui)。

## 支持工具

aweshelf 由两个配套工具驱动：

- **[aweskill](https://github.com/wehuman01/aweskill)** — 面向 AI agent 的 CLI skill 包管理器。负责 skill 的安装、更新和投影，支持 47+ 编程 agent。
- **[aweswitch](https://github.com/wehuman01/aweswitch)** — Agent profile 切换器。用不同 API、token 和模型启动会话。aweshelf 在书签中存储 aweswitch profile，恢复会话时自动还原配置。

## 扩展

- **[aweshelf-extension/vscode](https://github.com/wehuman01/aweshelf-extension/tree/main/vscode)** — VS Code / Cursor 扩展，可在侧边栏浏览、搜索和恢复书签。在扩展市场搜索 **aweshelf-ext**，或 [打开 Marketplace 页面](https://marketplace.visualstudio.com/items?itemName=webioinfo.aweshelf-ext)。也可下载 [.vsix](https://github.com/wehuman01/aweshelf-extension/releases) 安装。

## 浏览模式 (TUI)

更喜欢直接操作而不是问 agent？`aweshelf browse` 打开交互式终端 UI，左侧为书签表格，右侧为详情面板 — 无需记忆命令，直接浏览、搜索、编辑和恢复书签：

```bash
aweshelf browse
```

浏览视图会按分类组织书签，右侧展示当前选中书签的详情。

![aweshelf 带分类分组的浏览视图](resources/image/example1.png)

按 `e` 可以在表格里直接编辑当前单元格，标题、分类和 profile 都可以在 TUI 内保存。

![aweshelf 内联编辑模式](resources/image/example2.png)

按 `/` 可以按标题、分类、会话、项目、首条提示词或 profile 过滤书签。

![aweshelf 搜索过滤](resources/image/example3.png)

也可以使用 VS Code / Cursor 插件，在侧边栏里浏览、搜索和恢复书签。在扩展市场搜索 **aweshelf-ext** 安装，或 [打开 Marketplace 页面](https://marketplace.visualstudio.com/items?itemName=webioinfo.aweshelf-ext)。

![aweshelf VS Code 侧边栏](resources/image/example4.png)

`aweshelf bookmark` 会标记已经收藏的会话，并可在确认后更新已有 bookmark。使用 `aweshelf bookmark --current` 可以确认并保存当前项目最近的会话，不打开会话选择列表。交互收藏时会提示填写标题、分类和 Claude aweswitch profile；未配置 aweswitch 时会跳过 profile 选择。使用 `--no-interactive` 可跳过所有提示——适用于 agent 和脚本场景，仅使用默认值或传入的参数创建书签。

| 按键 | 操作 |
|------|------|
| `Enter` | 恢复选中的会话（带确认） |
| `e` | 内联编辑当前单元格（标题、分类、配置） |
| `r` | 删除选中书签（带确认） |
| `y` / `n` | 确认 / 取消操作 |
| `c` | 切换分类分组 / 全部视图 |
| `s` | 循环排序方式（分类+ID / ID） |
| `/` | 过滤书签 |
| `Esc` | 清除过滤 / 取消 |
| `[` / `]` | 缩小 / 扩大侧边栏 |
| `?` | 显示快捷键帮助 |
| `q` | 退出 |

编辑模式：输入文字编辑当前单元格，`Delete` 清空当前单元格，`Tab`/`Right` 切换下一个字段，`Shift+Tab`/`Left` 切换上一个，`Up`/`Down` 切换行，`Enter` 保存，`Esc` 退出。

## 配置

书签存储在 `~/.config/aweshelf/bookmarks.json`。可通过 `AWESHELF_CONFIG` 环境变量覆盖。

```json
{
  "version": 1,
  "bookmarks": [
    {
      "id": "aweshelf_0001",
      "provider": "claude",
      "session_id": "550e8400-...",
      "title": "Fix auth middleware bug",
      "category": "backend",
      "project_path": "/Users/peng/Desktop/Project/my-app",
      "aweswitch_profile": "cc-glm",
      "bookmarked_at": "2026-05-20T14:00:00Z"
    }
  ]
}
```

## 命令

```bash
aweshelf bookmark [SESSION_ID] [-t TITLE] [-c CATEGORY] [--profile PROFILE] [--current] [--verbose] [--no-interactive]
aweshelf list [-c CATEGORY] [-p PROVIDER]
aweshelf search QUERY              # 搜索标题、分类、会话ID、项目路径、首条提示词、配置
aweshelf recent [-n COUNT]
aweshelf show BOOKMARK_ID [--json]
aweshelf edit BOOKMARK_ID [-t TITLE] [-c CATEGORY] [--profile PROFILE]
aweshelf rm BOOKMARK_ID [--force]
aweshelf resume BOOKMARK_ID [--profile PROFILE] [--raw] [--dry-run]
aweshelf browse
aweshelf self-update [--check]
aweshelf help [COMMAND]
```

## 自动更新

aweshelf 每次运行时会在后台检查 PyPI 是否有新版本。如果有更新，会在命令执行完毕后在 stderr 输出提醒。

手动更新：

```bash
aweshelf self-update
```

仅检查不更新：

```bash
aweshelf self-update --check
```

禁用后台检查：

```bash
export AWESHELF_NO_UPDATE_CHECK=1
```

## 赞助与支持

如果 aweshelf 帮到了你，欢迎支持一下：

- ⭐ 给项目点个 Star — 让更多人看到它。
- ☕ [Ko-fi](https://ko-fi.com/mugpeng) — 请我喝杯咖啡。
- 💬 微信 — 扫描下方收款码。

<p align="center">
  <img src="assets/images/wechat-pay.jpg" alt="微信收款码" width="240">
</p>

> aweshelf 是免费开源的，你的支持让它持续维护下去 — 谢谢。

## 开发

详见 [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) 了解开发环境搭建、架构、测试和代码风格。

```bash
python -m pytest tests/
```

## 已知风险

aweshelf 书签存储的是 `session_id` 引用——**不会**复制会话内容。
恢复书签要求原始会话文件仍然存在于磁盘上。

| 风险 | Claude Code | Codex CLI |
|------|------------|-----------|
| 自动清理 | 不活跃超过 30 天的文件在启动时删除 | 目前无自动清理（非文档化保证） |
| Worktree | 会话绑定 worktree 路径；worktree 删除后 resume 可能失败 | 同上 |
| `cleanupPeriodDays` | [已知 bug](https://github.com/anthropics/claude-code/issues/62272)：部分场景下设置被静默忽略 | 不适用 |

缓解方式：在 `~/.claude/settings.json` 中增大 `cleanupPeriodDays`（如 `365`），但需验证是否生效。
对于关键会话，建议手动备份 JSONL 文件。

这些风险的后续改进计划见 [docs/todo/session_retention_0529.md](docs/todo/session_retention_0529.md)。

## Awesome 软件生态

aweshelf 是一个不断壮大的 "awesome" 工具家族中的一员 — 围绕 AI 编程 agent 打造，local-first、可被 agent 直接操作。

### CLI 工具

- **[aweskill](https://aweskill.wehuman.top/)** — CLI 优先的技能包管理器，支持 47+ AI 编程 agent。
- **[aweswitch](https://github.com/wehuman01/aweswitch)** — Claude Code、Codex、OpenCode 的 agent 配置切换器。
- **[awerouter](https://github.com/wehuman01/awerouter)** — 智能路由器，用结构信号把请求分给 Flash 或 Pro 模型，减少不必要的模型开销。
- **[aweshelf](https://github.com/wehuman01/aweshelf)** — 收藏、分类、恢复 AI 编程会话，还能搭配 aweswitch 实现保存配置，一键启动。
- **[aweshare](https://github.com/wehuman01/aweshare)** — 通过自建 Hub 共享本地 Ollama/vLLM，或国产厂商 coding plan，或已授权的 OpenAI/Anthropic 帐号订阅，实现 token 的共享经济。
- **[awewarm](https://github.com/wehuman01/awewarm)** — 订阅窗口保持器，让 AI 编程套餐的窗口持续激活，无论是本地设置，还是通过远程连接的服务器。
- **[awescholar](https://github.com/wehuman01/awescholar)** — AI agent 可自主执行的科学文献发现与策展，搜索、标注、筛选和报告学术论文。

### 桌面应用

- **[awedot](https://awedot.wehuman.top/)** — 悬浮球驻留屏幕边缘，实时追踪当前 AI 会话；一键收藏、随时恢复，并可搭配 aweswitch 固定 agent 配置（比如用 GLM 模型启动）。

### Project Collections

- **[Awesome AI Meets Biology](https://github.com/Webioinfo01/Awesome-AI-Meets-Biology)** — AI 在生物学、生物信息学和生物医学研究中应用的精选综述。由 awescholar 驱动。
- **[Awesome AI Virtual Tumor](https://github.com/Webioinfo01/Awesome-AI-Virtual-Tumor)** — 面向虚拟肿瘤建模与仿真的前沿 AI 系统精选合集：静态模型、动态模型、agent、基准与综述。
