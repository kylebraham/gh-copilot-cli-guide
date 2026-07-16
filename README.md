# GitHub Copilot CLI Guide

Welcome to the comprehensive guide for GitHub Copilot CLI! This guide will help you understand and master the GitHub Copilot CLI, bringing AI-powered coding assistance directly to your terminal.

## 📚 Guide Structure

> **In a hurry?** Jump straight to the **[⚡ Cheat Sheet](00-cheat-sheet.md)** for a one-page quick reference of every command, shortcut, and flag.

### Foundation
1. **[Getting Started](01-getting-started.md)** - Installation, authentication, and first steps
2. **[Basic Concepts](02-basic-concepts.md)** - Understanding how the CLI works

### Core Features
3. **[Interactive Features](03-interactive-features.md)** - Working with the interactive interface
4. **[Slash Commands](04-slash-commands.md)** - Complete reference of all commands
5. **[File and Context Management](05-file-context.md)** - Managing files and context
6. **[Code Editing and Development](06-code-editing.md)** - Writing and editing code
7. **[GitHub Integration](07-github-integration.md)** - Working with repos, PRs, and issues

### Advanced Features
8. **[Advanced Features](08-advanced-features.md)** - MCP servers, custom agents, and more
9. **[Plan Mode](09-plan-mode.md)** - Creating implementation plans
17. **[Autopilot Mode](17-autopilot-mode.md)** - Complete guide to autonomous task execution
18. **[Fleet Mode](18-fleet-mode.md)** - Running tasks in parallel with subagents
19. **[Research Command](19-research-command.md)** - Deep investigation reports with `/research`

### Reference
10. **[Best Practices](10-best-practices.md)** - Tips, security, and effective use patterns
11. **[Troubleshooting](11-troubleshooting.md)** - Common issues and solutions
12. **[Examples and Tutorials](12-examples.md)** - Real-world scenarios and end-to-end walkthroughs

### Configuration
13. **[AGENTS.md File Guide](13-agents-file.md)** - Configuring AI agent behavior for your project
14. **[Skills System Guide](14-skills-system.md)** - Using and creating reusable skill modules
15. **[.copilot Directory Guide](15-copilot-directory.md)** - Understanding config, sessions, and data storage

### Teams and Automation
20. **[CI/CD and Automation](20-cicd-automation.md)** - Non-interactive mode, GitHub Actions, scripting
21. **[Team Setup](21-team-setup.md)** - Shared AGENTS.md, onboarding checklists, org configuration
22. **[Models and Costs](22-models-and-costs.md)** - Model comparison, cost optimization, team budget strategy

### What's New
16. **[Latest Features](16-new-features.md)** - Recent additions and experimental features

## 🎯 What is GitHub Copilot CLI?

GitHub Copilot CLI is a terminal-native AI coding assistant that brings the power of GitHub Copilot directly to your command line. It enables you to:

- **Build, edit, and debug code** through natural language conversations
- **Access GitHub resources** (repositories, issues, PRs) seamlessly
- **Execute complex tasks** with AI-powered planning and execution
- **Extend capabilities** with MCP (Model Context Protocol) servers
- **Maintain full control** with preview-before-execution

## 🚀 Quick Start

```bash
# Install with Homebrew (macOS/Linux)
brew install copilot-cli

# Or with npm (all platforms)
npm install -g @github/copilot

# Launch the CLI
copilot
```

### 🔄 Upgrading

Already have Copilot CLI? Upgrade to get the latest features:

```bash
# Homebrew
brew upgrade copilot-cli

# npm
npm update -g @github/copilot

# Or use the built-in update command
> /update
```

