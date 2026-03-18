# Slash Commands Reference

Slash commands are special commands that start with `/` and provide quick access to CLI features. This is a complete reference of all available commands.

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

### /clear, /new

Clear the conversation history and start fresh.

```
> /clear
```

**Use when:**
- Switching to a completely different task
- Context window is getting full
- Want to start with clean slate

**Note:** This doesn't delete the session, just resets the conversation.

### /resume [sessionId]

Switch to a different session or resume a previous one.

```
# List all sessions
> /resume

# Resume specific session
> /resume abc123-def456-789
```

**Sessions include:**
- Conversation history
- File context
- Working directory
- Checkpoints

### /session [subcommand]

Manage and view session information.

```
# Show session overview
> /session

# View recent checkpoints
> /session checkpoints
> /session checkpoints 10      # Last 10 checkpoints

# List files in workspace
> /session files

# Show implementation plan
> /session plan

# Rename current session
> /session rename "My Project Feature"
```

**Subcommands:**
- `checkpoints [n]` - View session checkpoints
- `files` - List workspace files
- `plan` - Display plan.md if it exists
- `rename <name>` - Give session a friendly name

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

**Available models:**
- `claude-sonnet-4.5` (default) - Balanced performance
- `claude-sonnet-4` - Previous generation Sonnet
- `gpt-5` - OpenAI flagship
- And more available via `/model` selection menu...

**Choose based on:**
- Task complexity (use Opus for complex reasoning)
- Speed requirements (use Haiku for quick tasks)
- Cost considerations (Haiku is most economical)

### /context

Display context window usage and included files.

```
> /context
```

**Shows:**
- Token usage (current/max)
- Percentage used
- List of files in context
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

### /compact

Compress conversation history to free up context space.

```
> /compact
```

**What it does:**
- Summarizes old messages
- Preserves important information
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

**Use when:**
- `/fleet` is active and you want to monitor progress
- Background operations are running
- Need to cancel a stuck or unwanted task

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

### /add-dir <directory>

Add a directory to the allowed list for file access.

```
> /add-dir /opt/my-libs
> /add-dir ~/shared-code
```

**Use when:**
- Need to access files outside working directory
- Working with multiple project roots
- Accessing shared libraries

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

**⚠️ Use with caution:**
- Only use in fully trusted, local-only environments
- Grants broad access; avoid with untrusted projects or shared machines
- For fine-grained control, prefer `/add-dir` and explicit tool approvals

## Code

### /ide

Connect to an IDE workspace for richer file access and editing context.

```
# Open IDE connection interface
> /ide
```

**What it does:**
- Establishes a connection to VS Code or another supported IDE
- Enables file access via the IDE workspace
- Provides richer editing context (open tabs, cursor position, etc.)

**Use when:**
- Working alongside an IDE session
- Need IDE-level file awareness
- Want AI suggestions that are aware of your open editor state

### /diff

Review the changes made in the current directory.

```
> /diff
```

**Shows:**
- Staged and unstaged git changes
- File modifications, additions, and deletions
- Unified diff output
- Summary of which files changed

**Use when:**
- Reviewing changes before committing
- Understanding what the AI modified
- Auditing edits after a long session

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

### /review

Run a code review agent to analyze current changes.

```
> /review
```

**What it does:**
- Analyzes staged and unstaged code changes
- Identifies bugs, security vulnerabilities, and logic errors
- Provides actionable, high-signal review comments
- Focuses on issues that genuinely matter — not style or formatting

**Example output:**
```
> /review
🔍 Analyzing changes...

📋 Code Review:
  src/auth.js
    ⚠️  Line 42: Potential SQL injection — use parameterized queries
  src/utils.js
    ℹ️  Line 15: Missing null check before accessing .length
```

**Use when:**
- Before committing or opening a PR
- After large AI-assisted refactors
- As a final sanity check on your changes

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

### /login

Authenticate with GitHub Copilot.

```
> /login
```

**Process:**
1. Command generates device code
2. Browser opens automatically
3. Enter code on GitHub
4. Return to CLI when authorized

**Alternative:** Use PAT with `GH_TOKEN` environment variable.

### /logout

Sign out of GitHub Copilot.

```
> /logout
```

Clears authentication tokens. You'll need to `/login` again.

## Configuration

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

### /reset-allowed-tools

Reset the list of tools the AI can use.

```
> /reset-allowed-tools
```

