# Slash Commands Reference

Slash commands are special commands that start with `/` and provide quick access to CLI features. This is a complete reference of all available commands.

## Command Categories

- [Session Management](#session-management)
- [Context & Model](#context--model)
- [File System](#file-system)
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
- `claude-sonnet-4.5` (default) - Balanced
- `claude-haiku-4.5` - Fast
- `claude-opus-4.5` - Most capable
- `gpt-5.2` - OpenAI flagship
- `gpt-5.2-codex` - Optimized for code
- And more...

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

## GitHub Integration

### /delegate <prompt>

Create a PR in a remote repository with AI-generated changes.

```
> /delegate Add user authentication to the API
```

**What happens:**
1. AI analyzes the request
2. Creates a new branch
3. Makes necessary changes
4. Pushes to remote
5. Creates a Pull Request

**Requirements:**
- Must be in a Git repository
- Remote must be configured
- GitHub authentication required

**Example flow:**
```
> /delegate Fix typo in README

🔧 Creating branch: copilot-fix-readme-typo
📝 Making changes...
⬆️  Pushing to origin...
🔗 Created PR #42: Fix typo in README
   https://github.com/user/repo/pull/42
```

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
- Pre-packaged capabilities
- Reusable patterns
- Domain expertise
- Custom instructions

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
| `/model` | Change AI model | `/model gpt-5.2` |
| `/cwd` | Change directory | `/cwd ~/projects` |
| `/context` | Check memory | `/context` |
| `/plan` | Create plan | `/plan Build API` |
| `/usage` | Check quota | `/usage` |
| `/help` | Show help | `/help` |
| `/share` | Export session | `/share file out.md` |
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
