# GitHub Copilot CLI — Cheat Sheet

> This is a quick reference. For full documentation see the [Guide Index](README.md).

---

## Keyboard Shortcuts

### Navigation & Control

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current input / interrupt running task / copy selection |
| `Ctrl+C` × 2 | Exit Copilot CLI |
| `Ctrl+D` | Shutdown (does not queue a message) |
| `Ctrl+Q` / `Ctrl+Enter` | Queue message while agent is running |
| `Ctrl+L` | Clear the terminal screen (conversation session preserved) |
| `Esc` | Cancel input / close picker | Clear the current input line or close a picker; press **twice** (`Esc Esc`) to cancel an in-flight AI operation |
| `↑ / ↓` | Navigate command history |
| `Shift+Tab` | Cycle modes (interactive → plan) |
| `Ctrl+S` | Run command while preserving input |
| `Ctrl+T` | Toggle model reasoning display |
| `Ctrl+O` | Expand all timeline entries (when no input) |
| `Ctrl+E` | Expand all timeline entries (when no input) |
| `Ctrl+X → O` | Open link from most recent timeline event |
| `Ctrl+X → B` | Move current running task or shell command to the background (v1.0.39+) |
| `Tab` / `Ctrl+Y` | Accept highlighted completion option (`@`-mentions, paths, slash commands) |
| `!` | Execute command in local shell (bypass Copilot); uses `$SHELL` when set |
| `s` | Cycle session picker sort order (relevance → last used → created → name) — press while in the `/resume` picker |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line (when typing) |
| `Ctrl+W` | Delete previous word |
| `Alt+D` | Delete next word (word in front of cursor) |
| `Ctrl+U` | Delete from cursor to beginning of line |
| `Ctrl+K` | Delete from cursor to end of line (joins lines at end) |
| `Ctrl+H` | Delete previous character (backspace) |
| `Meta+← / →` | Move cursor by word |
| `Ctrl+G` | Edit prompt in external editor |

---

## Modes

| Mode | How to Enter | When to Use |
|------|-------------|-------------|
| **Interactive** | Default on launch | Conversational coding, exploration, Q&A |
| **Plan** | `Shift+Tab` (first press) | Review a proposed implementation plan before executing |
| **Autopilot** | `/autopilot` (v1.0.45+) or `Shift+Tab` (twice from interactive) | Let Copilot implement autonomously with minimal interruption |

---

## All Slash Commands

### Session & Context
| Command | Description |
|---------|-------------|
| `/clear` | Clear conversation history and context |
| `/reset` | Alias for `/clear` |
| `/compact` | Summarize and compress current context to save tokens |
| `/context` | Show what's currently in context |
| `/session` | Show current session info |
| `/session delete <id>` | Delete a specific session by ID or 7+ char prefix |
| `/session delete-all` | Delete all sessions |
| `/resume` | Resume a previous session (picker shows branch and idle/in-use status) |
| `/continue` | Alias for `/resume` |
| `/rename` | Rename the current session |
| `/share` | Share session as markdown, gist, or HTML (`/share html`) |
| `/export` | Alias for `/share` |
| `/copy` | Copy last response to clipboard |
| `/env` | Show loaded environment details (instructions, MCP servers, skills, agents, plugins) |
| `/ask` | Ask a quick question without affecting conversation history |
| `/fork` | Fork the current session into a new independent session; accepts optional name (v1.0.45+, named forks v1.0.47+) |

### Navigation
| Command | Description |
|---------|-------------|
| `/cwd` | Show current working directory |
| `/add-dir` | Add a directory to the active context |
| `/list-dirs` | List all directories currently in context |

### Code & GitHub
| Command | Description |
|---------|-------------|
| `/diff` | Show current git diff |
| `/review` | Request a code review of staged or recent changes |
| `/pr` | Create or manage a pull request |
| `/delegate` | Hand off a task to an autonomous subagent |

### AI & Models
| Command | Description |
|---------|-------------|
| `/model` | View or switch the active AI model |
| `/agent` | Configure or inspect the active agent |
| `/fleet` | Launch parallel subagents for distributed tasks |
| `/tasks` | View or manage running agent tasks (`j`/`k` to navigate, `x` to cancel) |
| `/research` | Run a deep research pass on a topic or codebase |
| `/plan` | Generate a step-by-step implementation plan |

