# Slash Commands Reference

Slash commands are special commands that start with `/` and provide quick access to CLI features. As of v1.0.44, slash commands can appear **anywhere in your input** — not just at the start of a message — and multiple slash commands can be used in a single message. This is a complete reference of all available commands.

## Command Categories

- [Session Management](#session-management)
- [Models & Subagents](#context--model)
- [Code](#code)
- [Permissions & File System](#file-system)
- [GitHub Integration](#github-integration)
- [Configuration](#configuration)
- [Information](#information)
- [Advanced Features](#advanced-features)

## Session Management

### /clear, /new, /reset

Clear the conversation history and start fresh. `/reset` is an alias for `/clear`.

```
> /clear
```

**Use when:**
- Switching to a completely different task
- Context window is getting full
- Want to start with clean slate

**Note:** This doesn't delete the session, just resets the conversation. As of v1.0.40, `/clear` and `/new` also reset the active custom agent selection.

### /resume, /continue [sessionId]

Switch to a different session or resume a previous one. `/continue` is an alias for `/resume`.

```
# List all sessions
> /resume

# Resume specific session (full ID or 7+ character prefix)
> /resume abc123-def456-789
> /resume abc123d
```

**Sessions include:**
- Conversation history
- File context
- Working directory
- Checkpoints

**Remote sessions:** When resuming a remote session, the `--remote` flag is automatically inherited — no need to re-specify it:

```bash
# Before v1.0.33 you needed --remote
copilot --resume <session-id> --remote

# v1.0.33+: --remote is inherited automatically
copilot --resume <session-id>
```

**Cloud agent sessions (v1.0.47+):** `--resume` now works even when the cloud agent has not yet pushed any commits to its branch:

```bash
copilot --resume <cloud-agent-session-id>
```

### /session [subcommand]

Manage and view session information.

```
# Show session overview
> /session

# Display the current session ID and copy to clipboard (v1.0.49+)
> /session id

# View recent checkpoints
> /session checkpoints
> /session checkpoints 10      # Last 10 checkpoints

# List files in workspace
> /session files

# Show implementation plan
> /session plan

# Rename current session
> /session rename "My Project Feature"

# Delete a specific session (full ID or 7+ character prefix)
> /session delete abc123d

# Delete all sessions
> /session delete-all
```

**Subcommands:**
- `id` - Display the current session ID and copy it to the clipboard (v1.0.49+)
- `checkpoints [n]` - View session checkpoints
- `files` - List workspace files
- `plan` - Display plan.md if it exists
- `rename <name>` - Give session a friendly name
- `delete <id>` - Delete a specific session by ID (or 7+ char prefix)
- `delete-all` - Delete all sessions

**Tip:** In the session picker (`/resume`), press `x` on any entry to delete it.

### /rename <name>

Rename the current session. Alias for `/session rename`.

```
> /rename "My Feature Work"
> /rename "Bug Fix - Auth Issue"
```

**Use when:**
- You want a descriptive name for the current session
- Organizing work across multiple sessions
- Making it easier to find sessions with `/resume`

### /fork (v1.0.45+)

Fork the current session into a new, fully independent session. The forked session inherits the current conversation history and context, then diverges from that point forward. In v1.0.47+, you can supply an optional name and the sessions dialog shows the origin session for every fork.

> **v1.0.64:** `/branch` is now an alias for `/fork`, matching Claude Code's command naming.

```
> /fork
> /fork my-experiment
> /branch my-experiment   # alias for /fork (v1.0.64+)
```

**What it does:**
1. Creates a copy of the current session (optionally with a provided name)
2. Opens the fork as the active session (showing its origin in the sessions dialog, v1.0.47+)
3. The original session is preserved unchanged and resumable via `/resume`

**Use when:**
- Exploring a risky or experimental approach without losing your current progress
- Branching work to try two different solutions in parallel
- Keeping a clean checkpoint before a batch of destructive changes

### /rewind, /undo

Rewind the last turn and revert any file changes made during that turn. In v1.0.13+, `/rewind` and double-Esc open a **timeline picker** to roll back to any point in conversation history — not just the previous snapshot. Use **↑ / ↓** to select a checkpoint and **Enter** to confirm (v1.0.28+; the 1–9 quick-select shortcut was removed).

```
> /rewind
```

**Use when:**
- The AI made edits you want to undo
- You want to retry a prompt with different wording
- You accidentally ran a destructive operation
- You need to roll back several turns at once (use the timeline picker)

> **Tip:** `/undo` is an alias for `/rewind` — both work identically.

> **v1.0.63+ (experimental):** `/rewind` no longer requires a git repository. It also restores only the files Copilot changed — your own edits are preserved. A choice menu lets you pick **Conversation only** (roll back chat history, leave files alone) or **Conversation + files** (roll back chat history and restore Copilot-modified files).

> **v1.0.78+:** `/rewind` also skips restoring any file whose contents no longer match what Copilot last wrote — for example, if you've edited that file yourself since. This prevents your own subsequent edits to a Copilot-touched file from being overwritten.

### /copy

Copy the last AI response to the clipboard.

```
> /copy
```

**Use when:**
- Pasting the last response into another application
- Copying generated code into your IDE
- Saving output without exporting the full session

## Context & Model

### /model [model]

Select or change the AI model.

```
# Show available models
> /model

# Switch to specific model
> /model claude-opus-4.5
> /model gpt-5.2
```

**Model family aliases (v1.0.64+):** Short aliases resolve to the latest model in each family:

```
> /model opus     # latest Claude Opus
> /model sonnet   # latest Claude Sonnet
> /model haiku    # latest Claude Haiku
> /model gpt      # latest GPT
> /model gemini   # latest Gemini
```

**Available models:**
- `claude-sonnet-4.5` (default) - Balanced performance
- `claude-sonnet-4` - Previous generation Sonnet
- `claude-sonnet-5` - Newest Sonnet, added v1.0.67
- `gpt-5` - OpenAI flagship
- `gpt-5.6` - Newest GPT generation, added v1.0.70
- `kimi-k2.7-code` - Code-specialized model, added v1.0.68
- `kimi-k3` - Newest Kimi generation, added v1.0.79
- And more available via `/model` selection menu...

**Choose based on:**
- Task complexity (use Opus for complex reasoning)
- Speed requirements (use Haiku for quick tasks)
- Cost considerations (Haiku is most economical)

**Scoped changes (v1.0.70+):** Add `--repo` or `--local` to change the model for just the current repository or local config instead of your global default:

```
> /model gpt-5.6 --repo
> /model claude-opus-4.8 --local
```

**Session-only changes (v1.0.72+):** Add `--session` (or `-s`) to change the model, reasoning effort, or context window for just the current session, leaving global settings unchanged:

```
> /model claude-opus-4.8 --session
> /model gpt-5.6 -s
```

**Plan Mode model override (v1.0.74+):** Add `plan` (or `--plan`) to set a model used only while in Plan Mode. Pass a model id, `off` to clear it, or no id to open the picker. The override reverts to your regular session model when you leave Plan Mode:

```
> /model plan claude-opus-4.8
> /model --plan gpt-5.6
> /model plan off
```

> **v1.0.67+:** Claude Sonnet 5 is now available as a supported model. See [Model Selection Strategy](22-models-and-costs.md) for details.
>
> **v1.0.68+:** `kimi-k2.7-code` is now available as a supported model. See [Model Selection Strategy](22-models-and-costs.md) for details.
>
> **v1.0.70+:** `gpt-5.6` is now available as a supported model. See [Model Selection Strategy](22-models-and-costs.md) for details.
>
> **v1.0.77+:** Reasoning effort can now be left unset — omit it and the server selects the default effort level for the chosen model instead of requiring you to pick one explicitly.
>
> **v1.0.79+:** `kimi-k3` is now available as a supported model. See [Model Selection Strategy](22-models-and-costs.md) for details. The model picker also now groups models into **Recent**, **Recommended**, **New**, and other sections; press `Shift+Tab` to cycle between grouping views. `/model` changes are now **session-scoped by default**; use `/config model <model-id>` to set the default model for future sessions instead.

### /context

Display context window usage and included files.

```
> /context
```

**Shows:**
- Token usage (current/max)
- Percentage used
- List of files in context
- Custom Instructions (shown as a distinct section, separate from the base system prompt — v1.0.60)
- Per-server MCP tool token costs (cross-referenced with `/mcp` — v1.0.60)
- Conversation length
- Visual bar graph

**Example output:**
```
Context Window Usage:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Used: 25,432 / 200,000 tokens (12.7%)

Included Files:
  • src/app.js (3,245 tokens)
  • src/utils.js (1,856 tokens)
  • tests/app.test.js (2,134 tokens)

Conversation: 24 messages
```

> **v1.0.48 fix:** `/context` now shows the correct maximum token limit for the active model. Previously it always reported 128k regardless of the model's actual context window size.

### /compact

Compress conversation history to free up context space. Accepts an optional focus argument (v1.0.52+) to shape what the summary retains.

```
> /compact
> /compact focus on the authentication refactor decisions
```

**What it does:**
- Summarizes old messages
- Preserves important information (guided by focus argument if provided)
- Reduces token usage
- Keeps recent context intact

**When to use:**
- Context window getting full
- Long conversation with repetition
- Before starting new major task

### /fleet

Enable fleet mode for parallel subagent execution.

```
> /fleet
```

**What it does:**
- Splits complex tasks into parallel workstreams
- Multiple subagents work simultaneously on independent subtasks
- Results are synthesized when all subagents complete

**Use when:**
- Working on large tasks with independent components
- Parallelizing research or analysis across multiple files
- Running several operations (tests, file generation) concurrently

> **Full guide:** See [Fleet Mode](18-fleet-mode.md) — orchestrator model, custom agents, monitoring with /tasks, and examples.

### /tasks

View and manage background tasks including running subagents and shell sessions.

```
> /tasks
```

**Shows:**
- Active subagents and their current status
- Background shell sessions
- Task completion progress
- Option to cancel individual tasks

**Keyboard navigation in the tasks dialog:**

| Key | Action |
|-----|--------|
| `j` | Move selection down |
| `k` | Move selection up |
| `x` | Cancel / kill the selected task |

**Use when:**
- `/fleet` is active and you want to monitor progress
- Background operations are running
- Need to cancel a stuck or unwanted task

> **Related:** [Fleet Mode](18-fleet-mode.md) — full details on managing subagents.

### /remote

Remote control your CLI sessions — view and steer running sessions from another terminal or device.

```
# Show current remote control status (with actionable hints for next steps)
> /remote

# Enable remote control
> /remote on

# Disable remote control
> /remote off
```

The status output shows **actionable hints** for each connection state, telling you exactly what to do next based on the current status (v1.0.39+).

**Use cases:**
- Monitor a long-running autopilot session from a second terminal
- Steer an active session from a different machine
- Inspect session state without interrupting the active conversation

You can also pass `--remote` at startup to connect to an existing remote session immediately.

> **Related:** The `--remote` flag is listed in [CLI Flags](00-cheat-sheet.md#most-useful-command-line-flags).

## File System

### /cwd, /cd [directory]

View or change the current working directory.

```
# Show current directory
> /cwd

# Change directory
> /cwd ~/projects/my-app
> /cd /path/to/project
```

**Important:**
- AI can only access files in/below working directory
- Changes Git context if new directory is a repo
- Use absolute or relative paths
- **v1.0.65+:** The chosen directory is persisted across session resumes — reopening a session returns to the same directory automatically. Changing directory also discovers custom agents defined there.

### /add-dir <directory>

Add a directory to the allowed list for file access.

```
> /add-dir /opt/my-libs
> /add-dir ~/shared-code
> /add-dir ./src
> /add-dir ../sibling-project
```

**Use when:**
- Need to access files outside working directory
- Working with multiple project roots
- Accessing shared libraries

Relative paths (e.g., `./src`, `../sibling`) are accepted and automatically resolved to their absolute equivalents.

**Security note:** Only add trusted directories.

### /list-dirs

Display all directories the CLI can access.

```
> /list-dirs
```

**Shows:**
- Current working directory
- Additional allowed directories
- Permissions status

### /allow-all

Enable all permissions — all tools, paths, and URLs — in one command.

```
> /allow-all
```

**What it enables:**
- File system access for all paths (not just the working directory)
- All tools and shell commands
- All URL access
- Disables permission prompts for the session

> **v1.0.37+:** Individual tool approvals are automatically persisted per-directory by default. Approvals you grant in a session are remembered for future sessions in the same directory, so you do not need to re-approve the same operations each time you restart.

> **v1.0.69+:** An **auto allow-all mode** is available that auto-approves requests an LLM judge evaluates as acceptable, instead of blanket-approving everything. Enabling it via `/allow-all auto` now requires **experimental mode** (`/experimental on` or `--experimental`) — it can no longer be enabled solely with the `AUTO_APPROVAL` environment variable or feature flag.

**⚠️ Use with caution:**
- Only use in fully trusted, local-only environments
- Grants broad access; avoid with untrusted projects or shared machines
- For fine-grained control, prefer `/add-dir` and explicit tool approvals

### /yolo

Alias for `/allow-all` — enables all permissions in one command. `/yolo` state **persists across `/restart`**, so you do not need to re-enable it after restarting the session. Equivalent to the `--yolo` flag.

```
> /yolo
```

See [/allow-all](#allow-all) for the full list of what this enables.

### /permissions (v1.0.78+)

Switch between approval modes without needing to remember separate commands or flags.

```
> /permissions
```

Opens an interactive picker for the current session's approval mode — for example, the default ask-before-use behavior, or a more permissive mode equivalent to `/allow-all`.

**Use when:**
- You want a single place to check or change how much Copilot CLI can do without prompting, instead of `/allow-all`, `/reset-allowed-tools`, and CLI flags separately
- You're switching between a cautious review pass and a faster, less-interrupted pass on the same task

## Command-Line Flags

### --no-ask-user

Suppresses clarifying questions that Copilot would normally ask — the agent makes decisions autonomously rather than asking for your input. Unlike autopilot mode, this does not allow the agent to continue through multiple steps autonomously; it only skips the question-asking, not the waiting for your next prompt.

```bash
copilot --no-ask-user "Refactor src/api.js to use async/await"
```

**Comparison:**
- `--no-ask-user` — skips clarifying questions, still waits for your next prompt
- Autopilot mode — full autonomous execution end-to-end (see [Autopilot Mode](17-autopilot-mode.md))

### --continue

Resumes the most recently closed Copilot CLI session from the current working directory (falls back to the globally most recently touched session if no matching session exists), restoring context and conversation history. From v1.0.52, sessions resume in the working directory that was active when the session was last saved; pass `-C <dir>` to override. Branch and git context are also refreshed on resume.

```bash
# Resume most recent session instantly
copilot --continue

# Resume and override the working directory
copilot --continue -C /path/to/other/dir

# Or from within a session
> /resume
```

Useful when you close the terminal mid-task and want to pick up exactly where you left off.

### --name

Assigns a friendly name to the session at startup. The name can be used with `--resume` to resume the session by name rather than by ID.

```bash
# Start a named session
copilot --name "auth-refactor"

# Later, resume by name
copilot --resume auth-refactor
```

**Why it matters:** Meaningful names are easier to remember than session ID prefixes, especially for long-running work across multiple days.

## Code

### /ide

Connect Copilot CLI to an IDE workspace, giving it access to open files, editor context, and workspace information.

```
> /ide
```

**Supported IDEs:**
- Visual Studio Code
- JetBrains IDEs (IntelliJ, WebStorm, PyCharm, etc.)

**What IDE connection enables:**
- Copilot CLI can read which files you currently have open
- Access to workspace-level context (open tabs, active file, cursor position)
- Synchronized edits — changes made by Copilot CLI appear in your IDE
- Richer context for suggestions based on what you're actively working on

**Typical workflow:**
```
# 1. Open your project in VS Code
# 2. In terminal, connect CLI to the workspace
> /ide

AI: Connected to VS Code workspace: my-project
    Open files: src/app.js, src/auth.js, package.json

# 3. Now Copilot CLI has context about your open editor state
> Fix the bug in the file I currently have open

AI: I can see you have src/auth.js open. Looking at the issue on line 42...
```

**Troubleshooting:**
- Ensure the Copilot extension is installed and active in your IDE
- The IDE must be open with the same project directory
- If connection fails, try restarting both the IDE and CLI

### /diff

Review all changes made in the current directory — staged, unstaged, and new files. Uses your git config's diff tool with syntax highlighting.

```
> /diff
```

**What it shows:**
- All modified files with a summary of additions/deletions
- Syntax-highlighted diff output
- Staged vs unstaged changes

> **v1.0.64:** `/diff` now works in non-git folders, showing a session diff of all files Copilot changed during the current session.

**Navigation:** Use `j` / `k` (or ↑ / ↓) to scroll line-by-line (v1.0.47+). Vim-style jump keys also work (v1.0.60):

| Key | Action |
|-----|--------|
| `g` | Jump to top |
| `G` | Jump to bottom |
| `Ctrl+D` | Scroll down half a page |
| `Ctrl+U` | Scroll up half a page |

**Search and navigation (v1.0.62+):**

| Key | Action |
|-----|--------|
| `/` | Open search bar — type to find matches in the diff |
| `n` | Jump to next match |
| `N` | Jump to previous match |
| `w` | Toggle whitespace-only changes on/off (v1.0.63+) |

**File tree sidebar (v1.0.62+):** Press `Tab` to toggle a file tree panel showing all changed files — navigate to any file directly.

**Inline comment editor (v1.0.62+):** Select a line in the diff and press `c` to open an inline comment editor for that line.

**Pre-commit workflow:**
```
# 1. Make changes with Copilot
> Add input validation to the login endpoint

# 2. Review everything before committing
> /diff

# 3. Run code review agent
> /review

# 4. Create PR when satisfied
> /delegate Add input validation to login endpoint
```

**Tip:** Use `/diff` before `/delegate` to make sure you understand exactly what Copilot changed, especially after autopilot sessions.

```bash
# Disable rich syntax highlighting (plain diff)
copilot --plain-diff
# Or set env var permanently
export PLAIN_DIFF=true
```

### /pr

Operate on pull requests for the current branch.

```
# View PR status for the current branch
> /pr

# Create a new PR
> /pr create

# Check CI/review status
> /pr status
```

**Use when:**
- Checking the state of an open PR
- Creating a PR for the current branch directly from the CLI
- Viewing review comments or CI run results

> **v1.0.63+:** Fork-based pull requests are now shown in `/pr` and the branch PR badge, in addition to PRs from branches within the same repository.

> **v1.0.66+:** `/pr auto` starts a self-paced loop that fixes one thing per run and paces itself around CI to drive the PR to green. `/pr automerge` keeps looping until the PR is merged. Manage or stop either loop from `/loop` or `/every`.

```
# Drive the PR to green, one fix per run, paced around CI
> /pr auto

# Keep looping until the PR is merged
> /pr automerge
```

### /review

Run the code review agent on your current changes. Focuses exclusively on high-signal issues — bugs, security vulnerabilities, and logic errors — not style or formatting.

```
> /review
```

**Example output:**
```
Code Review
────────────────────────────────────────
🔴 CRITICAL: src/auth/login.js (line 42)
   SQL query built with string concatenation — SQL injection risk.
   Fix: Use parameterized queries.

🟡 WARNING: src/api/upload.js (line 18)
   No file size validation before writing to disk.
   Fix: Check Content-Length and enforce a max size.

✅ src/utils/format.js — No issues found.
```

**Review specific files:**
```
> /review @src/payments/processor.js
```

**Pre-PR workflow:**
```
> /diff      # See what changed
> /review    # Get high-signal review
> /delegate  # Create PR when clean
```

**How it differs from CI:**
- Runs locally before you push — catches issues before CI even sees them
- Focused on logic/security, not formatting (CI often checks both)
- Uses AI reasoning, not static analysis rules

### /research <query>

Run a specialized research agent that gathers information from your codebase, GitHub repositories, and the web, producing a comprehensive cited Markdown report.

```
> /research What is the architecture of this codebase?

> /research How does React implement concurrent rendering?

> /research What are best practices for rate limiting in Node.js?
```

The agent classifies your query (process / conceptual / technical deep-dive) and adapts the report format. It uses a fixed built-in model regardless of your `/model` setting. When complete, Copilot shows a summary and a link to the full Markdown report.

Press **Ctrl+Y** to open the most recent research report in your editor.

> **Full guide:** See [Research Command](19-research-command.md)

### /lsp

Manage language server configuration for enhanced code intelligence.

```
# Show LSP status
> /lsp

# Configure a language server
> /lsp configure

# Restart all language servers
> /lsp restart
```

**What it provides:**
- Symbol lookup and go-to-definition support
- Type information and inline documentation
- Diagnostics and error detection in context
- Richer code completions

**Use when:**
- Working with typed languages (TypeScript, Rust, Go, etc.)
- The AI is missing type or symbol information
- After installing new language tooling

## GitHub Integration

### /init [project-type]

Initialize a new project with scaffolding, dependencies, and configuration.

```
# Interactive: Choose from templates
> /init

# Direct: Specify project type
> /init react-app
> /init express-api
> /init python-flask
> /init nextjs
```

**What happens:**
1. AI asks clarifying questions about your project
2. Creates appropriate directory structure
3. Initializes package manager (npm, pip, etc.)
4. Sets up configuration files
5. Installs dependencies
6. Creates starter files and examples

**Supported project types:**
- `react-app` - React with Vite or Create React App
- `nextjs` - Next.js application
- `express-api` - Express.js REST API
- `python-flask` - Flask web application
- `python-django` - Django project
- `nodejs` - Basic Node.js project
- `typescript` - TypeScript project
- `rust` - Rust project with Cargo
- `go` - Go module project
- And many more...

**Example workflow:**
```
> /init react-app

AI: I'll create a new React application. A few questions:

1. Project name?
> my-awesome-app

2. Use TypeScript?
> yes

3. Include React Router?
> yes

4. Testing library (Jest/Vitest/None)?
> vitest

AI: Creating React app with TypeScript, React Router, and Vitest...

📁 Created directory structure
📦 package.json configured
⚙️  tsconfig.json created
🧪 Test setup complete
📝 README.md with instructions
⬇️  Installing dependencies...

✅ Project initialized! 
   Run: cd my-awesome-app && npm run dev
```

**Advanced usage:**
```
# Initialize in specific directory
> /cwd ~/projects
> /init nextjs

# Initialize with custom options
> /init python-flask --with-db postgres --auth jwt

# Initialize from GitHub template
> /init from gh:user/template-repo
```

**Tips:**
- Run in an empty directory or let the CLI create one
- Review generated files before committing
- Customize configuration after initialization
- Use `/plan` mode for complex project setup

### /delegate <prompt>

Create a PR in a remote repository with AI-generated changes.

```
> /delegate Add user authentication to the API
> /delegate Fix issue #123
> /delegate Refactor database connection logic
```

**What happens:**
1. AI analyzes the request and plans changes
2. Creates a new branch (auto-named or custom)
3. Makes necessary code changes
4. Commits with descriptive message
5. Pushes branch to remote
6. Creates a Pull Request with description
7. Links related issues automatically

**Requirements:**
- Must be in a Git repository
- Remote must be configured (`git remote -v`)
- GitHub authentication required (`gh auth status`)
- Write permissions on repository

**Basic example:**
```
> /delegate Fix typo in README

🔧 Creating branch: copilot-fix-readme-typo
📝 Making changes...
   ✓ Updated README.md
💾 Committing: "Fix typo in README"
⬆️  Pushing to origin/copilot-fix-readme-typo...
🔗 Created PR #42: Fix typo in README
   https://github.com/user/repo/pull/42
```

**With issue reference:**
```
> /delegate Implement user profile page from issue #156

🔍 Analyzing issue #156...
📋 Plan:
   - Create ProfilePage component
   - Add routing
   - Connect to user API
   - Add tests
   
Proceed? (y/n) y

🔧 Creating branch: feature/user-profile-156
📝 Making changes...
   ✓ src/components/ProfilePage.jsx
   ✓ src/routes.js
   ✓ tests/ProfilePage.test.js
💾 Committing: "Implement user profile page (fixes #156)"
⬆️  Pushing to origin/feature/user-profile-156...
🔗 Created PR #157: Implement user profile page
   Closes #156
   https://github.com/user/repo/pull/157
```

**Advanced usage:**

**Custom branch name:**
```
> /delegate --branch feature/new-auth Implement OAuth2 authentication
```

**Target different base branch:**
```
> /delegate --base develop Add new feature
```

> **v1.0.69+:** `/delegate` now creates the PR against your **current branch** by default (instead of the repository's default branch). Use `--base` to target a different branch, as shown above.

**Draft PR:**
```
> /delegate --draft Experimental: try new caching strategy
```

**Multiple file changes:**
```
> /delegate Update all API endpoints to use async/await
  Include error handling and logging

🔧 Creating branch: refactor/async-api
📝 Making changes...
   ✓ src/api/users.js
   ✓ src/api/products.js
   ✓ src/api/orders.js
   ✓ src/middleware/errorHandler.js
   ✓ tests/api/integration.test.js
💾 Committing: "Refactor: convert API to async/await"
⬆️  Pushing to origin/refactor/async-api...
🔗 Created PR #158: Refactor API endpoints to async/await
```

**With testing and validation:**
```
> /delegate Add input validation to login form
  Run tests before creating PR

🔧 Creating branch: fix/login-validation
📝 Making changes...
   ✓ src/components/LoginForm.jsx
   ✓ src/utils/validators.js
   ✓ tests/LoginForm.test.js
   
🧪 Running tests...
   ✓ All tests passed (24/24)
   
💾 Committing: "Add input validation to login form"
⬆️  Pushing to origin/fix/login-validation...
🔗 Created PR #159: Add input validation to login form
```

**Best practices:**
- Be specific in your prompt for better results
- Reference issue numbers to auto-link PRs
- Review the changes before approving push
- Use descriptive prompts that explain the "why"
- Test locally before using `/delegate` for critical changes

**Common use cases:**
```
# Quick fixes
> /delegate Fix broken link in footer

# Bug fixes
> /delegate Fix authentication timeout issue #234

# Features
> /delegate Add dark mode toggle to settings page

# Refactoring
> /delegate Extract database logic into repository pattern

# Documentation
> /delegate Update API docs with new endpoints

# Tests
> /delegate Add unit tests for UserService class
```

**Troubleshooting:**
- **"Not a git repository"**: Initialize git with `git init`
- **"No remote configured"**: Add remote with `git remote add origin <url>`
- **"Permission denied"**: Authenticate with `gh auth login`
- **"Branch already exists"**: Use custom branch name with `--branch`

### /worktree \<branch\> and /move \<branch\> (v1.0.61+)

Create a new git worktree and switch the active working directory into it.

```
> /worktree new-branch-name
> /move my-experiment
```

**What they do:**
1. Create a new git worktree for the specified branch (creating the branch if it doesn't exist)
2. Switch Copilot CLI's working directory to the new worktree

> **v1.0.71+: `/worktree` and `/move` are no longer aliases.** They now behave differently:
> - `/worktree` creates the new worktree and **leaves your uncommitted changes behind** in the original checkout.
> - `/move` creates the new worktree and **carries your uncommitted changes into it**.
>
> If you previously relied on `/move` (or `/worktree`) always bringing in-progress work along, double-check which command you're using — this is a breaking behavior change from earlier versions where the two commands were identical.

**Use when:**
- You want to start a fresh branch without stashing or committing in-progress work (`/move`)
- You want a clean new worktree while keeping your current checkout's uncommitted changes untouched (`/worktree`)
- Context-switching between branches mid-task
- Experimenting with a risky change without disrupting the current checkout

> **Tip (v1.0.62+):** Press `W` on the expanded issue or pull request details panel to create a worktree for that item in one keystroke.

> **v1.0.66+:** Pass a task to `/worktree` to name the branch after that task and run the task as the first prompt in the new worktree:
> ```
> > /worktree fix the login redirect
> ```
> With no argument, `/worktree` names the branch from your uncommitted changes and recent conversation using your active model. Branch names typed exactly (e.g. `feature/JIRA-123`) are kept as-is instead of being flattened to a slug (e.g. `feature-jira-123`).

> **v1.0.79+:** A new `worktreeBaseRef` setting in `settings.json` controls whether `/worktree`, `/worktree new`, and `--worktree` start from `HEAD` or the remote default branch. All three now **default to `HEAD`**; previously `--worktree` started from the remote default branch. See [.copilot Directory Guide](15-copilot-directory.md#settingsjson) for the setting.

> **v1.0.82+:** Typing a message while `/worktree` or `/move` is preparing the new worktree no longer breaks the switch into it.

### /worktree new (v1.0.79+, replaces experimental /new-worktree)

Create a new git worktree and start a **brand-new conversation** in it, rather than continuing the current one.

```
> /worktree new feature/my-branch
```

**How it differs from `/worktree`:**
- `/worktree` switches your **current conversation's** working directory into the new worktree, keeping the same chat history.
- `/worktree new` creates the worktree and opens a **fresh session** in it, leaving your current conversation and working directory untouched.

**Use when:**
- You want to start an unrelated task in its own worktree without branching off your current conversation's context
- You want to keep exploring your current task while a separate, independent conversation runs in another worktree

> **v1.0.79+:** The experimental `/new-worktree` command from v1.0.78 is now `/worktree new` — a subcommand of `/worktree` instead of a separate top-level command. Update any muscle memory or scripts that referenced `/new-worktree`.

### /app (v1.0.62+)

Open the GitHub app if it is installed, or fall back to opening your browser to the GitHub web interface.

```
> /app
```

**Use when:**
- You want to jump from the CLI to the GitHub app without navigating manually
- The app is not installed and you need the browser fallback

> **v1.0.79+:** `/app` now opens the current session directly in the GitHub Copilot desktop app (requires GitHub Copilot app 1.1.3 or later), instead of landing on the app's Home screen with the wrong folder selected.

> **v1.0.81+:** A new `copilot app` shell command opens the GitHub Copilot app in the current directory, without starting an interactive session first:
> ```bash
> copilot app
> ```

### /login

Authenticate with GitHub Copilot.

```
> /login
```

**Process:**
1. Command opens a browser-based OAuth flow (or generates a device code)
2. Browser opens automatically
3. Authorize (or enter the code) on GitHub
4. Return to CLI when authorized

> **v1.0.77+:** Browser-based (web) OAuth login is now the **default** for `copilot login`/`/login` on local interactive terminals — it opens your browser directly, skipping the device-code step. Device code login remains the default on remote/headless terminals without direct browser access. Use `--web-flow` or `--device-code` on the `copilot login` CLI command to force a mode, or choose one from the interactive `/login` command.

> **v1.0.78+:** The browser-based default also now covers local desktop subprocesses without a TTY, including IDE integrations — they get the browser flow instead of a device code. Remote and headless environments are unaffected and continue to default to device code.

> **v1.0.81+:** `copilot login --with-token` reads an auth token from stdin instead of requiring an interactive browser or device-code flow, useful for scripted and CI logins:
> ```bash
> echo "$MY_TOKEN" | copilot login --with-token
> ```

**Alternative:** Use PAT with `GH_TOKEN` environment variable.

### /logout

Sign out of GitHub Copilot.

```
> /logout
```

Clears OAuth authentication tokens and signs you out. You'll need to `/login` again.

> **Note:** `/logout` only manages OAuth sessions. If you authenticated via `gh` CLI, a Personal Access Token (PAT), an API key, or the `GH_TOKEN` environment variable, a warning is shown because `/logout` cannot remove those credentials. Remove them manually (e.g., unset `GH_TOKEN`, revoke the PAT in GitHub settings).

## Configuration

### /settings (v1.0.61+)

Open an interactive dialog to browse and edit all user settings in one place.

```
> /settings
```

**What it does:** Displays every available setting with its current value. Navigate with arrow keys, select a setting to edit it, and save on exit — no need to manually edit `~/.copilot/settings.json`.

**Why use it:**
- Discover settings you didn't know existed
- Edit values without leaving the CLI
- Safer than manual JSON editing

> **v1.0.66+:** Usage-based billing users can configure subagent concurrency and depth limits directly from `/settings`, instead of only via config files. See [Fleet Mode — Sub-Agent Depth and Concurrency Limits](08-advanced-features.md#sub-agent-depth-and-concurrency-limits-v10122).

> **v1.0.70+:** Add `--repo` or `--local` to scope a settings change to the current repository or local config instead of the global user config: `/settings --repo`. A new setting also controls whether timeline timestamps are shown or hidden.

> **v1.0.71+:** The `/settings` dashboard adds dedicated **Repo** and **Repo (local)** scope tabs, so you can browse and edit repo-scoped settings visually instead of only through the `--repo`/`--local` flags. GitHub MCP toolset and tool selections (`githubMcpToolsets`, `githubMcpTools`, and related settings) now persist via `settings.json` instead of being session-only.

> **v1.0.80+:** Unknown and ineffective settings keys are now moved into an actionable **Problems** tab within `/settings`, instead of being silently ignored — retired CLI-owned keys are also cleaned up automatically.

### /refine <prompt> (v1.0.70+)

Rewrite a rough, stream-of-consciousness prompt into a clear, well-structured one before sending it.

```
> /refine fix the thing where login sometimes doesnt work and also clean up that file its messy
```

**What it does:** Copilot restructures the input into a clear, actionable prompt (e.g., separating the bug report from the cleanup request) and shows it for confirmation before running.

**Why use it:** Skip manually rewording a messy prompt — useful when dictating or jotting down ideas quickly.

### /memory [on|off|show] (v1.0.49+)

Enable, disable, or view persistent memory. When memory is on, Copilot can store information across sessions (such as preferences or recurring context). Memory is scoped to either the user account or a specific repository.

```
# Enable memory
> /memory on

# Disable memory
> /memory off

# View current memory status and stored entries
> /memory show
```

**Memory scopes:**
- **User scope** — private to your account
- **Repository scope** — shared with repository collaborators

**How it works:** When the agent saves something to memory, a permission prompt shows exactly who can see the stored memory (e.g., `(for user)` or `(shared with repository collaborators)`). Memory entries are also annotated in the session timeline.

### /terminal-setup

Configure terminal for enhanced multiline input.

```
> /terminal-setup
```

**Enables:**
- `Shift+Enter` - New line without submitting
- `Ctrl+Enter` - Submit multiline input
- Better text editing experience

**Supported terminals:**
- iTerm2 (macOS)
- Terminal.app (macOS)
- Windows Terminal
- Most modern terminal emulators

**Troubleshooting by terminal:**

| Terminal | Common issue | Fix |
|----------|-------------|-----|
| **iTerm2** | Shift+Enter submits instead of newlines | Run `/terminal-setup` — it patches your iTerm2 profile |
| **Terminal.app** | Multiline not working | `/terminal-setup` modifies key bindings; may need restart |
| **Windows Terminal** | `Ctrl+Enter` not responding | Ensure PowerShell v6+ and run `/terminal-setup` |
| **tmux** | Key sequences not passing through | Add `set -g xterm-keys on` to `~/.tmux.conf` |
| **VS Code terminal** | Modifier keys intercepted | Use the external terminal or disable conflicting VS Code keybindings |
| **SSH sessions** | No effect | `/terminal-setup` configures the local terminal; run it locally before SSH |

If multiline still doesn't work after setup, restart the terminal emulator and relaunch `copilot`.

### /theme [show|set|list] [theme]

Manage the CLI visual theme.

```
# View current theme
> /theme show

# List available themes
> /theme list

# Set specific theme
> /theme set dark
> /theme set light
> /theme set auto
```

**Themes:**
- `auto` - Follow system preference
- `dark` - Dark mode
- `light` - Light mode

### /statusline [item]

Customize which items appear in the status bar at the bottom of the CLI. Also available as `/footer`.

```
# Show current status bar configuration
> /statusline

# Toggle individual items on or off
> /statusline directory
> /statusline branch
> /statusline effort
> /statusline context
> /statusline quota
> /statusline username
```

**Available items:**

| Item | What it shows |
|------|---------------|
| `directory` | Current working directory |
| `branch` | Active git branch |
| `effort` | Effort level indicator |
| `context` | Context window usage |
| `quota` | Premium request quota remaining |
| `changes` | Lines added/removed in the current session (v1.0.36+) |
| `username` | Active GitHub account name (v1.0.43+) |
| `ci` | CI check status for the current branch: passing/running/failing (v1.0.65+, opt-in) |
| `pr` | Link to the current pull request for the active branch (v1.0.66+) |

**Use when:** You want to declutter the status bar or focus on specific metrics during your workflow.

### /reset-allowed-tools

Reset the list of tools the AI can use.

```
> /reset-allowed-tools
```

**When to use:**
- After restricting tools for safety
- To restore full functionality
- Troubleshooting permission issues

### Tool Permission System

Copilot CLI uses an explicit permission model. By default, the agent asks before using tools that modify files or execute commands. You can pre-approve or deny specific tools using flags.

#### Permission flags

| Flag | Purpose |
|------|---------|
| `--allow-all` | Allow all tools, paths, and URLs (alias: `--yolo`) |
| `--allow-tool=PATTERN` | Pre-approve specific tools |
| `--deny-tool=PATTERN` | Always deny specific tools (takes precedence over allow) |
| `--allow-all-paths` | Allow file access to any path |
| `--allow-all-tools` | Allow all tools without confirmation |
| `--allow-all-urls` | Allow all URL access |

#### Permission patterns

Patterns use the format `Kind(argument)` — the argument is optional (omitting it matches all tools of that kind):

| Pattern | Matches |
|---------|---------|
| `shell` | All shell commands |
| `shell(git push)` | Only `git push` |
| `shell(git:*)` | All git commands (`git push`, `git pull`, etc.) |
| `write` | All file writes |
| `write(src/*.ts)` | Writes to .ts files in src/ only |
| `read(.env)` | Reads of .env files |
| `MyMCP(create_issue)` | Specific MCP server tool |
| `url(github.com)` | Access to github.com |

> **Note:** Deny rules always take precedence over allow rules, even when `--allow-all` is set.

#### Examples

```bash
# Allow all git commands except git push
copilot --allow-tool='shell(git:*)' --deny-tool='shell(git push)'

# Full permissions for autopilot sessions
copilot --allow-all --max-autopilot-continues 20

# Allow a specific MCP tool only
copilot --allow-tool='MyMCP(create_issue)'

# Restrict to read-only (no writes or shell execution)
copilot --deny-tool='write' --deny-tool='shell'
```

#### In-session permission commands

```
> /allow-all          # Grant all permissions for this session
> /reset-allowed-tools  # Reset tool permissions to default
> /list-dirs          # Show which directories Copilot can access
> /add-dir PATH       # Add a directory to the allowed list
```

### /instructions

View and toggle custom instruction files.

```
# View all instruction files and their status
> /instructions

# Toggle an instruction file on or off
> /instructions toggle <filename>
```

**What it shows:**
- Active instruction files loaded into context
- Available instruction files in `.copilot/`
- Enable/disable individual files without deleting them

**Instruction files provide:**
- Project-specific coding conventions
- Style guidelines and preferred patterns
- Behavioral preferences for the AI

> **v1.0.81+:** `/instructions` now shows each user instruction file separately, rather than grouping them, making it clearer which specific file is active or disabled.

See [Copilot Directory Guide](15-copilot-directory.md) for setup details.

### /streamer-mode

Toggle streamer mode to hide sensitive details during live streaming or screen sharing.

```
> /streamer-mode
```

**What it hides:**
- Preview model names
- Quota details and usage statistics
- Other potentially sensitive session metadata

**Use when:**
- Screen sharing or live streaming your terminal
- Presenting to an audience
- You don't want quota or model details visible on screen

### /plugin [subcommand]

Manage plugins and plugin marketplaces.

```
# List installed plugins
> /plugin list

# Browse the plugin marketplace
> /plugin marketplace

# Install a plugin
> /plugin install <name>

# Remove a plugin
> /plugin remove <name>

# Add a custom marketplace
> /plugin marketplace add <url>

# Update all plugins (v1.0.49+)
> /plugin update --all

# Update a specific plugin
> /plugin update

# Refresh marketplace catalogs from inside a session (v1.0.80+)
> /plugin marketplace update
> /plugin marketplace update <name>
```

> **v1.0.81+:** The plugins dashboard opened by `/plugin` (as well as bare `/mcp` and `/skills`) is now available to everyone. The `PLUGINS_DASHBOARD` environment variable opt-out and the legacy skills picker it kept alive have been removed — these commands always open the dashboard (`/mcp config` still opens the dedicated MCP wizard). The standalone `/plugins` command has been removed; its resources are covered by `/plugin`, `/mcp`, and `/skills`, with `/subagents` for agents and `/instructions` for instructions. `/plugin` now also flags installed plugins and marketplaces that have a newer version upstream and offers an **Update** action to pull it.

> **v1.0.80+:** `/plugin marketplace update [name]` refreshes marketplace catalogs directly from within an interactive session, without needing to drop to the shell — omit `name` to refresh all configured marketplaces, or pass one to refresh just that marketplace.

> ⚠️ **Removed in v1.0.81:** The `/plugins` command has been removed. Use `/plugin`, `/mcp`, and `/skills` for plugin, MCP server, and skill management, `/subagents` for agents, and `/instructions` for instructions.

> **v1.0.69+:** A `/plugins` dashboard was available for managing installed plugins, and installed plugin extensions could be reloaded without restarting the session. (Superseded — see the v1.0.81 note above.)

> **v1.0.70+:** Pin a plugin to an exact commit using the `sha` field in its plugin source configuration, so the plugin version stays fixed even if the source ref moves.

> **v1.0.71+:** New `copilot plugins marketplace` CLI subcommands manage plugin marketplaces directly from the shell:
> ```bash
> copilot plugins marketplace list
> copilot plugins marketplace add <url>
> copilot plugins marketplace remove <name>
> copilot plugins marketplace browse
> copilot plugins marketplace update
> ```

> **v1.0.72+:** `/plugins` (and `copilot plugins`) gains `update` and `uninstall` verbs, and `enable`, `disable`, and `remove` now accept `--plugin`, `--mcp`, or `--skill` flags — or a positional kind — to target plugins, MCP servers, or skills through the same set of subcommands. Skills can now be installed and removed the same way:
> ```bash
> copilot plugins install --skill <file-path-or-url-or-directory>
> copilot plugins install --skill ./my-skill.skill.md --scope project
> copilot plugins remove --skill my-skill
> copilot plugins update --skill my-skill
> copilot plugins enable --mcp my-server
> copilot plugins disable --plugin my-plugin
> ```
> A `/plugins help` command documents the full set of subcommands and flags.

**Shell command — refresh plugin catalogs without starting a session:**

```bash
copilot plugin marketplace update
```

**Plugins extend CLI capabilities with:**
- New slash commands
- Specialized domain tools
- Third-party integrations
- Custom workflows and automations

**Example plugins:**

| Plugin | What it adds |
|--------|-------------|
| `copilot-jira` | `/jira` — link PRs to Jira tickets, view issues |
| `copilot-datadog` | Query metrics and logs from the CLI |
| `copilot-terraform` | Terraform plan/apply assistance |
| `copilot-k8s` | Kubernetes cluster management |

**Plugin structure** (for plugin authors): Plugins are npm packages that export a manifest declaring new slash commands, tools, and instructions. Publish to the npm registry or host a private marketplace JSON registry and add it with `/plugin marketplace add <url>`.

## Information

### /ask <question>

Ask a quick question without adding it to your conversation history. The question and its answer are ephemeral — your next regular prompt resumes from the conversation state before the `/ask`.

Responses render full markdown, including tables, code blocks, and formatted links (v1.0.37+).

```
> /ask What does the --allow-all flag do?
> /ask How many tokens does a typical file read use?
```

**Use when:**
- You want a quick lookup or clarification that shouldn't influence the conversation context
- You're mid-task and need a side-question answered without derailing the thread
- You want to check facts without the model carrying the question forward

> **v1.0.48 fix:** The `/ask` dialog no longer displays a follow-up input prompt after answering. It closes cleanly once the answer is shown.

### /env

Show all loaded environment details for the current session.

```
> /env
```

**Shows:**
- Active instruction files
- Connected MCP servers and their status
- Loaded skills
- Available agents
- Installed plugins
- Hook counts and source provenance for each active hook (v1.0.60); internal hooks are hidden and full file paths are shown for hook sources (v1.0.61)

**Why use it:** Quickly audit everything the CLI has loaded before starting a task — especially useful when debugging unexpected behaviour or verifying that MCP servers and skills are connected correctly.

### /help

Display help information and available commands.

```
> /help
> /help billing
```

**Shows:**
- All slash commands
- Keyboard shortcuts
- Quick reference guide
- Links to documentation
- User-level instruction locations, including `$HOME/.copilot/instructions/**/*.instructions.md` (v1.0.61)

**Help topics (v1.0.60):** Pass a topic name to get a focused overview:

| Topic | Description |
|-------|-------------|
| `billing` | Overview of AI credit usage, quota limits, and links to `/usage` and `/mcp` |

> **Tip:** Typing `help` at the prompt (without the `/`) also opens the quick-help overlay (v1.0.60).

### /usage

Display session usage metrics and statistics, including a GitHub-style contribution graph of your usage history (v1.0.35+) and quota progress bars for session and weekly limits (v1.0.52+).

```
> /usage
```

**Shows:**
- Requests used this session
- Monthly premium requests
- Requests remaining (with visual progress bars)
- Weekly quota progress bar
- Token consumption including cache read and cache write tokens (v1.0.60)
- Cost estimates (if applicable)
- Contribution graph of usage history (adapts to terminal color mode)

**Example output:**
```
Usage Statistics:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Session:
  Requests: 12
  Tokens: 45,231

Monthly Quota:
  Used: 134 / 500 requests
  Remaining: 366 requests
  Resets: January 28, 2026
```

### /limits

Manage and predict AI-credit session limits.

```
> /limits predict
```

**v1.0.76+:** `/limits predict` suggests a session AI-credit limit based on similar past sessions (same repo, similar task size), helping you set a realistic cap before starting a long-running or autopilot task.

### /user [show|list|switch]

Manage GitHub user accounts.

```
# Show current user
> /user show

# List all authenticated users
> /user list

# Switch between users
> /user switch
```

**Use cases:**
- Multiple GitHub accounts
- Switching between personal/work accounts
- Team collaboration

### /changelog, /release-notes [summarize]

Display the changelog for CLI versions. `/release-notes` is an alias for `/changelog`.

```
# View raw changelog
> /changelog

# Get an AI-generated summary of recent changes
> /changelog summarize
```

**Shows:**
- Recent version history
- New features and enhancements
- Bug fixes
- Breaking changes

**With `summarize`:** The AI provides a concise digest of what changed, highlighting the most important updates since your last version.

### /chronicle

View a **narrative history** of what the current session has done — file edits, commands run, and key decisions — formatted as a readable summary. Use the `search` subcommand to query all session content by keyword or topic (v1.0.49+). Use `cost-tips` for personalized token cost recommendations (v1.0.51+). Use `skills review` to review proposed draft skill changes (v1.0.66+).

```
> /chronicle

# Search all session content by keyword or topic (v1.0.49+)
> /chronicle search <query>
> /chronicle search "database migration"

# Get personalized token usage and cost reduction tips (v1.0.51+)
> /chronicle cost-tips

# Review proposed draft skill changes (v1.0.66+)
> /chronicle skills review
```

**Use when:**
- Writing a commit message for a long session
- Reviewing what was changed before opening a pull request
- Handing off work to a colleague
- Finding past decisions or context with a keyword search
- Deciding whether to accept, reject, or defer a draft skill change proposed during the session

> **v1.0.40+:** Session history, file tracking, and `/chronicle` are available to all users.

> **v1.0.66+:** `/chronicle skills review` lets you step through each proposed draft skill change and accept, reject, or defer it individually.

### /update, /upgrade

Update the CLI to the latest version. `/upgrade` is an alias for `/update`.

```
> /update
```

Add the optional `prerelease` argument to install the latest prerelease build (v1.0.44+):

```
> /update prerelease
```

> **v1.0.71+:** `copilot update` and `/update` also accept `stable` as an explicit channel argument.
> ```
> > /update stable
> ```

**What it does:**
1. Checks for the latest available release
2. Downloads and installs the update
3. Restarts with the updated binary

**Use when:**
- You see an "Update available" notification
- You want the latest features or bug fixes

### /version

Display version information and check for available updates.

```
> /version
```

**Shows:**
- Current CLI version number
- Release date
- Whether a newer version is available
- Download URL if an update exists

### /experimental [enable|disable|list]

Show available experimental features, or enable/disable experimental mode.

```
# List experimental features
> /experimental

# Enable experimental mode
> /experimental enable

# Disable experimental mode
> /experimental disable

# Enable a specific experimental feature
> /experimental enable <feature-name>
```

**Experimental features:**
- Early access to new capabilities before general availability
- May change behaviour or be removed in future releases
- Feedback via `/feedback` is encouraged

**Use when:**
- You want cutting-edge features
- You're willing to accept potential instability
- You want to contribute feedback during a feature's development

> **Autopilot full guide:** See [Autopilot Mode](17-autopilot-mode.md) — permissions, --max-autopilot-continues, plan→autopilot workflow, and examples.

### /autopilot [objective] (v1.0.45+)

Toggle autopilot mode on or off directly, without cycling through modes with Shift+Tab. In v1.0.55+, you can supply an optional objective to keep the autonomous session focused.

```
> /autopilot
> /autopilot Refactor the payments module to use the new billing API
```

**`/goal` is an alias** for `/autopilot <objective>`:

```
> /goal Add end-to-end tests for the checkout flow
```

**What it does:**
- Switches from interactive mode directly into autopilot mode (or back)
- When an objective is given, the model is anchored to that goal throughout the session
- Equivalent to pressing Shift+Tab until autopilot is reached, but in one command

**Use when:**
- You want a quick way to enter or exit autopilot without pressing Shift+Tab multiple times
- You want to constrain a long autopilot run to a specific task

> **v1.0.80+:** Setting an explicit objective with `/autopilot <objective>` (or `/goal <objective>`) no longer requires experimental mode — you can anchor an autopilot run to a goal without first running `/experimental enable`. Entering autopilot mode itself is still gated behind experimental mode.

> **Full guide:** See [Autopilot Mode](17-autopilot-mode.md) for permissions, continuation limits, and examples.

## Advanced Features

### /plan [prompt]

Enter plan mode to create an implementation plan before coding.

```
# Enter plan mode with prompt
> /plan Create a REST API with authentication

# Or just enter plan mode
> /plan
```

**What happens:**
1. AI analyzes requirements
2. Creates detailed plan
3. Saves to plan.md
4. Waits for approval to execute

**Plan includes:**
- Problem statement
- Approach description
- Task checklist
- Implementation notes

See [Plan Mode Guide](09-plan-mode.md) for details.

### /agent

Browse, select, and invoke custom agents — specialized versions of Copilot optimized for particular types of work.

```
> /agent
```

**Built-in agents:**

| Agent | Description |
|-------|-------------|
| `explore` | Fast codebase analysis — ask questions without adding to main context |
| `task` | Run commands (tests, builds, lints) — brief summary on success, full output on failure |
| `general-purpose` | Complex multi-step tasks in a separate context window |
| `code-review` | High-signal code review — bugs, security, logic errors only |

**Selecting an agent:**
```
> /agent

Available agents:
  1. explore         — Codebase analysis
  2. task            — Command execution
  3. general-purpose — Complex tasks
  4. code-review     — Code review
  5. @test-writer    — Custom: write unit tests (if configured)

Select: _
```

**Using agents in fleet mode:**
```
> /fleet Use @test-writer to add tests for src/services/,
         use @doc-generator to add JSDoc to src/utils/
```

**Using a specific agent for a prompt:**
```
> /agent explore
> What are all the API endpoints in this codebase?
```

> **See also:** [Fleet Mode](18-fleet-mode.md) for using custom agents with parallel subagent execution.

> **v1.0.61:** Press `/` in the `/agent` picker to filter agents by name. Number keys (1–9 and beyond) select agents directly from the list.

### /subagents (alias /agents) (v1.0.62+)

Open the subagents picker to configure how background subagents behave — choose their model, reasoning effort level, and context tier. You can also set these preferences in user settings via `/settings`.

```
> /subagents
> /agents
```

**Configurable options:**

| Option | Description |
|--------|-------------|
| Model | The AI model subagents use (e.g., `claude-haiku-4.5` for speed, `claude-opus-4.8` for quality) |
| Reasoning effort | `low`, `medium`, `high`, or `max` — controls reasoning depth vs. cost |
| Context tier | `default` or `long_context` — use long context for tasks involving large files or codebases |

**Why it matters:** Reduce costs for routine background tasks by configuring a fast/cheap model for subagents while keeping a powerful model for your main session.

> **v1.0.71+:** Reopening the `/subagents` model picker keeps each agent's previously configured reasoning effort and context tier instead of resetting them. The default maximum sub-agent nesting depth is now 4 (down from 6); usage-based billing users can raise `subagents.maxDepth` up to 128.

### /security-review (v1.0.51+)

Run a dedicated security review agent on your current changes. Unlike `/review`, which covers general code quality, `/security-review` focuses exclusively on security vulnerabilities — injection risks, authentication flaws, secrets exposure, and related concerns.

```
> /security-review
```

**Use when:**
- Preparing a PR that touches authentication, authorization, or data handling
- You want a security-focused second pass before merging sensitive changes
- The task involves user input, external APIs, or secret management

> **v1.0.64:** `/security-review` is now available to all users — the `--experimental` flag is no longer required.

### /rubber-duck (v1.0.49+)

Invoke the rubber-duck agent for an independent critique of the agent's current work. The rubber-duck agent reviews what has been done so far and provides fresh perspective, surfacing blind spots, potential issues, or alternative approaches.

```
> /rubber-duck
```

**Use when:**
- The agent seems stuck or is going in circles
- You want a second opinion on the current approach before continuing
- A complex task is nearing completion and you want a sanity check

> **v1.0.58:** Rubber Duck is now **enabled by default** for all users — no experimental flag needed.

> **v1.0.56:** The rubber-duck agent can be enabled or disabled via the `builtInAgents.rubberDuck` setting in `~/.copilot/settings.json` or `copilot config`.

### /mcp [subcommand] [server-name]

Manage MCP (Model Context Protocol) server configuration.

```
# Show MCP servers
> /mcp show

# Add new server
> /mcp add my-server

# Edit server config
> /mcp edit my-server

# Delete server
> /mcp delete my-server

# Disable server (persists across sessions)
> /mcp disable my-server

# Re-enable server (persists across sessions)
> /mcp enable my-server

# Search and install MCP servers from the registry (v1.0.49+)
> /mcp search <query>
> /mcp registry   # browse registry interactively (v1.0.64+)

# List attached MCP servers and their status (v1.0.69+)
> /mcp list
```

> **v1.0.66+:** The `/mcp show` list view now includes a toggle to enable or disable each server directly, in addition to the `/mcp enable`/`/mcp disable` commands.

> **v1.0.69+:** `/mcp list` shows attached MCP servers and their status. Both `/mcp list` and `/plugin list` can run while the agent is working. The `/mcp` manager can also be opened mid-turn to enable or disable servers; adding, editing, deleting, and re-authenticating a server still wait until the current turn finishes.

> **v1.0.70+:** `/mcp list` marks locally-spawned MCP servers that run inside the sandbox, e.g. `connected (sandboxed)`, so you can tell at a glance which servers are sandbox-isolated.

> **v1.0.81+:** Bare `/mcp` and `/mcp show` (with no server name) now always open the unified plugins dashboard (`/mcp config` still opens the dedicated MCP wizard) — the `PLUGINS_DASHBOARD` opt-out has been removed. The CLI, SDK, IDE, and in-memory clients now support the MCP 2026-07-28 protocol revision. On Windows, remote MCP servers protected by Microsoft Entra ID can sign in through the OS authentication broker (WAM), usually with no prompt; other platforms, `--device-code`, and machines without the broker library keep the browser flow.

**MCP Servers extend CLI capabilities:**
- Database access
- API integrations
- Custom tools
- Domain-specific knowledge

See [Advanced Features](08-advanced-features.md) for MCP details.

### /skills [subcommand] [args]

Manage skills for enhanced capabilities. `/skill` is an alias for `/skills` (v1.0.65+).

```
# List available skills
> /skills list

# Get skill info
> /skills info <skill-name>

# Add a skill
> /skills add <skill-name>

# Remove a skill
> /skills remove <skill-name>

# Reload skills
> /skills reload
```

You can also manage skills from the shell without launching an interactive session using the `copilot skill` subcommand (v1.0.65+):

```bash
$ copilot skill list
$ copilot skill add python-expert
$ copilot skill remove python-expert
```

> **v1.0.71+:** `copilot skill list` and its JSON output now mark disabled skills, matching the `/skills list` picker in the interactive session.

> **v1.0.81+:** Skills (and custom agents) are now also discovered from directories added with `--add-dir`, in addition to the default skill/agent locations. `/skills` always opens the unified plugins dashboard — the legacy standalone skills picker has been removed.

**Skills are:**
- Modular expertise packages (e.g., "Python expert", "React patterns")
- Reusable across any project
- Can be enabled/disabled as needed
- Domain-focused specialized knowledge
- Different from AGENTS.md (project-specific) and instructions (style guides)

**Example use cases:**
```
> /skills add python-expert
# Now get Python best practices

> /skills add security-audit  
# Audit code for vulnerabilities

> /skills add react-patterns
# Get React component guidance
```

**For comprehensive details, see [Skills System Guide](14-skills-system.md).**

### /share, /export [file|gist|html] [path]

Share session to markdown file, GitHub Gist, or self-contained interactive HTML file. `/export` is an alias for `/share`.

```
# Share to file
> /share file session.md

# Share to specific path
> /share file ~/Documents/my-session.md

# Share to GitHub Gist
> /share gist

# Export as interactive HTML (v1.0.15+)
> /share html
> /share html ~/reports/session.html
```

**File output includes:**
- Full conversation
- Code snippets
- File changes
- Timestamps
- Formatted markdown

**Gist output:**
- Creates public gist
- Returns shareable URL
- Includes all session content

**HTML output:**
- Self-contained interactive HTML file
- No external dependencies
- Shareable without a GitHub account
- Displays a `file://` URL so you can open the file directly
- Supports `Ctrl+X O` to open the HTML file immediately from the CLI

**File extension handling:** If you provide a custom output path without a file extension, `/share` automatically appends `.md` (for file/gist output) or `.html` (for HTML output).

### /feedback, /bug

Submit feedback about the CLI. `/bug` is an alias for `/feedback`.

```
> /feedback
```

Opens confidential feedback survey in your browser. If the current working directory is not writable, the diagnostic bundle is saved to your system `TEMP` directory instead.

**Feedback helps improve:**
- Feature priorities
- Bug fixes
- User experience
- Documentation



### /keep-alive

Prevent your system from going to sleep while Copilot CLI is active. Available without experimental mode (v1.0.36+).

```
> /keep-alive
```

**Use when:**
- Running long autopilot or fleet tasks on a laptop
- You need the session to stay active overnight or during extended operations

**Note:** System sleep inhibition is released automatically when the CLI exits or when you run `/keep-alive` again to toggle it off.

### /restart

Restart the CLI while preserving the current session. From v1.0.52, the session ID is preserved across restarts.

```
> /restart
```

**Use when:**
- The CLI is behaving unexpectedly or feels stuck
- You just ran `/update` and want to load the new version
- You need to reload configuration or plugins
- Recovering from a frozen or unresponsive state

**Note:** Session data and session ID are preserved and automatically restored after restart.

### /exit, /quit

Exit the CLI. Use the `print` option to print the full session to the terminal before exiting (v1.0.49+).

```
> /exit
> /quit

# Print the session to the terminal before exiting (v1.0.49+)
> /exit print
```

**Alternative:** Press `Ctrl+D` to quickly shutdown (does not queue a message). Use `Ctrl+Q` or `Ctrl+Enter` to queue a message while the agent is running.

**Note:** Sessions are automatically saved and can be resumed later.

### /voice (v1.0.59+)

Dictate a prompt using local speech-to-text. Copilot CLI records audio, transcribes it locally, and populates the prompt field with the result.

```
> /voice
```

After transcription completes, review and optionally edit the text, then submit.

**Why it matters:** Hands-free prompt entry — useful for long, natural-language prompts or accessibility.

> **v1.0.71+:** `/voice devices` lets you choose which microphone to use and persists the selection across sessions.
> ```
> > /voice devices
> ```

### /every \<interval\> \<prompt\> (experimental, v1.0.58+)

Repeat a prompt automatically at a fixed interval.

> **v1.0.64:** `/loop` is now an alias for `/every`.

```
> /experimental on
> /every 10m check for new GitHub notifications and summarize them
> /loop 10m check for new GitHub notifications and summarize them   # alias (v1.0.64+)
> /every 1h run the test suite and report failures
```

**Intervals:** use `s` (seconds), `m` (minutes), or `h` (hours).

**Natural language (v1.0.61+):** cron expressions, calendar times, and relative durations are also accepted:

```
> /every "every weekday at 9am" summarize open PRs
> /every "0 */4 * * *" run the integration tests
```

**Schedule slash commands (v1.0.62+):** Pass a slash command as the prompt to run it on a schedule:

```
> /every 1d /chronicle standup
> /every "every Monday at 8am" /research open issues summary
```

> ⚠️ Experimental — enable with `/experimental on`.

> **Tip:** Set `"beepOnSchedule": false` in `~/.copilot/settings.json` to suppress the completion beep (v1.0.61).

### /after \<delay\> \<prompt\> (experimental, v1.0.58+)

Run a prompt once after a specified delay.

```
> /experimental on
> /after 30m remind me to commit my changes
> /after 1h summarize what I've done this session
```

**Delays:** use `s` (seconds), `m` (minutes), or `h` (hours).

**Natural language (v1.0.61+):** relative durations and calendar times are also accepted:

```
> /after "in 2 hours" remind me to deploy the staging build
> /after "tomorrow at 9am" summarize open issues
```

**Schedule slash commands (v1.0.62+):** Pass a slash command as the prompt:

```
> /after 2h /review
```

> ⚠️ Experimental — enable with `/experimental on`.

### /diagnose (v1.0.64+)

Analyze the current session's logs to surface errors, warnings, and actionable insights about what happened during the session.

```
> /diagnose
```

**Use when:**
- A session produced unexpected or incorrect results
- You are troubleshooting tool failures or agent behavior
- You want a summary of what operations ran and whether they succeeded

## Command Patterns

### Combining Commands

Commands can be chained in conversation:

```
> /model claude-opus-4.5
> Create a complex algorithm

[AI uses Opus model for complex task]

> /model claude-haiku-4.5  
> Format the code

[AI uses Haiku model for simple formatting]
```

### Command Context

Some commands affect subsequent prompts:

```
> /cwd ~/projects/backend
> Create a new route
[Creates route in backend project]

> /cwd ~/projects/frontend
> Create a new component
[Creates component in frontend project]
```

## Tips for Using Slash Commands

### Efficiency Tips

✅ **Use shortcuts:** `/h` often works for `/help`  
✅ **Tab completion:** Type `/mod` and tab for `/model`  
✅ **Chain actions:** Use commands to set up before prompting  
✅ **Check context:** Run `/context` before long tasks  

### Common Workflows

**Starting a new task:**
```
> /clear          # Fresh start
> /cwd ~/project  # Set location
> /model claude-sonnet-4.5  # Choose model
```

**Wrapping up:**
```
> /usage          # Check quota
> /share file session.md  # Save work
> /exit           # Close CLI
```

**Troubleshooting:**
```
> /context        # Check memory
> /compact        # Free space
> /reset-allowed-tools  # Reset permissions
```

## Quick Reference Table

| Command | Purpose | Example |
|---------|---------|---------|
| `/clear` | New conversation | `/clear` |
| `/rename` | Rename session | `/rename "My Work"` |
| `/rewind` | Undo last turn + file changes | `/rewind` |
| `/copy` | Copy last response | `/copy` |
| `/model` | Change AI model | `/model claude-sonnet-4.5` |
| `/fleet` | Parallel subagents | `/fleet` — see [Fleet Mode](18-fleet-mode.md) |
| `/tasks` | View background tasks | `/tasks` |
| `/diff` | Review changes | `/diff` |
| `/pr` | Operate on PRs | `/pr create` |
| `/review` | Code review agent | `/review` |
| `/security-review` | Security-focused code review (v1.0.51+) | `/security-review` |
| `/ide` | Connect to IDE | `/ide` |
| `/cwd` | Change directory | `/cwd ~/projects` |
| `/allow-all` | Enable all permissions | `/allow-all` |
| `/yolo` | Alias for `/allow-all`; persists across `/restart` | `/yolo` |
| `/permissions` | Switch between approval modes (v1.0.78+) | `/permissions` |
| `/context` | Check memory | `/context` |
| `/compact` | Compress history | `/compact` |
| `/plan` | Create plan | `/plan Build API` |
| `/research <query>` | Deep investigation report | `/research How does auth work?` |
| `/delegate` | Create PR via AI | `/delegate Fix issue #123` |
| `/usage` | Check quota | `/usage` |
| `/changelog` | View changelog | `/changelog summarize` |
| `/update` | Update CLI | `/update` |
| `/version` | Show version | `/version` |
| `/experimental` | Experimental features | `/experimental list` |
| `/instructions` | Toggle instructions | `/instructions` |
| `/streamer-mode` | Hide sensitive info | `/streamer-mode` |
| `/statusline` | Customize status bar items | `/statusline quota` |
| `/plugin` | Manage plugins | `/plugin list` |
| `/memory` | Enable, disable, or view persistent memory (v1.0.49+) | `/memory show` |
| `/rubber-duck` | Get independent critique of current work (v1.0.49+; default on v1.0.58+) | `/rubber-duck` |
| `/security-review` | Security-focused code review (v1.0.51+) | `/security-review` |
| `/help` | Show help | `/help` |
| `/share` | Export session | `/share file out.md` |
| `/restart` | Restart CLI | `/restart` |
| `/exit` | Quit CLI; add `print` to print session first (v1.0.49+) | `/exit` |
| `/voice` | Dictate a prompt with local speech-to-text (v1.0.59+); `/voice devices` to choose/persist the microphone (v1.0.71+) | `/voice` |
| `/every` | Repeat a prompt on a schedule (experimental, v1.0.58+) | `/every 10m check issues` |
| `/loop` | Alias for `/every` (v1.0.64+) | `/loop 10m check issues` |
| `/after` | Run a prompt after a delay (experimental, v1.0.58+) | `/after 1h remind me` |
| `/settings` | Browse and edit all user settings interactively (v1.0.61+) | `/settings` |
| `/worktree` | Create a new git worktree and switch into it, leaving uncommitted changes behind (v1.0.61+; behavior split from `/move` in v1.0.71+) | `/worktree my-branch` |
| `/move` | Create a new git worktree and switch into it, carrying uncommitted changes along; no longer an alias for `/worktree` (v1.0.71+) | `/move my-branch` |
| `/worktree new` | Create a new git worktree and start a fresh conversation in it (v1.0.79+, replaces experimental `/new-worktree`) | `/worktree new my-branch` |
| `/app` | Open the current session in the GitHub Copilot desktop app, or browser fallback (v1.0.62+; opens directly to the session in v1.0.79+) | `/app` |
| `/subagents` | Configure subagent model, reasoning effort, and context tier (v1.0.62+); alias `/agents` | `/subagents` |
| `/branch` | Fork the current session into a new independent session (v1.0.64+); alias for `/fork` | `/branch my-experiment` |
| `/diagnose` | Analyze session logs to surface errors and insights (v1.0.64+) | `/diagnose` |

## Hidden Commands

Some commands are less commonly used but powerful:

```
> /compact                    # Compress history
> /reset-allowed-tools        # Reset permissions
> /list-dirs                  # Show allowed directories
> /session checkpoints        # View history points
> /user switch                # Change GitHub account
> /mcp show                   # View MCP servers
> /skills list                # Available skills
```

## Command Aliases

Some commands have shorter aliases:

```
/new           = /clear
/reset         = /clear
/cd            = /cwd
/undo          = /rewind
/footer        = /statusline
/continue      = /resume
/upgrade       = /update
/release-notes = /changelog
/bug           = /feedback
/export        = /share
/h             = /help (if supported)
/q             = /quit (if supported)
/agents        = /subagents
/branch        = /fork
/loop          = /every
/skill         = /skills
```

## Error Messages

If a command fails or is unrecognized, the command picker suggests the closest matching commands:

```
> /changelg
  Did you mean: /changelog?
```

If the command is simply unknown:

```
> /invalid-command

❌ Unknown command: /invalid-command
   Type /help to see available commands
```

If a command needs arguments:

```
> /add-dir

❌ Missing required argument: directory
   Usage: /add-dir <directory>
```

## Practice Exercises

Try these to master slash commands:

### Exercise 1: Session Management
```
1. /session                    # Check current session
2. /session rename "Practice"  # Rename it
3. /clear                      # Start fresh
4. Create a test file
5. /session checkpoints        # View history
```

### Exercise 2: Model Switching
```
1. /model                      # See options
2. /model claude-haiku-4.5    # Switch to fast model
3. Format some code           # Quick task
4. /model claude-opus-4.5     # Switch to powerful model
5. Solve complex problem      # Complex task
```

### Exercise 3: Context Management
```
1. Mention several files with @
2. /context                    # Check usage
3. Have a long conversation
4. /context                    # Check again
5. /compact                    # Compress
6. /context                    # Verify reduction
```

---

**Next:** [File and Context Management](05-file-context.md)  
**Previous:** [Interactive Features](03-interactive-features.md)