**When to use:**
- After restricting tools for safety
- To restore full functionality
- Troubleshooting permission issues

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
```

**Plugins extend CLI capabilities with:**
- New slash commands
- Specialized domain tools
- Third-party integrations
- Custom workflows and automations

## Information

### /help

Display help information and available commands.

```
> /help
```

**Shows:**
- All slash commands
- Keyboard shortcuts
- Quick reference guide
- Links to documentation

### /usage

Display session usage metrics and statistics.

```
> /usage
```

**Shows:**
- Requests used this session
- Monthly premium requests
- Requests remaining
- Token consumption
- Cost estimates (if applicable)

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

### /changelog [summarize]

Display the changelog for CLI versions.

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

### /update

Update the CLI to the latest version.

```
> /update
```

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

Browse and select from available agents.

```
> /agent
```

**Shows:**
- Custom MCP agents
- Specialized agents
- Agent capabilities
- Selection interface

**What are agents?**
Agents are specialized AI assistants with specific capabilities or knowledge domains.

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

# Disable server temporarily
> /mcp disable my-server

# Re-enable server
> /mcp enable my-server
```

**MCP Servers extend CLI capabilities:**
- Database access
- API integrations
- Custom tools
- Domain-specific knowledge

See [Advanced Features](08-advanced-features.md) for MCP details.

### /skills [subcommand] [args]

Manage skills for enhanced capabilities.

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

### /share [file|gist] [path]

Share session to markdown file or GitHub Gist.

```
# Share to file
> /share file session.md

# Share to specific path
> /share file ~/Documents/my-session.md

# Share to GitHub Gist
> /share gist
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

### /feedback

Submit feedback about the CLI.

```
> /feedback
```

Opens confidential feedback survey in your browser.

**Feedback helps improve:**
- Feature priorities
- Bug fixes
- User experience
- Documentation

### /research <query>

Run a deep research investigation using GitHub search and web sources.

```
> /research How does Zod v3 validation work?
> /research Best practices for React Query caching
> /research OAuth2 PKCE flow implementation examples
```

**What it does:**
- Searches GitHub repositories for real-world usage examples
- Searches the web for relevant documentation and articles
- Synthesizes findings from multiple sources
- Provides a comprehensive, cited research report

**Use when:**
- Need authoritative, up-to-date information on a library or API
- Looking for real-world open-source examples before implementing
- Researching best practices or comparing approaches
- Investigating an unfamiliar technology

### /restart

Restart the CLI while preserving the current session.

```
> /restart
```

**Use when:**
- The CLI is behaving unexpectedly or feels stuck
- You just ran `/update` and want to load the new version
- You need to reload configuration or plugins
- Recovering from a frozen or unresponsive state

**Note:** Session data is preserved and automatically restored after restart.

### /exit, /quit

Exit the CLI.

```
> /exit
> /quit
```

**Alternative:** Press `Ctrl+D` to quickly exit.

**Note:** Sessions are automatically saved and can be resumed later.

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
| `/copy` | Copy last response | `/copy` |
| `/model` | Change AI model | `/model claude-sonnet-4.5` |
| `/fleet` | Parallel subagents | `/fleet` |
| `/tasks` | View background tasks | `/tasks` |
| `/diff` | Review changes | `/diff` |
| `/pr` | Operate on PRs | `/pr create` |
| `/review` | Code review agent | `/review` |
| `/lsp` | Language server | `/lsp restart` |
| `/ide` | Connect to IDE | `/ide` |
| `/cwd` | Change directory | `/cwd ~/projects` |
| `/allow-all` | Enable all permissions | `/allow-all` |
| `/context` | Check memory | `/context` |
| `/compact` | Compress history | `/compact` |
| `/plan` | Create plan | `/plan Build API` |
| `/research` | Deep research | `/research Zod validation` |
| `/delegate` | Create PR via AI | `/delegate Fix issue #123` |
| `/usage` | Check quota | `/usage` |
| `/changelog` | View changelog | `/changelog summarize` |
| `/update` | Update CLI | `/update` |
| `/version` | Show version | `/version` |
| `/experimental` | Experimental features | `/experimental list` |
| `/instructions` | Toggle instructions | `/instructions` |
| `/streamer-mode` | Hide sensitive info | `/streamer-mode` |
| `/plugin` | Manage plugins | `/plugin list` |
| `/help` | Show help | `/help` |
| `/share` | Export session | `/share file out.md` |
| `/restart` | Restart CLI | `/restart` |
| `/exit` | Quit CLI | `/exit` |

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
/new  = /clear
/cd   = /cwd
/h    = /help (if supported)
/q    = /quit (if supported)
```

## Error Messages

If a command fails:

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
