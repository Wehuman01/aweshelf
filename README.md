<div align="center">
  <img src="logo/aweshelf.png" alt="aweshelf" width="860">
  <h1>aweshelf: AI Agents Session Bookmark Manager <a href="https://github.com/Webioinfo01/aweskill"><img src="https://raw.githubusercontent.com/Webioinfo01/aweskill/main/logo/aweskill-badge2.svg" alt="aweskill companion"></a></h1>
  <p><strong>Bookmark, categorize, and restore AI coding sessions with aweswitch profiles.</strong></p>
  <p>A lightweight CLI-first tool for Claude Code and Codex session management.</p>
  <p>
    <strong>English</strong> ·
    <a href="./README_cn.md">简体中文</a> ·
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
    <img src="https://img.shields.io/github/stars/Webioinfo01/aweshelf?style=flat-square" alt="GitHub stars">
  </p>
</div>

## Quick Start

### 1. Install and use aweshelf

If you are working inside Claude Code, Codex, Cursor, or another coding agent, tell it:

```text
Read https://github.com/Webioinfo01/aweshelf/blob/main/README.ai.md and follow it to install aweshelf for this agent.
```

The agent will first install the `aweshelf` CLI, then choose one of two skill management options:

1. **Via [aweskill](https://aweskill.webioinfo.top/)** — installs and manages the skill from GitHub with update, projection, and backup support. Requires Node.js.
2. **Direct copy** — downloads `SKILL.md` into the agent's skill directory. No extra dependencies beyond Python, but future updates require copying the file again manually.

Once the bootstrap finishes, aweshelf is ready to use. Bookmarks, search, and edits all work through natural language — start with things like:

> "Bookmark the current session."

> "List my bookmarks in the backend category."

> "Search for bookmarks related to auth."

The agent reads [SKILL.md](resources/skills/aweshelf/SKILL.md) to understand all available commands and workflows.

<details>
<summary>Manual install — pip and skill setup</summary>

Install from PyPI:

```bash
pip install aweshelf
```

Then put the skill where your agent can find it. Via aweskill (requires Node.js):

```bash
aweskill install Webioinfo01/aweshelf
aweskill agent add skill aweshelf --global --agent <agent-id>   # <agent-id>: claude-code, codex, cursor, ...
```

or by direct copy — download `SKILL.md` into your agent's skill directory; the directory table is in [README.ai.md](README.ai.md).

</details>

### 2. Equip your agent

The bootstrap prompt in step 1 usually installs the `aweshelf` skill along the way — `README.ai.md` installs it as its own step. If you installed the CLI by hand or the skill is missing, project it once and the agent can manage bookmarks in this and future sessions:

<details>
<summary>Equivalent CLI commands</summary>

```bash
aweskill install Webioinfo01/aweshelf
aweskill agent supported                          # find your agent id (lines marked ✓)
aweskill agent add skill aweshelf --global --agent <agent-id>
aweskill agent list --global --agent <agent-id>   # aweshelf shows as linked
```

No aweskill (and no Node.js)? Copy `SKILL.md` straight into the agent's skill directory — steps in [README.ai.md](README.ai.md).

</details>

### 3. Manage bookmarks through natural language

Day-to-day aweshelf needs no commands — describe what you want (the full CLI reference is in [Commands](#commands)):

#### Bookmark and organize

You can tell your agent:

```text
Bookmark the current session as "Fix auth middleware bug" in the backend category.
```

<details>
<summary>Equivalent CLI commands</summary>

```bash
aweshelf bookmark -t "Fix auth middleware bug" -c backend   # bookmark the current session
aweshelf edit aweshelf_0001 -t "New title" -c frontend      # retitle or recategorize later
aweshelf rm aweshelf_0002                                   # remove a bookmark
```

</details>

#### Find sessions

You can tell your agent:

```text
Search my bookmarks for anything related to auth and show me the most recent ones.
```

<details>
<summary>Equivalent CLI commands</summary>

```bash
aweshelf list -c backend             # list one category
aweshelf search "auth"               # search title, category, session, project, prompt, profile
aweshelf recent -n 10                # the latest bookmarks
aweshelf show aweshelf_0001          # one bookmark in detail
```

</details>

#### Resume a session

Want to continue where a past session left off? Resuming is the one thing the agent will not do for you: `aweshelf resume` launches a new agent process, which would conflict with the session you are talking to. Exit the agent first, then run it in your own terminal:

```bash
aweshelf resume aweshelf_0001                    # resume with the stored profile
aweshelf resume aweshelf_0001 --profile cc-glm   # or force a different profile
```

#### Auto-bookmark with aweswitch

If you launch sessions with [aweswitch](https://github.com/Webioinfo01/aweswitch), they can bookmark themselves — and each bookmark remembers the profile it launched with, so `resume` restores the original provider (e.g. Claude Code official API) or switches to another configured one like `cc-xiaomi` or `cc-glm`. Run in your own terminal (aweswitch starts a new agent session — the agent will not launch it for you):

```bash
aweswitch -c                    # launch + auto-bookmark
aweswitch -c --profile cc-glm   # launch with a profile + auto-bookmark
```

Later, restore with the same profile:

```bash
aweshelf resume aweshelf_0001   # restores with the stored aweswitch profile
```

<details>
<summary>Install aweswitch</summary>

```bash
pip install aweswitch
```

Without aweswitch, aweshelf works fine — profile restore on resume is just skipped.

</details>

> **Tip:** Prefer pointing instead of asking? `aweshelf browse` opens a TUI for browsing, searching, editing, and resuming bookmarks — see [Browse (TUI)](#browse-tui).

## Supported by

aweshelf is powered by two companion tools:

- **[aweskill](https://github.com/Webioinfo01/aweskill)** — CLI-first skill package manager for AI agents. Handles skill installation, updates, and projection across 47+ coding agents.
- **[aweswitch](https://github.com/mugpeng/aweswitch)** — Agent profile switcher. Lets you launch sessions with different API endpoints, tokens, and models. aweshelf stores aweswitch profiles in bookmarks so sessions restore with the right config.

aweswitch manages how you **launch** sessions; aweshelf manages how you **remember** them. Use `aweswitch -c` to auto-bookmark at launch, and `aweshelf resume` to restore with the same profile later.

## Extensions

- **[aweshelf-extension/vscode](https://github.com/mugpeng/aweshelf-extension/tree/main/vscode)** — VS Code / Cursor extension for browsing, searching, and resuming bookmarks from the sidebar. Search **aweshelf-ext** in the extension marketplace, or [open in Marketplace](https://marketplace.visualstudio.com/items?itemName=webioinfo.aweshelf-ext). Also available as [.vsix](https://github.com/mugpeng/aweshelf-extension/releases).

## Browse (TUI)

Prefer pointing at your bookmarks instead of asking the agent? `aweshelf browse` opens an interactive TUI with a sidebar table and detail pane — browse, search, edit, and resume bookmarks without memorizing commands:

```bash
aweshelf browse
```

The browse view keeps bookmarks grouped by category, with the selected bookmark's details on the right.

![aweshelf browse view with category groups](resources/image/example1.png)

Press `e` to edit the current cell in place. Title, category, and profile changes can be saved without leaving the TUI.

![aweshelf inline edit mode](resources/image/example2.png)

Press `/` to filter bookmarks by title, category, session, project, prompt, or profile.

![aweshelf search filter](resources/image/example3.png)

You can also use the VS Code / Cursor extension to browse, search, and resume bookmarks from the sidebar. Search **aweshelf-ext** in the extension marketplace, or [open in Marketplace](https://marketplace.visualstudio.com/items?itemName=webioinfo.aweshelf-ext).

![aweshelf VS Code sidebar](resources/image/example4.png)

`aweshelf bookmark` marks already-bookmarked sessions and can update them after confirmation. Use `aweshelf bookmark --current` to confirm and save the most recent session in the current project without opening the session picker. Interactive bookmarking prompts for title, category, and Claude aweswitch profile; profile selection is skipped when aweswitch is not configured. Use `--no-interactive` to skip all prompts — for agents and scripting, bookmarks are created with defaults or passed values only.

| Key | Action |
|-----|--------|
| `Enter` | Resume selected session (with confirmation) |
| `e` | Inline-edit the current cell (title, category, profile) |
| `r` | Remove selected bookmark (with confirmation) |
| `y` / `n` | Confirm / cancel action |
| `c` | Toggle between Category-grouped and All view |
| `s` | Cycle sort order (category+id / id) |
| `/` | Filter bookmarks |
| `Esc` | Clear filter / cancel |
| `[` / `]` | Shrink / grow sidebar |
| `?` | Show keyboard shortcuts |
| `q` | Quit |

In edit mode: type to edit the active cell, `Delete` to clear it, `Tab`/`Right` to next field, `Shift+Tab`/`Left` to previous, `Up`/`Down` to move rows, `Enter` to save, `Esc` to exit.

## Config

Bookmarks are stored at `~/.config/aweshelf/bookmarks.json`. Override with `AWESHELF_CONFIG` env var.

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

## Commands

```bash
aweshelf bookmark [SESSION_ID] [-t TITLE] [-c CATEGORY] [--profile PROFILE] [--current] [--verbose] [--no-interactive]
aweshelf list [-c CATEGORY] [-p PROVIDER]
aweshelf search QUERY              # search title, category, session, project, prompt, profile
aweshelf recent [-n COUNT]
aweshelf show BOOKMARK_ID [--json]
aweshelf edit BOOKMARK_ID [-t TITLE] [-c CATEGORY] [--profile PROFILE]
aweshelf rm BOOKMARK_ID [--force]
aweshelf resume BOOKMARK_ID [--profile PROFILE] [--raw] [--dry-run]
aweshelf browse
aweshelf self-update [--check]
aweshelf help [COMMAND]
```

## Self-Update

aweshelf checks PyPI for newer versions in the background on each run. If an update is available, a reminder is printed to stderr after the command finishes.

To update manually:

```bash
aweshelf self-update
```

To check without updating:

```bash
aweshelf self-update --check
```

To disable the background check:

```bash
export AWESHELF_NO_UPDATE_CHECK=1
```

## Support

If aweshelf saves you time, consider supporting it:

- ⭐ Star the repo — it helps others find it.
- ☕ [Ko-fi](https://ko-fi.com/mugpeng) — buy me a coffee.
- 💬 WeChat — scan the QR code below.

<p align="center">
  <img src="assets/images/wechat-pay.jpg" alt="WeChat Pay" width="240">
</p>

> aweshelf is free and open source. Sponsors keep it maintained — thank you.

## Development

See [docs/CONTRIBUTING.md](docs/CONTRIBUTING.md) for setup, architecture, testing, and code style.

```bash
python -m pytest tests/
```

## Known Risks

aweshelf bookmarks store a `session_id` reference — they do **not** copy session content.
Resuming a bookmark requires the original session file to still exist on disk.

| Risk | Claude Code | Codex CLI |
|------|------------|-----------|
| Auto cleanup | Files inactive for 30 days are deleted at startup | No auto cleanup today (not a documented guarantee) |
| Worktree | Sessions tied to worktree path; resume may fail if worktree is deleted | Same |
| `cleanupPeriodDays` | [Known bugs](https://github.com/anthropics/claude-code/issues/62272) where the setting is silently ignored | N/A |

Mitigation: extend `cleanupPeriodDays` in `~/.claude/settings.json` (e.g. `365`), but verify it takes effect.
For critical sessions, consider backing up the JSONL file manually.

These risks are tracked in [docs/todo/session_retention_0529.md](docs/todo/session_retention_0529.md).

## Awesome Ecosystem

aweshelf is part of a growing family of "awesome" tools — CLI-first, local-first, and operable by AI agents.

### CLI Tools

- **[aweskill](https://aweskill.webioinfo.top/)** — CLI-first skill package manager supporting 47+ AI coding agents.
- **[aweswitch](https://github.com/Webioinfo01/aweswitch)** — Agent profile switcher for Claude Code, Codex, and OpenCode.
- **[awerouter](https://github.com/mugpeng/awerouter)** — Smart router that splits requests between Flash and Pro models using structural signals, cutting unnecessary model spend.
- **[aweshelf](https://github.com/Webioinfo01/aweshelf)** — Bookmark, categorize, and restore AI coding sessions; pairs with aweswitch to save profiles and launch with one command.
- **[aweshare](https://github.com/wehuman01/aweshare)** — Share local Ollama/vLLM backends, domestic coding plans, or authorized OpenAI/Anthropic subscriptions through a self-hosted hub — a sharing economy for tokens.
- **[awewarm](https://github.com/wehuman01/awewarm)** — Subscription window warmer that keeps AI coding-plan windows active, for local setups and through a remote hub server.
- **[awescholar](https://github.com/Webioinfo01/awescholar)** — AI-agent-operable scientific literature discovery and curation.

### Desktop Apps

- **[awedot](https://awedot.wehuman.top/)** — A floating orb at your screen edge keeps track of the current AI session: bookmark it in one click, resume anytime, and pair with aweswitch to pin the agent's config (e.g., relaunch with the GLM model).

### Project Collections

- **[Awesome AI Meets Biology](https://github.com/Webioinfo01/Awesome-AI-Meets-Biology)** — A curated survey of AI applications in biology, bioinformatics, and biomedical research. Powered by awescholar.
- **[Awesome AI Virtual Tumor](https://github.com/Webioinfo01/Awesome-AI-Virtual-Tumor)** — A curated collection of state-of-the-art AI systems for virtual tumor modeling and simulation: static models, dynamic models, agents, benchmarks, and reviews.