### Config & Tools
| Command | Description |
|---------|-------------|
| `/mcp` | Manage MCP (Model Context Protocol) server connections |
| `/lsp` | Manage language server connections |
| `/skills` | List or manage available skills |
| `/plugin` | Manage installed plugins |
| `/remote` | Show remote control status; `/remote on` enables, `/remote off` disables |
| `/keep-alive` | Prevent system sleep while Copilot CLI is active |
| `/init` | Initialize Copilot configuration for the current repo |
| `/experimental` | Toggle experimental features |
| `/autopilot` | Toggle autopilot mode on/off (v1.0.45+) |
| `/allow-all` | Allow all tool calls without per-call confirmation |
| `/yolo` | Alias for `/allow-all`; state persists across `/restart` |
| `/reset-allowed-tools` | Reset tool allowlist to default (prompt-per-use) |

### Info & Help
| Command | Description |
|---------|-------------|
| `/help` | Show available commands and shortcuts |
| `/usage` | Show premium request usage for this session |
| `/version` | Show Copilot CLI version |
| `/changelog` | View recent release notes |
| `/release-notes` | Alias for `/changelog` |
| `/chronicle` | View a narrative history of session actions and file changes (v1.0.40+) |
| `/feedback` | Submit feedback to GitHub |
| `/bug` | Alias for `/feedback` — report a bug |
| `/instructions` | Show active instruction files in effect |

### Session Lifecycle
| Command | Description |
|---------|-------------|
| `/login` | Authenticate with GitHub |
| `/logout` | Sign out |
| `/restart` | Restart the current session |
| `/update` | Update CLI to the latest version; add `prerelease` to fetch latest prerelease (v1.0.44+) |
| `/upgrade` | Alias for `/update` |
| `/exit` | Exit Copilot CLI |

### Display
| Command | Description |
|---------|-------------|
| `/theme` | Change the color theme |
| `/streamer-mode` | Toggle streamer-safe mode (hides sensitive info) |
| `/statusline` | Customize status bar items (alias: `/footer`); items: `directory`, `branch`, `effort`, `context`, `quota`, `changes`, `username` (v1.0.43+) |
| `/keep-alive` | Prevent system sleep while active (v1.0.36+) |
| `/terminal-setup` | Configure terminal integration |
| `/ide` | Configure IDE integration |

---

## Most Useful Command-Line Flags

| Flag | Purpose |
|------|---------|
| `--allow-all` | Skip all tool confirmation prompts |
| `-p` / `--prompt` | Pass a prompt non-interactively |
| `--silent` | Suppress all output except the final response |
| `--model MODEL-ID` | Set the model for this session (`auto` lets Copilot choose) |
| `--experimental` | Enable experimental features |
| `--mode MODE` | Start in a specific mode: `interactive`, `plan`, or `autopilot` |
| `--autopilot` | Shorthand for `--mode autopilot` — start in autopilot mode |
| `--plan` | Shorthand for `--mode plan` — start in plan mode |
| `--max-autopilot-continues N` | Cap the number of autonomous continuation steps |
| `--no-ask-user` | Never pause to ask clarifying questions |
| `--continue` / `--resume` | Resume the most recent session from CWD (accepts 7+ char ID prefix or session name); auto-inherits `--remote` for remote sessions |
| `--connect SESSION-ID` | Connect directly to a remote session by ID |
| `-C DIRECTORY` | Change working directory before starting (like `git -C`; v1.0.42+) |
| `--name NAME` | Assign a friendly name to the session; use with `--resume=<name>` to resume by name |
| `--allow-tool TOOL` | Allow a specific tool without prompting |
| `--deny-tool TOOL` | Block a specific tool |
| `--remote` | Sync session with the remote GitHub repository |
| `--no-auto-update` | Disable automatic CLI updates |
| `--output-format FORMAT` | Set output format (e.g., `json`, `text`) |
| `--print-debug-info` | Print version, terminal capabilities, and env vars, then exit |
| `--session-idle-timeout DUR` | Close idle session after duration (e.g., `30m`); disabled by default |

### Shell Completion

Shell completions are automatically installed on first run (v1.0.41+). To manually regenerate:

```bash
# Generate static shell completion scripts (v1.0.37+)
copilot completion bash >> ~/.bashrc
copilot completion zsh  >> ~/.zshrc
copilot completion fish > ~/.config/fish/completions/copilot.fish
```

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `COPILOT_ALLOW_ALL=1` | Equivalent to `--allow-all` for all sessions |
| `GH_TOKEN` | GitHub personal access token for authentication |
| `COPILOT_MODEL` | Set the default model globally (e.g., `claude-haiku-4.5`) |
| `COPILOT_GH_HOST` | Override the GitHub hostname (takes precedence over `GH_HOST`; useful for GHES) |
| `COPILOT_HOME` | Override the config directory (replaces deprecated `--config-dir`; v1.0.40+) |
| `GITHUB_COPILOT_PROMPT_MODE_REPO_HOOKS=1` | Opt in to repo hooks (AGENTS.md, instructions) in prompt mode (`-p`) (v1.0.40+) |
| `GITHUB_COPILOT_PROMPT_MODE_WORKSPACE_MCP=1` | Opt in to workspace MCP servers (`.mcp.json`) in prompt mode (`-p`) (v1.0.40+) |
| `GITHUB_COPILOT_PROMPT_MODE_EXTENSIONS=true` | Opt in to project extensions and management tools in prompt mode (`-p`) (v1.0.41+) |