**Latest features:**
- 🌳 `/worktree` and `/move` are now separate commands: `/worktree` creates a new worktree and leaves uncommitted changes behind, while `/move` carries them into the new worktree (v1.0.71)
- 🚧 Plan Mode now hard-blocks built-in tool calls that would modify the workspace — the agent can't edit files or run mutating shell commands while planning (v1.0.71)
- 🎙️ `/voice devices` lets you choose and persist the microphone used for voice mode (v1.0.71)
- 🏪 New `plugins marketplace` CLI subcommands to list, add, remove, browse, and update plugin marketplaces (v1.0.71)
- 🧩 Lowered the default maximum sub-agent nesting depth from 6 to 4 to curb runaway recursion; usage-based billing users can still raise `subagents.maxDepth` up to 128 (v1.0.71)
- 🗂️ `copilot skill list` (and its JSON output) now marks disabled skills (v1.0.71)
- 💾 GitHub MCP toolset/tool configuration (`githubMcpToolsets`, `githubMcpTools`, etc.) now persists via `settings.json` (v1.0.71)
- 🖥️ `/settings` dashboard adds Repo and Repo (local) scope tabs alongside the existing `--repo`/`--local` flags (v1.0.71)
- 🧠 Added support for the `gpt-5.6` model (v1.0.70)
- ✍️ New `/refine` command rewrites a rough prompt into a clear one before sending (v1.0.70)
- 🛡️ New `--sandbox` / `--no-sandbox` flags force the shell sandbox on/off for just the current session, without changing your saved setting — handy with `-p` (v1.0.70)
- ⚙️ `/settings` and `/model` accept `--repo` / `--local` to scope a change instead of updating the global default (v1.0.70)
- 🔒 Trusted repos can pin model, effort, and context tier and extend the URL/MCP/skill deny lists via `.github/copilot/settings.json` (v1.0.70)
- 🔌 Pin a plugin to an exact commit with the `sha` field in its source config (v1.0.70)
- 🗂️ `/mcp list` marks sandboxed servers, e.g. `connected (sandboxed)` (v1.0.70)
- ⌨️ `Ctrl+Y` now opens the plan file or research report from any mode (v1.0.70)
- 🗂️ `/mcp list` shows attached MCP servers and status; runs while the agent is working, and the `/mcp` manager can be opened mid-turn (v1.0.69)
- ✅ New auto allow-all mode auto-approves LLM-judged acceptable requests; enabling it now requires experimental mode (v1.0.69)
- 🔁 `stayInAutopilot` setting keeps the CLI in autopilot mode after a task completes (v1.0.69)
- 🌿 `/delegate` now targets your current branch by default instead of the repo's default branch; use `--base` to override (v1.0.69)
- 🧩 `/plugins` dashboard and plugin reload without restarting the session (v1.0.69)
- ⚡ Minimal reasoning effort available for `gemini-3.5-flash` (v1.0.69)
- 🧩 Added support for the `kimi-k2.7-code` model (v1.0.68)
- 🧠 Claude Sonnet 5 added as a supported model (v1.0.67)
- 🚧 Session limits must now be at least 30 AI credits (v1.0.67)
- 🛡️ Disabling the sandbox for the rest of the session now takes effect immediately, with no more re-prompts mid-turn (v1.0.67)
- ⚡ New Claude Opus 4.8 Fast model; Claude Opus 4.6 Fast is now deprecated (v1.0.66)
- 🔁 `/pr auto` now runs a self-paced loop that fixes one thing per run and paces around CI to drive a PR to green; `/pr automerge` keeps going until the PR is merged (v1.0.66)
- 📓 `/chronicle skills review` for reviewing proposed draft skill changes — accept, reject, or defer each one (v1.0.66)
- 🌲 Pass a task to `/worktree` (e.g. `/worktree fix the login redirect`) to name the branch and run it as the first prompt in the new worktree (v1.0.66)
- 🔗 `@`-style imports now expand in `AGENTS.md`, `CLAUDE.md`, and Copilot instruction files (v1.0.66)
- 🎚️ Configure subagent concurrency and depth limits from `/settings` (usage-based billing users) (v1.0.66)
- 💾 `/cd` now persists the working directory across session resumes and discovers custom agents in the new directory (v1.0.65)
- 🛠️ New `copilot skill` CLI subcommand + `/skill` alias for `/skills` to list, add, and remove skills (v1.0.65)
- 🚦 Opt-in CI check status bar item shows passing/running/failing CI status for the current branch (v1.0.65)
- ⌨️ Up/down history and Ctrl+R reverse search now include past shell commands in normal mode (v1.0.65)
- 🔒 `/security-review` is now available to all users — no `--experimental` flag required (v1.0.64)
- 🌿 `/branch` — new alias for `/fork` matching Claude Code's command naming (v1.0.64)
- 🔁 `/loop` — new alias for `/every` scheduled prompts (v1.0.64)
- 🩺 `/diagnose` — new command to analyze session logs (v1.0.64)
- 🤖 Autopilot mode now returns to interactive mode after the agent calls `task_complete` (v1.0.64)
- 🌐 HTTP(S) proxy can be set as a persistent user setting in addition to environment variables (v1.0.64)
- 🏷️ Model family aliases (`opus`, `sonnet`, `haiku`, `gpt`, `gemini`) for `/model` shorthand (v1.0.64)
- 🔍 Press `w` in `/diff` to hide whitespace-only changes (v1.0.63)
- 🔒 Auth validation errors (VPN / IP allowlist failures) now appear in the sign-in banner with network guidance (v1.0.63)
- 🔀 Fork-based pull requests now shown in `/pr` and the branch PR badge (v1.0.63)
- 🔧 `deferTools` option for MCP server config keeps a server's tools always available even when tool search is enabled (v1.0.63)
- 📱 `/app` slash command — open the GitHub app or a browser fallback (v1.0.62)
- 🤖 `/subagents` (alias `/agents`) picker — configure subagent model, reasoning effort, and context tier (v1.0.62)
- 🌿 `/worktree` (alias `/move` until v1.0.71 — [now separate commands](04-slash-commands.md#worktree-branch-and-move-branch-v1061)) — create a new git worktree and switch into it, carrying uncommitted changes (v1.0.61)
- 🤖 Claude Fable 5 model added (v1.0.61)
- 📂 Auto-load MCP servers from `.github/mcp.json` workspace config file (v1.0.61)
- 🗓️ `/every` and `/after` now accept natural language scheduling (cron, calendar times, relative durations) (v1.0.61)
- 🔔 `beepOnSchedule` setting disables completion beeps for `/every` and `/after` runs (v1.0.61)
- 🗂️ `tabs` setting configures home tab bar visibility, order, and hidden tabs (v1.0.61)
- 🧠 Max reasoning effort level for Anthropic models; all effort levels now available on every plan (v1.0.60)
- 🗂️ `/context` separates Custom Instructions from the system prompt and cross-references per-server MCP tool token costs with `/mcp` (v1.0.60)
- 💳 New `billing` help topic — overview of AI credit usage features (v1.0.60)
- 🪿 `builtInAgents.rubberDuckAutoInvoke` setting controls automatic rubber duck agent invocation (v1.0.60)
- 🎙️ `/voice` command — dictate prompts using local speech-to-text models (v1.0.59)
- 🦆 Rubber Duck agent is now enabled by default — no need to enable experimental mode (v1.0.58)
- ⏰ `/experimental` scheduled prompts with `/every` and `/after` (v1.0.58)
- 💡 `showTipsOnStartup` setting controls whether startup tips are shown (v1.0.57)
- 📡 Default networking is now HTTP/1.1 for improved reliability; opt into HTTP/2 with `COPILOT_ENABLE_HTTP2=1` (v1.0.57)
- 🆓 Free and Student users can now select any model in the picker (v1.0.56)
- 🦆 `builtInAgents.rubberDuck` setting to enable/disable rubber-duck agent via config (v1.0.56)
- 🔧 GitHub MCP server omits redundant `gh`-replaceable tools when `gh` CLI is on PATH, reducing token usage (v1.0.56)
- 📋 MCP tools with both `content` and `structuredContent` now surface both payloads to the agent (v1.0.56)
- 📜 Diff view uses continuous scroll layout with sticky headers and theme-aware colors (v1.0.56)
- 🔍 `/review` code review agent now uses the current session's model (v1.0.56)
- 🔐 `permissions.disableBypassPermissionsMode` setting prevents enabling allow-all/yolo mode (v1.0.55)
- 📊 Per-MCP-server token usage shown in `/mcp`; MCP tool tokens broken out in `/context` (v1.0.55)
- 🖊️ Multiline prompts display fully without content clipping or selection offset (v1.0.53)
- 🗂️ `/skills` picker now correctly honors `--config-dir` when saving skill preferences (v1.0.53)
- 🐚 Bash shell sessions no longer hang when `PS0` or `PROMPT_COMMAND` is set (v1.0.53)
- ✂️ `/compact` accepts optional focus instructions to shape the compaction summary (v1.0.52)
- 📊 `/usage` shows quota progress bars for session and weekly limits (v1.0.52)
- 🔧 Custom agents support `deferred-tool-loading` frontmatter for on-demand tool discovery (v1.0.52)
- 📁 Sessions resume in their saved working directory; pass `-C <dir>` to override (v1.0.52)
- 🔄 `/restart` and `/update` preserve the current session ID after restarting (v1.0.52)
- 🤖 General-purpose subagents use GPT-5.4 or GPT-5.5 when available (v1.0.52)
- 🗑️ Old process log files in `~/.copilot/logs/` are automatically pruned at startup (v1.0.52)
- 🔒 `/security-review` — dedicated security-focused code review (v1.0.51; GA in v1.0.64)
- 🆔 `--session-id=<id>` — resume or start sessions with a specific UUID (v1.0.51)
- 🪝 `preMcpToolCall` hook — control outgoing MCP request metadata (v1.0.51)
- 💡 `/chronicle cost-tips` — personalized token cost recommendations (v1.0.51)
- 🔐 Secret scanning for commit messages and PR descriptions (v1.0.51)
- 🧠 `/memory on|off|show` — persistent cross-session memory with user and repository scopes (v1.0.49)
- 🦆 `/rubber-duck` — independent critique of the agent's current work; enabled by default in v1.0.58+ (v1.0.49)
- 🔍 `/chronicle search <query>` — search all session content by keyword or topic (v1.0.49)
- 🆔 `/session id` — display and copy the current session ID (v1.0.49)
- 🖨️ `/exit print` — print session transcript to terminal before exiting (v1.0.49)
- 🔌 `/mcp search <query>` — discover and install MCP servers from the registry (experimental, v1.0.49)
- 🔄 `/plugin update --all` — update all installed plugins in one command (v1.0.49)
- 🔗 Auto-link GitHub issue and PR references (`owner/repo#number`) in responses (v1.0.49)
- 🐧 Alpine Linux (musl libc) support (v1.0.49)
- 💰 Model picker displays actual token prices for token-based billing users (v1.0.48)
- 📐 Instruction files with unquoted glob patterns in `applyTo` frontmatter now apply correctly (v1.0.48)
- 📏 `/context` shows correct token limits for all models, not just 128k (v1.0.48)
- 🔕 `/ask` dialog no longer shows a follow-up prompt it cannot receive (v1.0.48)
- ✂️ Skill YAML frontmatter metadata stripped before injection — only instructions reach the model (v1.0.48)
- 🍴 `/fork` accepts an optional name; forked sessions show their origin in the sessions dialog (v1.0.47)
- 💎 Copilot Max subscribers now see the correct models available to their subscription tier (v1.0.47)
- ⌨️ j/k keys supported for up/down navigation in the `/diff` view (v1.0.47)
- ☁️ `--resume` supports Copilot cloud agent sessions before the agent has pushed any branch changes (v1.0.47)
- ⚠️ Warning displayed when the CLI version is deprecated and premium model access may be affected (v1.0.46)
- ✅ Read-only `gh` CLI commands (list, view, status, diff, etc.) are auto-approved without confirmation prompts (v1.0.46)
- 📏 Long lines in diff view now wrap at terminal width instead of being truncated (v1.0.46)
- 🤖 `/autopilot` slash command toggles between interactive and autopilot modes directly (v1.0.45)
- 🍴 `/fork` command forks the current session into a new independent session (v1.0.45)
- 📡 OpenTelemetry output now aligns with GenAI semantic conventions; MCP tool calls emit standard `tool_call` spans (v1.0.45)
- ⚡ CLI starts up to ~1.5s faster on terminals with limited OSC color query support (v1.0.45)
- ⌨️ Slash commands can now appear mid-input, and multiple skills can be invoked in a single message (v1.0.44)
- 🪝 `userPromptSubmitted` hooks can now handle requests directly, bypassing the LLM entirely (v1.0.44)
- 📦 `/update prerelease` fetches the latest prerelease build (v1.0.44)
- 🔒 Security fix: protection against RCE from malicious bare repositories nested inside a project (v1.0.43)
- 👤 `/statusline` now has a `username` toggle to display the active account in the footer (v1.0.43)
- 🤖 Auto mode uses server-side model routing for improved real-time model selection (v1.0.43)
- 📦 MCP server child processes (npx, uvx, etc.) are fully terminated when a session ends (v1.0.43)
- 📁 New `-C <directory>` flag to change working directory before starting, like `git -C` (v1.0.42)
- 🦆 Rubber-duck agent for GPT sessions powered by Claude, available in `/experimental` (v1.0.42)
- ⚡ CLI starts faster — UI renders immediately while authentication resolves in the background (v1.0.41)
- 🔧 Shell completions (bash, zsh, fish) install automatically on first run and refresh after `copilot update` (v1.0.41)
- 📎 `--attachment` flag in non-interactive (`-p`) mode lets you attach images or documents to the initial prompt (v1.0.41)
- 📂 `COPILOT_HOME` env var replaces deprecated `--config-dir` flag for non-default config directories (v1.0.40)
- 📖 `/chronicle` command and session history/file tracking now available to all users (v1.0.40)
- 🔬 `/research` uses an orchestrator/subagent model for more thorough deep research results (v1.0.40)
- ⌨️ Press `Ctrl+X → B` to move the current running task or shell command to the background (v1.0.39)
- 💡 `/remote` status output now shows actionable hints for each connection state (v1.0.39)
- 📋 `--resume` session picker has improved tab layout, status display, and progressive loading (v1.0.39)
- ⚡ Slash command argument picker opens immediately at exact command boundaries — no trailing space needed (v1.0.39)
- 📁 Location-based permission persistence enabled by default — approvals carry over across sessions for the same directory (v1.0.37)
- 🔤 `copilot completion <bash|zsh|fish>` generates static shell completion scripts (v1.0.37)
- 🔃 Press `s` in the session picker to cycle sort order: relevance, last used, created, or name (v1.0.37)
- 📝 `/ask` responses now render full markdown including tables and formatted links (v1.0.37)
- ❯ Subcommand picker shows a selection indicator (❯) next to the highlighted item (v1.0.36)
- 🔒 Double `Esc` required to cancel in-flight work, preventing accidental interruptions (v1.0.36)
- 💤 `/keep-alive` no longer requires experimental mode — prevents system sleep while Copilot CLI is active (v1.0.36)
- 📡 `/remote` shows current status; `/remote on` and `/remote off` toggle remote control (v1.0.36)
- 📊 `changes` statusline toggle shows added/removed line counts for the session (v1.0.36)
- 🩹 `preToolUse` hooks with `matcher` now run only for tool names that fully match the regex (v1.0.36 fix)
- 📂 Session picker shows branch names, idle/in-use status, and improved search (v1.0.35)
- 🏷️ `--name` flag names sessions at startup; `--resume=<name>` resumes by name (v1.0.35)
- 🗑️ `/session delete`, `/session delete <id>`, `/session delete-all`, and `x`-to-delete in the session picker (v1.0.35)
- 🌐 `COPILOT_GH_HOST` env var sets GitHub hostname, taking precedence over `GH_HOST` (v1.0.35)
- ⚙️ User settings now in `~/.copilot/settings.json`; internal state stays in `config.json` (v1.0.35)
- 🔁 `--resume` / `--continue` auto-inherits `--remote` flag (v1.0.33)
- 🔤 New slash command aliases: `/upgrade`, `/bug`, `/continue`, `/release-notes`, `/export`, `/reset` (v1.0.33)
- 💡 Slash command picker suggests similar commands for typos (v1.0.33)
- ⌨️ `j`/`k`/`x` vim-style navigation and task-cancel in `/tasks` dialog (v1.0.33)
- ⚠️ Usage warnings at 50% and 95% of weekly limit (v1.0.33)
- 🤖 `auto` model — let Copilot automatically pick the best available model for each session
- 📎 Attach supported document files to prompts for the agent to read and reason about
- 🔗 `--connect` flag — directly connect to a remote session by ID
- 🔍 `--print-debug-info` — display version, terminal capabilities, and environment variables
- ⏱️ `--session-idle-timeout` — configurable session idle timeout (disabled by default)
- 🔢 Short session ID prefixes (7+ hex chars) work with `--resume` and `/resume`
- 📊 `/statusline` (alias `/footer`) — customize which items appear in the status bar (directory, branch, effort, context, quota)
- 🧠 Claude Opus 4.7 model support added
- 🔑 `COPILOT_AGENT_SESSION_ID` env var passed to shell commands and MCP servers
- 🌐 Remote MCP server config: `type` field now optional (defaults to `http`)
- 🔗 `--resume` picker now includes remote control sessions
- 🔇 `COPILOT_DISABLE_TERMINAL_TITLE` — opt out of terminal title updates
- 🔀 Rewind picker navigation simplified to arrow keys + Enter
- 🔧 MCP migration hint links to platform-specific docs instead of embedding shell commands
- ❓ `/ask` — ask a quick question without affecting conversation history
- 📋 `/env` — show all loaded environment details (instructions, MCP servers, skills, agents, plugins) at a glance
- 🔌 Install MCP servers from the registry with guided configuration directly in the CLI
- 🖥️ Remote control your CLI sessions using `--remote` or `/remote`
- ⌨️ `Alt+D` deletes the word in front of the cursor in text input
- 🪝 `preToolUse` hooks: `modifiedArgs`/`updatedInput` rewrite tool inputs; `additionalContext` enriches model results
- 🏷️ Custom agent `model` field accepts VS Code display names (e.g., `"Claude Sonnet 4.5"`)
- 🚀 `--mode`, `--autopilot`, `--plan` flags — start the CLI directly in a specific agent mode
- ⌨️ `Ctrl+L` clears screen while preserving the conversation session
- 🗂️ `.mcp.json` only — MCP config consolidated to one file; `.vscode/mcp.json` and `.devcontainer/devcontainer.json` no longer read
- 🤖 Custom agents: `skills` field for eager skill loading at startup
- 🔌 `copilot mcp` — manage MCP servers from the shell without starting a session
- 🔄 `/rewind` / `/undo` — undo the last turn and revert file changes
- 🧠 `/context` — visualize context window token usage
- 🗜️ `/compact` — summarize conversation history to save tokens
- ✨ New models: Claude Sonnet 4.6, Claude Opus 4.6, GPT-5.4 mini, GPT-4.1
- ⌨️ Updated keyboard shortcuts (`Ctrl+S`, `Ctrl+T`, `Ctrl+O`, `Ctrl+E`)
- 🎙️ `/streamer-mode` — hide model/quota details while screen-sharing
- 🤖 [Autopilot mode](17-autopilot-mode.md) (experimental) - fully autonomous task execution
- 🌐 [Fleet mode](18-fleet-mode.md) - run subtasks in parallel with subagents
- 🔍 LSP support - deep language-aware code intelligence
- 🔬 [Research command](19-research-command.md) - investigate codebases and summarise findings
- ⚡ [Cheat sheet](00-cheat-sheet.md) - one-page quick reference
- [See full feature details →](16-new-features.md)

## 💡 What You'll Learn

By the end of this guide, you'll be able to:

- Set up and authenticate GitHub Copilot CLI
- Navigate the interactive interface efficiently
- Use natural language to write and modify code
- Integrate with GitHub workflows
- Create implementation plans for complex tasks
- Customize and extend the CLI with MCP servers
- Apply best practices for productive AI-assisted development

## 🎓 Prerequisites

To get the most out of this guide, you should have:

- Basic familiarity with command-line interfaces
- An active GitHub Copilot subscription
- Basic programming knowledge (any language)
- **Git installed on your system** (required for Git features)
- **GitHub CLI (gh) installed** (required for GitHub integration)

## 📖 How to Use This Guide

- **New to GitHub Copilot CLI?** Start with [Getting Started](01-getting-started.md) and work through each section sequentially.
- **Looking for specific features?** Jump to the relevant section using the links above.
- **Want hands-on practice?** Each section includes runnable examples you can try immediately.
- **Need quick reference?** Check the [⚡ Cheat Sheet](00-cheat-sheet.md) or [Slash Commands](04-slash-commands.md) reference.
- **Setting up for a team?** Go straight to [Team Setup](21-team-setup.md).

## 🆘 Getting Help

- **In the CLI:** Type `/help` to see available commands
- **Documentation:** Visit [GitHub Docs](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)
- **Feedback:** Use `/feedback` command to submit feedback
- **Issues:** Report bugs on [GitHub](https://github.com/github/copilot-cli)

## 🎬 Let's Begin!

Ready to start? Head over to [Getting Started](01-getting-started.md) to install and configure GitHub Copilot CLI.

## 🛠️ Skills in This Repo

This repo includes five [Copilot Skills](14-skills-system.md) in `.github/` — modular expertise packages you can activate in Copilot CLI to get specialized help. Copy any skill folder to `~/.copilot/skills/` or `.copilot/skills/` in your project, then activate with `/skills add <name>`.

| Skill | Folder | What it does |
|-------|--------|--------------|
| `update-repo` | `.github/skills/update-repo/SKILL.md` | Refresh all remote refs (`git remote update`) and fast-forward the branch you want to work on |
| `doc-maintenance` | `.github/skills/doc-maintenance/SKILL.md` | Keep this docs repo current with new CLI releases |
| `cli-expertise` | `.github/skills/cli-expertise/SKILL.md` | Deep CLI feature knowledge — slash commands, agents, MCP, and more |
| `copilot-cli-guide-quickstart` | `.github/skills/quickstart/SKILL.md` | Interactive tutorial through all docs (works best with `cli-expertise`) |
| `get-release-url` | `.github/skills/get-release-url/SKILL.md` | Resolve the canonical GitHub Copilot CLI release URL for a version tag |
| `git-commit` | `.github/skills/git-commit/SKILL.md` | Stage, commit, and push changes with conventional commit messages |

### 🚀 Quick Start with the Tutorial

The `quickstart` skill walks you through the entire guide interactively:

1. Copy the skill folders to your skills directory:
   ```bash
   cp -r .github/skills/quickstart ~/.copilot/skills/
   cp -r .github/skills/cli-expertise ~/.copilot/skills/
   ```
2. Activate both skills in Copilot CLI:
   ```
   > /skills add copilot-cli-guide-quickstart
   > /skills add cli-expertise
   ```
3. Say **"start tutorial"** to begin!

> 💡 The quickstart skill works best with `cli-expertise` active — it uses that skill's deep feature knowledge to answer your Q&A questions during the tutorial.

### Using Other Skills

```bash
# Sync repo with remote
cp -r .github/skills/update-repo ~/.copilot/skills/
> /skills add update-repo

# Documentation maintenance help
cp -r .github/skills/doc-maintenance ~/.copilot/skills/
> /skills add doc-maintenance
```

For more on the Skills system, see [Skills System Guide](14-skills-system.md).

## 🔧 Configuration & Customization

Learn how to customize and extend Copilot CLI:

### Configuration Files
- **[.copilot Directory](15-copilot-directory.md)** - Understanding `~/.copilot/` structure, config.json, and data storage
- **config.json** - Main configuration (model, theme, trusted folders, etc.)
- **mcp-config.json** - MCP server configuration

### Instruction Files
- **[Skills System](14-skills-system.md)** - Reusable expertise modules (Python expert, React patterns, etc.)
- **[AGENTS.md](13-agents-file.md)** - Project-specific AI agent instructions
- **.github/copilot-instructions.md** - Copilot CLI workflows  
- **CLAUDE.md / GEMINI.md** - Model-specific preferences

### Comparison

| Feature | Skills | AGENTS.md | config.json | Instructions |
|---------|--------|-----------|-------------|--------------|
| Scope | Domain expertise | Project config | CLI behavior | Coding style |
| Location | ~/.copilot/skills/ | Project root | ~/.copilot/ | .github/ |
| Reusable | ✅ Across projects | ❌ Project only | ✅ Global | ❌ Project only |
| Activatable | ✅ On/off | ❌ Always on | N/A | ❌ Always on |
| Examples | "Python expert" | Architecture | theme, model | Formatting |

---

## ⚙️ Automation

This repo includes a daily GitHub Actions workflow that uses the `doc-maintenance` skill to automatically keep the documentation current.

### How It Works

1. Runs every day at **9:00 AM UTC** (or manually via the Actions tab)
2. Installs Copilot CLI using [`mvkaran/setup-copilot-cli@v1`](https://github.com/mvkaran/setup-copilot-cli)
3. Loads the `doc-maintenance` skill
4. Runs Copilot non-interactively to check for new CLI releases and update relevant markdown files
5. If changes are detected → opens a **Pull Request** for review
6. If no changes → exits cleanly

### Required Secret

The workflow requires a Personal Access Token (PAT) with **Copilot Requests** permission stored as a repository secret:

| Secret name | Where to create |
|-------------|----------------|
| `COPILOT_TOKEN` | [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new) → Permissions → Copilot Requests |

> ℹ️ The default `GITHUB_TOKEN` does not have Copilot access — a PAT is required.

### Manual Trigger

Go to **Actions → Daily Doc Maintenance → Run workflow** to trigger it on demand.

---

**Note:** This guide covers GitHub Copilot CLI v1.0.71. Some capabilities may vary by version — run `/update` to stay current.
