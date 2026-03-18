# Latest Features in GitHub Copilot CLI

This guide covers the latest and most recently added features in GitHub Copilot CLI, from experimental modes and parallel execution to LSP integration and extended instruction support.

## Table of Contents

1. [Autopilot Mode (Experimental)](#autopilot-mode-experimental)
2. [Fleet Mode (`/fleet`)](#fleet-mode-fleet)
3. [Research Command (`/research`)](#research-command-research)
4. [LSP Support](#lsp-language-server-protocol-support)
5. [Code Review Agent (`/review`)](#code-review-agent-review)
6. [Plugin System (`/plugin`)](#plugin-system-plugin)
7. [New Keyboard Shortcuts](#new-keyboard-shortcuts)
8. [PAT Authentication](#pat-authentication)
9. [Extended Instructions Support](#extended-instructions-support)
10. [Project Initialization (`/init`)](#project-initialization-init)
11. [Enhanced Pull Request Creation (`/delegate`)](#enhanced-pull-request-creation-delegate)
12. [Staying Up to Date](#staying-up-to-date)

---

## Autopilot Mode (Experimental)

### Overview

Autopilot mode is an **experimental** feature that instructs the agent to work autonomously until a task is fully completed, without pausing for confirmation at every step. It is ideal for long-running, complex tasks where you want minimal interruptions.

### Enabling Experimental Features

Autopilot is only available after activating experimental mode:

```bash
# Start Copilot CLI with experimental features enabled
copilot --experimental
```

Or, once the CLI is running, toggle it with the slash command:

```
> /experimental
```

The setting persists in your config file after first activation — you do not need to pass `--experimental` on every launch.

### Activating Autopilot

The three execution modes cycle with **Shift+Tab**:

```
interactive  →  plan  →  autopilot  →  (back to interactive)
```

| Mode | Behavior |
|------|----------|
| **interactive** | Default. Responds to messages, asks for confirmation. |
| **plan** | Creates a plan first, then awaits approval before executing. |
| **autopilot** | Executes autonomously until the task is done, with minimal interruptions. |

### When to Use Autopilot

```
> /experimental
> [Shift+Tab to switch to autopilot]

> Refactor the entire authentication module to use JWTs,
  update all tests, and fix any linting issues.

[AI works autonomously across many files and steps without pausing]
[Reports back when the full task is complete]
```

**✅ Good for:**
- Large refactoring across many files
- End-to-end feature implementations
- Tasks with clear success criteria

**❌ Avoid for:**
- Tasks requiring frequent human judgment
- Exploratory work where direction may change
- Changes in sensitive production configurations

---

## Fleet Mode (`/fleet`)

### Overview

Fleet mode enables **parallel subagent execution**, letting the CLI run multiple background tasks concurrently. Instead of sequential work, fleet mode spawns specialized agents that operate at the same time — useful for large refactors, multi-component features, or tasks that can be naturally divided.

### Enabling Fleet Mode

```
> /fleet
```

Fleet mode is enabled per session. Once active, tasks you describe can be broken up and executed in parallel.

### Viewing and Managing Background Tasks

```
> /tasks
```

Shows all running and completed background tasks, their status, and output:

```
Active Tasks:
  [1] ✅ Refactor: src/api/users.js     (done)
  [2] 🔄 Refactor: src/api/products.js  (in progress)
  [3] 🔄 Add tests: src/api/orders.js   (in progress)
  [4] ⏳ Update docs: API.md             (queued)
```

### Example: Parallel Refactoring

```
> /fleet
> Refactor the following in parallel:
  - Update all API handlers to use async/await
  - Add JSDoc comments to all service files
  - Run the test suite and fix any failures

[Fleet spawns 3 concurrent agents working simultaneously]
[All three complete faster than sequential execution]
```

### Concurrent Operation Types

Fleet mode is well-suited for running these simultaneously:
- **Code changes** across different modules
- **Research and analysis** (codebase scanning, pattern detection)
- **Test execution** in parallel test suites
- **Documentation updates** alongside code changes

---

## Research Command (`/research`)

### Overview

The `/research` command runs a deep investigation using GitHub search, web sources, and local codebase analysis. It returns a comprehensive report useful for understanding unfamiliar codebases, evaluating dependencies, and planning large changes.

### Basic Usage

```
> /research How does authentication work in this codebase?

AI: Researching authentication patterns...

📋 Research Report: Authentication
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Overview:
  Authentication is handled by src/auth/ using JWT tokens.
  Sessions are stored in Redis (see src/cache/session.js).

Key Files:
  - src/auth/middleware.js   — JWT verification
  - src/auth/login.js        — Login flow
  - src/auth/tokens.js       — Token generation and refresh

External Dependencies:
  - jsonwebtoken ^9.0.0
  - bcryptjs ^2.4.3

Patterns:
  - Tokens expire after 1 hour, refresh tokens after 7 days
  - All protected routes use the `requireAuth` middleware

Potential Issues:
  - Token refresh logic does not handle concurrent requests
  - No rate limiting on /login endpoint
```

### Research with GitHub Search

```
> /research Common patterns for rate limiting in Express APIs

[Searches GitHub repositories and returns best practices with examples]
```

### Sharing Research Results

After a research session, share the report:

```
> /share

[Generates a shareable link or exports the report]
```

### When to Use `/research`

- Before undertaking a large refactor (understand the current shape)
- Evaluating whether to add a new dependency
- Onboarding to an unfamiliar codebase
- Investigating a bug's root cause across many files

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

## New Keyboard Shortcuts

Several new keyboard shortcuts have been added to improve the interaction experience:

| Shortcut | Action |
|----------|--------|
| `Shift+Tab` | Cycle through modes: interactive → plan → autopilot |
| `Ctrl+T` | Toggle model reasoning display |
| `Ctrl+G` | Open the current prompt in your external `$EDITOR` |
| `Ctrl+X → O` | Open the link from the most recent timeline event |

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
# Example: GitHub Copilot CLI version 0.0.410
```

### Updating

```bash
# Homebrew (macOS/Linux)
brew update && brew upgrade github/gh/gh-copilot

# GitHub CLI extension
gh extension upgrade gh-copilot

# npm global install
npm update -g @github/copilot

# npx (always uses latest)
npx @github/copilot@latest
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