---

## File Reference (@) Patterns

| Pattern | Description | Example |
|---------|-------------|---------|
| `@file` | Single file by name | `@server.ts` |
| `@dir/file` | File by path | `@src/auth/login.ts` |
| `@**/*.ts` | All TypeScript files recursively | `@**/*.ts` |
| `@src/**/*.js` | All JS files under `src/` | `@src/**/*.js` |

---

## Permission Patterns

| Pattern | Matches |
|---------|---------|
| `shell` | Any shell command |
| `shell(git:*)` | Any `git` subcommand |
| `write` | Any file write operation |
| `write(src/*.ts)` | Write to `.ts` files directly in `src/` |
| `read(.env)` | Read access to `.env` files |
| `url(github.com)` | HTTP requests to `github.com` |

---

## Mode Comparison

| Mode | Best For | Avoid For |
|------|----------|-----------|
| **Interactive** | Exploration, Q&A, incremental edits, pair programming | Bulk tasks that would take many turns |
| **Plan** | Reviewing a proposed approach before committing | Simple one-liner requests |
| **Plan + Critic** | High-stakes plans needing a second-model review (experimental, Claude only) | Simple one-liner requests |
| **Autopilot** | Implementing a clear, well-scoped task end-to-end | Ambiguous requirements, sensitive codebases |
| **Fleet** | Parallel work across many files or modules | Tasks with complex interdependencies between files |
| **`/research`** | Deep knowledge gathering before a major change | Quick factual questions (use interactive instead) |

---

## Instruction Files Quick Lookup

| File | Read by | Use for |
|------|---------|---------|
| `AGENTS.md` | All Copilot agents (interactive, autopilot, fleet, delegate) | Project-wide rules, commands, code style, testing conventions |
| `.github/copilot-instructions.md` | GitHub Copilot broadly (including IDE extensions) | Org/repo-level instructions |
| `CLAUDE.md` | Anthropic Claude agents (alternative convention) | Same use as AGENTS.md, recognized by Claude-based tooling |
| `~/.copilot/copilot-instructions.md` | Your personal Copilot CLI sessions | Personal preferences, preferred languages, global style rules |
| `.github/instructions/*.md` | Context-specific agents | Per-topic rules (e.g., `testing.md`, `security.md`) |

---

## Common Workflows (One-Liners)

```
Research before refactor:
  /research Give me a deep-dive on the authentication module before I refactor it

Plan then autopilot:
  [Shift+Tab → plan mode] → review plan → [/experimental then Shift+Tab → autopilot] → implement

Plan with Critic review (experimental, Claude only):
  /experimental → [Shift+Tab → plan mode] → Critic auto-reviews plan → implement

Fleet for parallel test generation:
  /fleet Add unit tests for every file in src/services/

Delegate a GitHub issue:
  /delegate Fix issue #42 with full test coverage

Pre-PR checklist:
  /diff → /review → /delegate (or push manually after review)

CI non-interactive run:
  copilot -p "fix all lint errors" --allow-all --silent

Quick model check before long task:
  /model && /usage

Switch model mid-session:
  /model claude-opus-4.5

Add entire directory to context:
  /add-dir src/payments
```

---

## Config File Locations

| File | Purpose |
|------|---------|
| `~/.copilot/settings.json` | User-editable settings (model, theme, continueOnAutoMode, etc.) |
| `~/.copilot/config.json` | Internal Copilot CLI state (managed by the CLI) |
| `~/.copilot/mcp-config.json` | MCP server definitions and connection settings |
| `~/.copilot/lsp-config.json` | Language server configuration |
| `~/.copilot/skills/` | Custom skill definitions (YAML/JSON) |
| `~/.copilot/session-state/` | Saved session state and history |
| `.github/copilot-instructions.md` | Repo-level instructions for GitHub Copilot |
| `AGENTS.md` | Project-level agent instructions (repo root or subdirectories) |

---

## Model Quick Pick

```
Complex reasoning / security:   Claude Opus 4.7
Daily coding (default):         Claude Sonnet 4.5
Fast / CI / fleet subagents:    Claude Haiku 4.5
Code-specialized:               GPT-5.3-Codex or GPT-5.2-Codex
Let Copilot decide:             auto
```

> See [Model Selection and Costs](22-models-and-costs.md) for a full breakdown with decision trees and team budget patterns.

---

**Full guide index:** [README.md](README.md)
