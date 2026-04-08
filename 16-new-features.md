# Latest Features in GitHub Copilot CLI — v1.0.21

This file covers recent additions to GitHub Copilot CLI. Features marked with "Full guide →" have their own dedicated documentation file — the entries here are summaries with links. Features without a dedicated file are covered in full below.

## Table of Contents

### Features with dedicated guides
1. [Autopilot Mode](#autopilot-mode-experimental) — [Full guide →](17-autopilot-mode.md)
2. [Fleet Mode (`/fleet`)](#fleet-mode-fleet) — [Full guide →](18-fleet-mode.md)
3. [Research Command (`/research`)](#research-command-research) — [Full guide →](19-research-command.md)

### Features covered in this file
4. [New in v1.0.21](#new-in-v1021)
5. [New in v1.0.20](#new-in-v1020)
6. [New in v1.0.19](#new-in-v1019)
7. [New in v1.0.18](#new-in-v1018)
8. [New in v1.0.17](#new-in-v1017)
9. [New in v1.0.16](#new-in-v1016)
10. [New in v1.0.15](#new-in-v1015)
11. [New in v1.0.13](#new-in-v1013)
12. [New in v1.0.11](#new-in-v1011)
13. [LSP Support](#lsp-language-server-protocol-support)
14. [Code Review Agent (`/review`)](#code-review-agent-review)
15. [Plugin System (`/plugin`)](#plugin-system-plugin)
16. [Keyboard Shortcuts Reference](#keyboard-shortcuts-reference)
17. [PAT Authentication](#pat-authentication)
18. [Extended Instructions Support](#extended-instructions-support)
19. [Project Initialization (`/init`)](#project-initialization-init)
20. [Enhanced Pull Request Creation (`/delegate`)](#enhanced-pull-request-creation-delegate)
21. [Staying Up to Date](#staying-up-to-date)

---

## New in v1.0.21

Released: 2026-04-07

### `copilot mcp` — CLI Command for MCP Server Management

A new top-level CLI subcommand lets you manage MCP servers directly from your shell, without needing to start an interactive session:

```bash
$ copilot mcp
```

**Why it matters:** Previously MCP servers could only be added, disabled, or removed via `/mcp` slash commands inside an active Copilot session. The `copilot mcp` command gives you full MCP management from the shell — useful for scripts, CI setup, and one-time configuration changes.

> See [MCP Management Commands](08-advanced-features.md#mcp-management-commands) in the Advanced Features guide for the full list of subcommands.

### Hook Payloads Normalized to `snake_case`

Hook scripts that use **PascalCase event names** (e.g., `PreToolUse`, `PostToolUse`) now receive VS Code-compatible `snake_case` payloads. Each payload includes:

- `hook_event_name` — the snake_case name of the event
- `session_id` — the current session identifier
- Timestamps formatted as **ISO 8601** strings

Hooks already using `snake_case` event names are unaffected.

**Why it matters:** Hook scripts can now be shared between VS Code and Copilot CLI without conditional payload-handling logic, since both environments produce the same payload shape.

### Other Improvements

- **Spinner**: No longer appears stuck when a long-running async shell command is active.
- **Login flow**: Enterprise GitHub URL input now accepts keyboard input and submits on Enter.
- **Slash command picker**: No longer flickers or shifts the input while filtering.
- **Timeline**: No longer goes blank when content shrinks (e.g., after cancelling or tool completion).
- **Plan mode**: Timeline display shows user text without a redundant "Plan" prefix.
- **Memory**: Idle shell sessions are automatically shut down to reduce memory usage.

---

## New in v1.0.20

Released: 2026-04-07

### `copilot help monitoring` — OpenTelemetry Configuration Guide

A new built-in help topic covers OpenTelemetry monitoring in detail, including configuration options, environment variables, and examples for wiring up tracing backends.

```bash
$ copilot help monitoring
```

**Why it matters:** All OpenTelemetry configuration details — span kinds, attribute names, exporter setup — are now a single command away without leaving the terminal.

### `/yolo` Slash Command — Persists Across `/restart`

`/yolo` and `--yolo` now behave identically. In addition, `/yolo` state **persists across `/restart`** — you no longer need to re-enable it after restarting the session.

```
> /yolo
> /restart   # /yolo stays active
```

**Why it matters:** Avoids repeatedly re-enabling unrestricted mode during iterative autopilot sessions that use `/restart` to reset context.

### Azure OpenAI BYOK: Versionless v1 Route Default

When no API version is configured, Azure OpenAI BYOK connections now default to the GA **versionless v1 route** (`/openai/deployments/{deployment}/chat/completions?api-version=`). This eliminates errors caused by stale or missing API version strings.

**Why it matters:** BYOK Azure setups that previously required explicit API version pinning now work correctly out of the box.

### Spinner Active Until Background Work Completes

The activity spinner now stays visible until all **background agents and shell commands** finish — not just until the model stops streaming. User input remains available throughout.

**Why it matters:** Gives a clear visual signal that Copilot is still working, preventing premature follow-up messages that interrupt in-flight operations.

---

## New in v1.0.19

Released: 2026-04-06

### `/mcp enable` and `/mcp disable` Persist Across Sessions

MCP server enable/disable state is now saved between sessions. Previously, disabling an MCP server with `/mcp disable` was session-local and the server would re-enable on next launch.

```
> /mcp disable heavy-server   # now saved permanently until re-enabled
> /mcp enable heavy-server    # restore and save
```

**Why it matters:** No more re-running `/mcp disable` commands on startup for servers you rarely use.

### OpenTelemetry Monitoring Improvements

- Subagent spans now use **INTERNAL** span kind (previously unset), making agent hierarchy clearer in tracing backends.
- Chat spans now include a `github.copilot.time_to_first_chunk` attribute for streaming sessions, enabling first-token latency tracking.

### Slash Command Timeline Entries Now Include Command Name

The session timeline now labels slash command entries with the command name (e.g., "Review", "Plan") instead of a generic entry.

**Why it matters:** Makes session history and replays easier to navigate when a session contains multiple slash commands.

### Other Fixes

- Plugin hook scripts with missing execute permissions now run correctly on macOS.
- Custom agent is properly restored when resuming a session where the agent display name differs from its filename.
- IDE auto-connect is skipped when the session is already in use by another client.

---

## New in v1.0.18

Released: 2026-04-04

### Critic Agent (Experimental)

A new **Critic agent** automatically reviews plans and complex implementations using a complementary model to catch errors early. The Critic runs alongside the primary agent and provides an independent second opinion before changes are finalized.

The Critic agent is available in **experimental mode for Claude models only**.

**How to enable:**

```
> /experimental
```

Once experimental mode is active, the Critic agent runs automatically during plan reviews and complex multi-step implementations — no additional configuration required.

**Why it matters:** An independent model reviewing the work catches logical errors, missed edge cases, and architectural mistakes before they become code — without requiring you to manually review every step.

### Notification Hook Event

A new `notification` hook event fires **asynchronously** when significant events occur during a session:

- Shell command completes
- Permission prompt appears
- Elicitation dialog opens
- Agent completes a task

```json
{
  "hooks": {
    "notification": [
      {
        "command": "scripts/notify.sh"
      }
    ]
  }
}
```

Because the hook fires asynchronously, it does not block the agent or add latency to the session.

**Why it matters:** Lets you wire up desktop notifications, Slack alerts, or custom logging for any significant event during a long-running Copilot session.

### `preToolUse` Hook: `allow` Now Suppresses Approval Prompt

When a `preToolUse` hook returns `permissionDecision: 'allow'`, the tool approval prompt is now fully suppressed. Previously the hook could grant approval programmatically, but the UI still displayed the confirmation prompt.

```json
{
  "hooks": {
    "preToolUse": [
      {
        "command": "scripts/auto-approve-safe-tools.sh"
      }
    ]
  }
}
```

**Why it matters:** Enables seamless, prompt-free tool approvals in automated workflows without requiring `--allow-all`.

### Other Fixes

- Session resume picker now correctly groups sessions by branch and repository on first use.

---

## New in v1.0.17

Released: 2026-04-03

### Built-in Skills Now Included with the CLI

Starting in v1.0.17, the CLI ships with a set of built-in skills out of the box. The first included skill is a guide for customizing Copilot cloud agent's environment — available without any manual installation or configuration.

```
> /skills list
```

Look for skills marked `(built-in)` in the output. They are ready to use immediately on any install.

**Why it matters:** Previously all skills had to be created or installed manually. Built-in skills provide useful capabilities with zero setup.

### MCP OAuth: HTTPS Redirect URI Fallback

MCP OAuth flows now support HTTPS redirect URIs via a self-signed certificate fallback. This improves compatibility with OAuth providers that require HTTPS (e.g., Slack).

No configuration change is required — the fallback activates automatically when an OAuth provider rejects an HTTP redirect.

**Why it matters:** Enables MCP OAuth with providers that mandate HTTPS, such as Slack, without needing a full TLS setup.

### Faster `/resume` Session Picker

The `/resume` session picker now loads significantly faster, especially for users with large session histories.

```
> /resume
```

**Why it matters:** Large session histories no longer cause a noticeable delay when picking up previous work.

---

## New in v1.0.16

Released: 2026-04-02

### PermissionRequest Hook

A new `PermissionRequest` hook lets scripts programmatically approve or deny tool permission requests. This is particularly useful in CI/CD pipelines and automated workflows where interactive prompts aren't possible.

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "command": "scripts/approve-tool.sh"
      }
    ]
  }
}
```

The hook script receives the permission request details and exits `0` to approve or non-zero to deny.

**Why it matters:** Enables fully automated, non-interactive Copilot runs without using `--allow-all`.

### MCP Tool Calls Shown in Timeline

MCP tool calls now display the tool name and a parameter summary directly in the session timeline, making it easier to audit exactly what each MCP server did during a session.

**Why it matters:** Improved observability for sessions that use MCP servers heavily.

### `postToolUseFailure` Hook

A new `postToolUseFailure` hook fires when a tool call fails, enabling custom error-handling scripts. The existing `postToolUse` hook now only fires on successful tool calls (previously it fired on both success and failure).

**Why it matters:** Lets you react specifically to tool errors — for example, logging failures or sending alerts — without duplicating logic in `postToolUse`.

### Deprecated: `marketplaces` Config Key

> ⚠️ **Removed in v1.0.16:** The `marketplaces` repository config setting has been removed. Use `extraKnownMarketplaces` instead.

```json
// ❌ Old (removed)
{ "marketplaces": ["https://plugins.example.com/registry.json"] }

// ✅ New
{ "extraKnownMarketplaces": ["https://plugins.example.com/registry.json"] }
```

### Other Fixes and Improvements

- SQL prompt tags no longer appear when the `sql` tool is excluded via `excludedTools` or `availableTools`
- MCP servers reconnect correctly with valid authentication when the working directory changes
- MCP servers load correctly after login, user switch, and `/mcp reload`
- BYOK Anthropic provider now respects the configured `maxOutputTokens` limit

---

## New in v1.0.15

Released: 2026-04-01

### `/share html` — Export Session as Interactive HTML

Export your session or research report as a self-contained interactive HTML file — no GitHub account required to view it.

```
> /share html
> /share html ~/reports/my-session.html
```

**Why it matters:** Easily share Copilot sessions with teammates or stakeholders who don't have CLI access.

### `/mcp auth` — MCP OAuth Re-authentication

Authenticate or re-authenticate an MCP OAuth server, with account switching support.

```
> /mcp auth my-server
```

**Why it matters:** Fixes auth expiry without needing to remove and re-add a server.

### Config Keys Now Use camelCase

Config settings now prefer camelCase names. snake_case still works for backwards compatibility.

| Old key | New key |
|---------|---------|
| `ask_user` | `askUser` |
| `auto_update` | `autoUpdate` |
| `store_token_plaintext` | `storeTokenPlaintext` |
| `log_level` | `logLevel` |
| `skill_directories` | `skillDirectories` |
| `disabled_skills` | `disabledSkills` |

### Autopilot No Longer Resumes After Cancel

Pressing **Escape** or **Ctrl+C** now fully stops Autopilot. Previously, autopilot could continue after a cancel under some conditions.

### Ctrl+D No Longer Queues Messages

`Ctrl+D` is now shutdown-only. Use `Ctrl+Q` or `Ctrl+Enter` to queue a message while the agent is running.

### Model Removals

The following models were removed in v1.0.15:
- `gpt-5.1-codex`
- `gpt-5.1-codex-mini`
- `gpt-5.1-codex-max`

> ⚠️ **Removed in v1.0.15:** If you were using `gpt-5.1-codex*` models, switch to `gpt-5.3-codex` or `gpt-5.2-codex`. Run `/model` to see current options.

---

## New in v1.0.13

Released: 2026-03-30

### `/rewind` Timeline Picker

`/rewind` and double-Esc now open a **timeline picker** to roll back to any point in conversation history — not just the previous snapshot.

```
> /rewind
# or press Esc Esc to open the picker
```

**Why it matters:** Recover from mistakes several turns back without losing all context.

### MCP Servers Can Request LLM Inference

MCP servers can now request LLM inference (sampling) with user approval via a review prompt. This allows MCP tools to leverage AI reasoning as part of their workflows.

### Model Removals

`gemini-3-pro-preview` was removed in v1.0.13.

> ⚠️ **Removed in v1.0.13:** If you were using `gemini-3-pro-preview`, switch to another available model via `/model`.

---

## New in v1.0.11

### `/rewind` / `/undo` — Undo Last Turn

Rewind the last turn and revert all file changes made during it. Useful when an AI action went wrong.

```
> /rewind
```

Both `/rewind` and `/undo` do the same thing.

### `/context` — Token Usage Visualization

Show a breakdown of the context window token usage, including a visualization of what's taking up space.

```
> /context
```

### `/compact` — Summarize Conversation History

Compress the current conversation to reduce context window usage while retaining key information.

```
> /compact
```

### New Model Lineup

v1.0.11 adds several new models to the picker:

| Model | ID |
|-------|----|
| Claude Sonnet 4.6 | `claude-sonnet-4.6` |
| Claude Opus 4.6 | `claude-opus-4.6` |
| Claude Opus 4.6 (fast) | `claude-opus-4.6-fast` |
| GPT-5.4 mini | `gpt-5.4-mini` |
| GPT-4.1 | `gpt-4.1` |

Run `/model` to see all available models with current multipliers.

### `/streamer-mode` — Safe Streaming

Toggle streamer mode to hide preview model names and quota details — useful when screen-sharing or live streaming.

```
> /streamer-mode
```

### Keyboard Shortcut Updates

Several keyboard shortcuts changed meaning in v1.0.11. See the full table in [Keyboard Shortcuts Reference](#keyboard-shortcuts-reference) below.

---

## Autopilot Mode (Experimental)

Autopilot mode lets Copilot work end-to-end on a task without pausing for your input at every step. Enable experimental features first, then press **Shift+Tab** to cycle to autopilot mode.

```
interactive  →  plan  →  autopilot  →  (back to interactive)
```

> **Note:** Without experimental mode active, `Shift+Tab` only cycles between **interactive** and **plan**. Enabling `/experimental` adds autopilot to the cycle.

```bash
# Enable experimental features first
copilot --experimental

# Or from inside the CLI
> /experimental
```

**✅ Good for:** Large refactors, end-to-end feature work, CI fixes, batch operations with clear success criteria.  
**❌ Avoid for:** Exploratory work, open-ended tasks, or sensitive production configs where you want step-by-step approval.

> **See the full guide:** [Autopilot Mode →](17-autopilot-mode.md)
> Covers permissions, `--max-autopilot-continues`, the plan→autopilot workflow, and four detailed examples.

---

## Fleet Mode (`/fleet`)

Fleet mode enables **parallel subagent execution** — the main Copilot agent acts as an orchestrator, breaking your request into independent subtasks and running them concurrently via subagents. Useful for large refactors, test generation, and any multi-part work with independent pieces.

```
> /fleet Add unit tests for every service in src/services/

[Orchestrator spawns one subagent per file — all run concurrently]
```

Monitor progress with `/tasks`. Navigate with `↑↓`, press `Enter` for details, `k` to kill, `r` to remove.

**Best combined with plan mode:** draft a plan, then choose **Accept plan and build on autopilot + /fleet** for fully autonomous parallel execution.

> **See the full guide:** [Fleet Mode →](18-fleet-mode.md)
> Covers the orchestrator model, custom agents, per-subtask model selection, cost considerations, and four detailed examples.

---

## Research Command (`/research`)

`/research` activates a **specialized research agent** that gathers information from your codebase, GitHub repos, and the web, then produces a comprehensive cited Markdown report. It is not a mode — it's a slash command for deep investigation work.

```
> /research How does authentication work in this codebase?

[Agent searches codebase, GitHub, and web — compiles full report]

Summary: Auth uses JWT tokens (src/auth/), sessions stored in Redis,
         refresh tokens handled separately in src/auth/tokens.js.

Full report: ~/.copilot/session-state/.../research/authentication.md
Press Ctrl+Y to open.
```

The agent classifies your query (process / conceptual / technical deep-dive) and adapts the report format. It uses a **fixed built-in model** regardless of your `/model` setting.

**Share the report:**
```
> /share gist research      # Publish as a GitHub Gist
> /share file research      # Save as a local Markdown file
```

> **See the full guide:** [Research Command →](19-research-command.md)
> Covers query-type classification, `Ctrl+Y`, sharing, finding past reports, and six example prompts with explanations.

---

## LSP (Language Server Protocol) Support

### Overview

Copilot CLI now supports **Language Server Protocol (LSP)** integration, enabling rich code intelligence features: go-to-definition, hover documentation, and inline diagnostics. LSP servers run alongside the CLI and feed the AI more precise context about your code.

### Installing an LSP Server

LSP servers are installed separately via your package manager. Example for TypeScript:

```bash
npm install -g typescript-language-server typescript
```

Other common LSP servers:

```bash
# Python
pip install python-lsp-server

# Rust
rustup component add rust-analyzer

# Go
go install golang.org/x/tools/gopls@latest
```

### Configuration

#### User-Level Config

Create or edit `~/.copilot/lsp-config.json` to define LSP servers globally:

```json
{
  "lspServers": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".ts": "typescript",
        ".tsx": "typescript"
      }
    },
    "javascript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".js": "javascript",
        ".jsx": "javascript"
      }
    }
  }
}
```

#### Repository-Level Config

For project-specific LSP settings, create `.github/lsp.json` in the repository root:

```json
{
  "lspServers": {
    "python": {
      "command": "pylsp",
      "args": [],
      "fileExtensions": {
        ".py": "python"
      }
    }
  }
}
```

Repository-level config takes precedence over user-level config for matching file extensions.

### Checking LSP Status

```
> /lsp

LSP Servers:
  typescript  ✅ Running  (pid 12345)
  python      ❌ Not configured
```

### What LSP Enables

With LSP active, the AI can:
- **Go-to-definition** — trace where a symbol is declared
- **Hover information** — get type signatures and documentation
- **Diagnostics** — see compiler errors and warnings inline
- **Rename symbols** — safely refactor identifiers across files
- **Find references** — understand where a function or variable is used

---

## Code Review Agent (`/review`)

### Overview

The `/review` command runs a dedicated, high-signal code review agent on your current changes. It focuses exclusively on what matters: bugs, security vulnerabilities, and logic errors — not style or formatting.

### Basic Usage

```
> /review

Code Review Agent
━━━━━━━━━━━━━━━━
Analyzing staged changes...

🔴 CRITICAL: src/auth/login.js (line 42)
   SQL query built with string concatenation — potential SQL injection.
   Fix: Use parameterized queries.

🟡 WARNING: src/api/upload.js (line 18)
   No file size validation before writing to disk.
   Fix: Check Content-Length header and enforce a maximum.

✅ src/utils/format.js — No issues found.
✅ src/models/user.js — No issues found.
```

### Reviewing Before Committing

Use `/review` alongside `/diff` for a pre-commit workflow:

```
> /diff        # See what changed
> /review      # Get the AI review
> /delegate    # Create the PR when ready
```

### Reviewing Specific Files

```
> /review @src/payments/processor.js

[Focuses the review agent on just that file]
```

### Integration with PR Workflow

After review, act on findings without leaving the CLI:

```
> /review
[AI finds a potential null-pointer dereference]

> Fix the issue the review agent found in src/api/orders.js

[AI applies the fix]

> /delegate Fix null-pointer issue in order processing
```

---

## Plugin System (`/plugin`)

### Overview

The plugin system lets you extend Copilot CLI with additional capabilities beyond the built-in tool set. Plugins can add new slash commands, custom skills, and integrations with external services.

### Browsing Available Plugins

```
> /plugin list

Available Plugins:
  copilot-jira       — Jira issue integration
  copilot-datadog    — Datadog metrics and logs
  copilot-terraform  — Terraform plan and apply assistance
  copilot-k8s        — Kubernetes cluster management
```

### Installing a Plugin

```
> /plugin install copilot-jira

Installing copilot-jira...
✅ Plugin installed. New command available: /jira
```

### Managing Marketplaces

```
> /plugin marketplace add https://plugins.example.com/registry.json

Marketplace added. Run /plugin list to see new plugins.
```

### Removing a Plugin

```
> /plugin remove copilot-jira

✅ copilot-jira removed.
```

---

## Keyboard Shortcuts Reference

The full list of keyboard shortcuts in v1.0.11:

### Navigation & Control

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current input / interrupt / copy selection |
| `Ctrl+C` × 2 | Exit Copilot CLI |
| `Ctrl+D` | Shutdown |
| `Ctrl+L` | Clear screen |
| `Esc` | Cancel the current operation |
| `↑ / ↓` | Navigate command history |
| `Shift+Tab` | Cycle modes: interactive → plan (autopilot requires `/experimental`) |
| `Ctrl+S` | Run command while preserving input |
| `Ctrl+T` | Toggle model reasoning display |
| `!` | Execute command in local shell (bypass Copilot) |

### Timeline

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Expand recent timeline (when no input) |
| `Ctrl+E` | Expand all timeline (when no input) |
| `Ctrl+X → O` | Open link from most recent timeline event |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line (when typing) |
| `Ctrl+W` | Delete previous word |
| `Ctrl+U` | Delete from cursor to beginning of line |
| `Ctrl+K` | Delete from cursor to end of line |
| `Ctrl+H` | Delete previous character (backspace) |
| `Meta+← / →` | Move cursor by word |
| `Ctrl+G` | Edit prompt in external editor |

### Using `Ctrl+G` (External Editor)

This is especially useful for long, multi-line prompts:

```
# Start typing a prompt
> Implement a new feature that...

# Press Ctrl+G — your $EDITOR opens with the prompt text
# Edit in full-screen, save, and close
# The edited prompt is sent automatically
```

### Using `Ctrl+X → O` (Open Link)

After any operation that produces a URL (a PR, a research result, a share link):

```
> /delegate Fix authentication bug

🔗 PR #147 created: https://github.com/user/repo/pull/147

# Press Ctrl+X then O to open that URL in your browser
```

---

## PAT Authentication

### Overview

In addition to `gh auth login`, Copilot CLI supports authentication via a **fine-grained Personal Access Token (PAT)**. This is useful for CI/CD environments, shared machines, or scenarios where interactive OAuth isn't feasible.

### Creating a PAT

1. Visit: https://github.com/settings/personal-access-tokens/new
2. Set a token name and expiration
3. Under **Permissions**, grant **"Copilot Requests"** (read/write)
4. Click **Generate token** and copy the value

### Setting the Token

```bash
# Set for the current shell session
export GH_TOKEN=github_pat_your_token_here

# Or use GITHUB_TOKEN (both are supported)
export GITHUB_TOKEN=github_pat_your_token_here
```

To persist across sessions, add the export to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.).

### Verifying Authentication

```bash
gh auth status
# Should show: ✓ Logged in to github.com as <username>
```

### When to Use PAT Auth

- **CI/CD pipelines** — non-interactive environments
- **Docker containers** — where browser auth isn't available
- **Service accounts** — automated workflows with minimal permissions
- **Multiple accounts** — switch between tokens for different organizations

---

## Extended Instructions Support

### Overview

Copilot CLI now reads custom instructions from a wider set of locations, giving you flexible control over AI behavior at the user, project, and team level.

### Instruction File Locations (in priority order)

| Location | Scope |
|----------|-------|
| `$HOME/.copilot/copilot-instructions.md` | User-level, applies everywhere |
| `.github/copilot-instructions.md` | Repository-level |
| `.github/instructions/**/*.instructions.md` | Repository-level, per-topic files |
| `AGENTS.md` (git root and cwd) | Project-level agent instructions |
| `CLAUDE.md` | Project-level (Claude-compatible format) |
| `GEMINI.md` | Project-level (Gemini-compatible format) |
| `COPILOT_CUSTOM_INSTRUCTIONS_DIRS` env var | Additional directories (colon-separated) |

### Example: Repository-Level Instructions

```markdown
<!-- .github/copilot-instructions.md -->

## Project: Payments Service

- Always use parameterized queries — never string interpolation in SQL
- All monetary values must use the `Money` type from `src/types/money.ts`
- PRs must include a test for every changed code path
- Follow the error handling patterns in `src/utils/errors.ts`
```

### Example: Per-Topic Instructions

```markdown
<!-- .github/instructions/testing.instructions.md -->

## Testing Standards

- Use Vitest for all unit and integration tests
- Test file naming: `*.test.ts` in the same directory as the source
- Minimum 80% branch coverage for new files
- Always test error paths, not just the happy path
```

### Example: Additional Directories via Env Var

```bash
# Include instructions from multiple directories
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="/team/shared-instructions:/project/instructions"
```

### How Instructions Are Combined

All applicable instruction files are merged and provided to the model as context. More specific scopes (repository-level) take precedence when there are conflicts with user-level instructions.

---

## Project Initialization (`/init`)

### Overview

The `/init` command scaffolds complete projects from scratch. It asks targeted questions, makes smart configuration choices, and sets up everything you need to start coding immediately.

### Basic Usage

#### Interactive Mode (Recommended)

```
> /init

AI: What type of project would you like to create?

1. Frontend (React, Vue, Angular, Next.js)
2. Backend API (Express, FastAPI, Flask)
3. Full-Stack (Next.js, MERN, Django)
4. Mobile (React Native, Flutter)
5. Desktop (Electron, Tauri)
6. CLI Tool (Node, Python, Rust)
7. Other

Select option (1-7):
> 1
```

#### Direct Mode

```
> /init react-app
> /init nextjs
> /init express-api
> /init python-flask
> /init rust
```

### Supported Project Types

- **Frontend:** React (Vite), Next.js (App Router), Vue 3, Angular, Svelte
- **Backend:** Express, FastAPI, Flask, NestJS, Django
- **Full-Stack:** Next.js full-stack, MERN, T3 Stack
- **Mobile:** React Native, Flutter
- **CLI Tools:** Node, Python, Rust, Go

### Example: React App

```
> /init react-app

AI Questions:
- Project name? my-app
- TypeScript? yes
- Build tool? Vite
- State management? Zustand
- UI Library? Tailwind CSS

AI: Generating project...

✅ Project created: my-app/
   - React 18 + TypeScript
   - Vite build tool
   - Zustand for state
   - Tailwind CSS
   - ESLint + Prettier
   - Vitest for testing

Next steps:
  cd my-app
  npm install
  npm run dev
```

### Tips for Using `/init`

**✅ Do:**
- Use interactive mode when exploring options
- Review the generated structure before committing
- Use for new projects and learning setups

**❌ Don't:**
- Use in an existing project directory (will conflict)
- Skip the generated `README.md` — it contains important setup steps
- Forget to configure required environment variables

---

## Enhanced Pull Request Creation (`/delegate`)

### Overview

The `/delegate` command automates end-to-end PR creation: it implements changes, runs tests, creates a well-described pull request, and handles reviewer assignment — all from a single instruction.

### Core Features

#### Intelligent Issue Linking

```
> /delegate Implement the feature described in issue #123

AI: Analyzing issue #123...
    Title: "Add user profile editing"
    I'll implement this with:
    - Profile edit form
    - API endpoint for updates
    - Input validation + tests
    
    Linked issue #123 will be auto-closed when PR merges.
    Proceed? (y/n)
```

#### Pre-Push Test Validation

```
> /delegate Fix authentication bug
  Run tests before pushing

AI: Making changes...
   ✓ src/auth/login.js
   ✓ tests/auth.test.js

🧪 Running test suite...
   ✓ auth.test.js (12 tests)
   ✓ integration.test.js (5 tests)
   All tests passed ✓ — safe to push? (y/n)
```

#### Draft PR Support

```
> /delegate --draft Experimental: try Redis caching

🔗 Created DRAFT PR #145
   This will not trigger CI/CD or request reviews.
   Mark as "Ready for review" when done.
```

#### Custom Branch Strategy

```
> /delegate --branch feature/user-profile-v2 Add profile editing
> /delegate --base develop Add new feature
```

#### Multi-File Change Intelligence

```
> /delegate Update all API endpoints to use async/await

AI: Found 23 files with API endpoints.
    Updating all to async/await with consistent error handling...

🔧 Updating 23 files...
🧪 Running tests... ✓ All passed
🔗 PR #146 created with comprehensive changeset
```

### Advanced Patterns

```
# Fix + regression tests
> /delegate Fix bug #234
  Add regression tests
  Run full test suite before pushing

# Feature + documentation
> /delegate Implement OAuth2 authentication
  Include implementation, API docs, and migration guide

# Refactor + verify
> /delegate Refactor database layer to repository pattern
  Ensure no breaking changes and all tests pass
```

---

## Staying Up to Date

### Checking Your Version

```bash
copilot --version
# Example: GitHub Copilot CLI version 1.0.11
```

### Updating

```bash
# Homebrew (macOS/Linux)
brew update && brew upgrade copilot-cli

# npm global install
npm update -g @github/copilot

# WinGet (Windows)
winget upgrade GitHub.Copilot

# Or use the built-in update command
> /update
```

### Checking for Experimental Features

New experimental features are gated behind the `--experimental` flag or `/experimental` command. Check the release notes after each update to discover what's newly available:

```
> /experimental
> /help
```

### Getting Help

```
> /help           # Full command reference
> /init --help    # Init-specific options
> /delegate --help
> /lsp            # LSP server status
> /plugin list    # Available plugins
```

---

**Previous:** [Copilot Directory](15-copilot-directory.md)  
**Related:** [Slash Commands](04-slash-commands.md) | [Examples](12-examples.md) | [Troubleshooting](11-troubleshooting.md)
