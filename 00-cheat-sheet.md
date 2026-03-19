# GitHub Copilot CLI — Cheat Sheet

> This is a quick reference. For full documentation see the [Guide Index](README.md).

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current input / interrupt running task |
| `Ctrl+D` | Exit Copilot CLI (EOF) |
| `Ctrl+L` | Clear the screen |
| `Ctrl+T` | Toggle between conversation and terminal view |
| `Ctrl+G` | Open file picker / context selector |
| `Ctrl+Y` | Accept a suggestion |
| `Ctrl+O` | Open current file in editor |
| `Ctrl+E` | Edit last message |
| `Ctrl+X → O` | Switch focus to output pane |
| `Shift+Tab` | Cycle through modes (interactive → plan → autopilot) |
| `Esc` | Cancel / dismiss picker |
| `↑ / ↓` | Navigate history / picker options |
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line |
| `Ctrl+W` | Delete word before cursor |
| `Ctrl+U` | Delete entire line before cursor |
| `Ctrl+K` | Delete from cursor to end of line |
| `Ctrl+H` | Delete character before cursor (backspace) |
| `Meta+← / →` | Move cursor word by word |
| `Ctrl+S` | Save / snapshot current session state |

---

## Modes

| Mode | How to Enter | When to Use |
|------|-------------|-------------|
| **Interactive** | Default on launch | Conversational coding, exploration, Q&A |
| **Plan** | `Shift+Tab` (first press) | Review a proposed implementation plan before executing |
| **Autopilot** | `Shift+Tab` (second press) | Let Copilot implement autonomously with minimal interruption |

---

## All Slash Commands

### Session & Context
| Command | Description |
|---------|-------------|
| `/clear` | Clear conversation history and context |
| `/compact` | Summarize and compress current context to save tokens |
| `/context` | Show what's currently in context |
| `/session` | Show current session info |
| `/resume` | Resume a previous session |
| `/rename` | Rename the current session |
| `/share` | Share session transcript |
| `/copy` | Copy last response to clipboard |

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
| `/tasks` | View or manage running agent tasks |
| `/research` | Run a deep research pass on a topic or codebase |
| `/plan` | Generate a step-by-step implementation plan |

### Config & Tools
| Command | Description |
|---------|-------------|
| `/mcp` | Manage MCP (Model Context Protocol) server connections |
| `/lsp` | Manage language server connections |
| `/skills` | List or manage available skills |
| `/plugin` | Manage installed plugins |
| `/init` | Initialize Copilot configuration for the current repo |
| `/experimental` | Toggle experimental features |
| `/allow-all` | Allow all tool calls without per-call confirmation |
| `/reset-allowed-tools` | Reset tool allowlist to default (prompt-per-use) |

### Info & Help
| Command | Description |
|---------|-------------|
| `/help` | Show available commands and shortcuts |
| `/usage` | Show premium request usage for this session |
| `/version` | Show Copilot CLI version |
| `/changelog` | View recent release notes |
| `/feedback` | Submit feedback to GitHub |
| `/instructions` | Show active instruction files in effect |

### Session Lifecycle
| Command | Description |
|---------|-------------|
| `/login` | Authenticate with GitHub |
| `/logout` | Sign out |
| `/restart` | Restart the current session |
| `/exit` | Exit Copilot CLI |

### Display
| Command | Description |
|---------|-------------|
| `/theme` | Change the color theme |
| `/streamer-mode` | Toggle streamer-safe mode (hides sensitive info) |
| `/terminal-setup` | Configure terminal integration |
| `/ide` | Configure IDE integration |

---

## Most Useful Command-Line Flags

| Flag | Purpose |
|------|---------|
| `--allow-all` | Skip all tool confirmation prompts |
| `-p` / `--prompt` | Pass a prompt non-interactively |
| `--silent` | Suppress all output except the final response |
| `--model MODEL-ID` | Set the model for this session |
| `--experimental` | Enable experimental features |
| `--max-autopilot-continues N` | Cap the number of autonomous continuation steps |
| `--no-ask-user` | Never pause to ask clarifying questions |
| `--continue` / `--resume` | Resume the most recent session |
| `--allow-tool TOOL` | Allow a specific tool without prompting |
| `--deny-tool TOOL` | Block a specific tool |
| `--no-auto-update` | Disable automatic CLI updates |
| `--output-format FORMAT` | Set output format (e.g., `json`, `text`) |

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `COPILOT_ALLOW_ALL=1` | Equivalent to `--allow-all` for all sessions |
| `GH_TOKEN` | GitHub personal access token for authentication |
| `COPILOT_MODEL` | Set the default model globally (e.g., `claude-haiku-4.5`) |

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
  [Shift+Tab → plan mode] → review plan → [Shift+Tab → autopilot] → implement

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
| `~/.copilot/config.json` | Global Copilot CLI configuration |
| `~/.copilot/mcp-config.json` | MCP server definitions and connection settings |
| `~/.copilot/lsp-config.json` | Language server configuration |
| `~/.copilot/skills/` | Custom skill definitions (YAML/JSON) |
| `~/.copilot/session-state/` | Saved session state and history |
| `.github/copilot-instructions.md` | Repo-level instructions for GitHub Copilot |
| `AGENTS.md` | Project-level agent instructions (repo root or subdirectories) |

---

## Model Quick Pick

```
Complex reasoning / security:   Claude Opus 4.5
Daily coding (default):         Claude Sonnet 4.5
Cheap / CI / fleet subagents:   Claude Haiku 4.5
```

> See [Model Selection and Costs](22-models-and-costs.md) for a full breakdown with decision trees and team budget patterns.

---

**Full guide index:** [README.md](README.md)
