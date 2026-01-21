# .copilot Directory Guide

A comprehensive guide to understanding the `~/.copilot` directory, its structure, configuration files, and how to manage your GitHub Copilot CLI data.

## Table of Contents

1. [What is ~/.copilot?](#what-is-copilot)
2. [Directory Structure](#directory-structure)
3. [Configuration Files](#configuration-files)
4. [Session Storage](#session-storage)
5. [Skills Directory](#skills-directory)
6. [Logs and History](#logs-and-history)
7. [Authentication Data](#authentication-data)
8. [Managing ~/.copilot](#managing-copilot)
9. [Backup and Restore](#backup-and-restore)
10. [Troubleshooting](#troubleshooting)

## What is ~/.copilot?

The `~/.copilot` directory is the **home directory for GitHub Copilot CLI**. It stores all user-specific configuration, sessions, authentication, logs, and custom skills.

### Purpose

```
~/.copilot/ serves as:
─────────────────────────────────────────
✅ Configuration storage      (config.json, mcp-config.json)
✅ Session persistence        (session-state/)
✅ Authentication cache       (auth tokens)
✅ Command history            (command-history-state.json)
✅ Custom skills storage      (skills/)
✅ Log files                  (logs/)
✅ Global instructions        (copilot-instructions.md)
```

### Location

```bash
# Default location (Linux/macOS)
~/.copilot/

# Full path example
/home/username/.copilot/

# Windows
%USERPROFILE%\.copilot\
C:\Users\username\.copilot\
```

## Directory Structure

### Complete Structure

```
~/.copilot/
├── config.json                      # Main configuration file
├── mcp-config.json                  # MCP server configuration
├── copilot-instructions.md          # Global instructions (optional)
├── command-history-state.json       # Command history
├── auth/                            # Authentication tokens
│   └── tokens.json
├── session-state/                   # Active and past sessions
│   ├── <session-id-1>/
│   │   ├── context.json
│   │   ├── conversation.json
│   │   ├── plan.md
│   │   └── checkpoints/
│   ├── <session-id-2>/
│   └── ...
├── skills/                          # Custom skills (optional)
│   ├── python-expert.skill.md
│   ├── react-patterns.skill.md
│   └── my-custom-skill.skill.md
├── logs/                            # Log files
│   ├── copilot.log
│   ├── copilot.log.1
│   └── copilot.log.2
└── pkg/                             # Package data (internal)
    └── ...
```

### File Descriptions

| File/Directory | Purpose | User Editable |
|----------------|---------|---------------|
| `config.json` | Main configuration | ✅ Yes |
| `mcp-config.json` | MCP server settings | ✅ Yes |
| `copilot-instructions.md` | Global AI instructions | ✅ Yes |
| `command-history-state.json` | Command history | ❌ Auto-managed |
| `auth/` | Authentication tokens | ❌ Auto-managed |
| `session-state/` | Session data | ⚠️  Read-only |
| `skills/` | Custom skill files | ✅ Yes |
| `logs/` | Application logs | 📖 Read-only |
| `pkg/` | Internal package data | ❌ Do not modify |

## Configuration Files

### config.json

The main configuration file that controls Copilot CLI behavior.

#### Full Schema

```json
{
  // Banner display (shown on first launch)
  "banner": "always" | "once" | "never",
  
  // Default AI model
  "model": "claude-sonnet-4.5" | "claude-opus-4.5" | "claude-haiku-4.5" | "gpt-5.2" | ...,
  
  // Visual theme
  "theme": "auto" | "dark" | "light",
  
  // Render markdown in responses
  "render_markdown": true | false,
  
  // Show AI reasoning/thinking process
  "show_reasoning": true | false,
  
  // Trusted folders (allowed file access)
  "trusted_folders": [
    "/path/to/folder1",
    "/path/to/folder2"
  ],
  
  // Allowed URLs for web access
  "allowed_urls": [
    "https://docs.example.com",
    "https://api.example.com"
  ],
  
  // Auto-compact conversation when context fills up
  "auto_compact": true | false,
  "compact_threshold": 0.8,  // Compact at 80% context usage
  
  // Last logged-in user
  "last_logged_in_user": {
    "host": "https://github.com",
    "login": "username"
  },
  
  // All authenticated users
  "logged_in_users": [
    {
      "host": "https://github.com",
      "login": "username1"
    },
    {
      "host": "https://github.com/enterprises/company",
      "login": "username2"
    }
  ],
  
  // Session settings
  "session": {
    "auto_save": true,
    "max_history": 100
  },
  
  // Feature flags (may vary by version)
  "experimental_features": {
    "enhanced_code_analysis": true,
    "multi_file_refactoring": true
  }
}
```

#### Minimal config.json

```json
{
  "model": "claude-sonnet-4.5",
  "theme": "auto"
}
```

#### Example Configurations

**Developer-Focused:**
```json
{
  "banner": "never",
  "model": "claude-sonnet-4.5",
  "theme": "dark",
  "render_markdown": true,
  "show_reasoning": true,
  "auto_compact": true,
  "compact_threshold": 0.75,
  "trusted_folders": [
    "/home/user/projects",
    "/home/user/work"
  ],
  "allowed_urls": [
    "https://docs.github.com",
    "https://developer.mozilla.org"
  ]
}
```

**Performance-Focused:**
```json
{
  "banner": "never",
  "model": "claude-haiku-4.5",
  "theme": "auto",
  "render_markdown": false,
  "show_reasoning": false,
  "auto_compact": true,
  "compact_threshold": 0.6,
  "trusted_folders": [
    "/home/user/projects"
  ]
}
```

**Premium Features:**
```json
{
  "banner": "never",
  "model": "claude-opus-4.5",
  "theme": "auto",
  "render_markdown": true,
  "show_reasoning": true,
  "auto_compact": false,
  "trusted_folders": [
    "/home/user/projects",
    "/opt/work"
  ],
  "allowed_urls": [
    "https://docs.company.com"
  ],
  "experimental_features": {
    "enhanced_code_analysis": true
  }
}
```

### Configuration Options Explained

#### banner
Controls when the animated banner is displayed.
- `"always"` - Show on every launch
- `"once"` - Show only first time
- `"never"` - Never show

**Usage:**
```json
"banner": "never"
```

#### model
Default AI model for conversations.

**Available models:**
```json
"model": "claude-sonnet-4.5"    // Balanced (default)
"model": "claude-haiku-4.5"     // Fast & efficient
"model": "claude-opus-4.5"      // Most capable
"model": "gpt-5.2"              // OpenAI
"model": "gpt-5.2-codex"        // Code-optimized
```

**Override per session:**
```bash
> /model claude-opus-4.5
```

#### theme
Visual appearance of the CLI.

```json
"theme": "auto"   // Follow system preference
"theme": "dark"   // Always dark mode
"theme": "light"  // Always light mode
```

#### render_markdown
Format responses with markdown rendering.

```json
"render_markdown": true   // Format code blocks, headers, etc.
"render_markdown": false  // Plain text (faster)
```

#### show_reasoning
Display AI's thinking process.

```json
"show_reasoning": true    // Show thinking tags
"show_reasoning": false   // Hide reasoning
```

**Example with reasoning:**
```
🤔 Thinking: I need to check the file structure first,
   then analyze the imports to understand dependencies...

Response: The code has three main issues...
```

#### trusted_folders
Directories Copilot CLI can access.

```json
"trusted_folders": [
  "/home/user/projects",
  "/home/user/work/repos",
  "/opt/company-code"
]
```

**Add via CLI:**
```bash
> /add-dir /path/to/folder
```

**Security note:** Only add trusted directories!

#### allowed_urls
URLs the CLI can fetch content from.

```json
"allowed_urls": [
  "https://docs.github.com",
  "https://api.company.com",
  "https://internal-docs.company.com"
]
```

**Use case:** Fetching documentation or API specs.

#### auto_compact
Automatically compress conversation when context fills.

```json
"auto_compact": true,
"compact_threshold": 0.8  // Trigger at 80% full
```

**Manual compact:**
```bash
> /compact
```

### mcp-config.json

Configuration for MCP (Model Context Protocol) servers.

#### Structure

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-name"],
      "env": {
        "VAR_NAME": "value"
      },
      "disabled": false
    }
  }
}
```

#### Example: Database Server

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/mydb"
      }
    },
    "sqlite": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sqlite"],
      "env": {
        "DB_PATH": "/home/user/data/mydb.sqlite"
      }
    }
  }
}
```

#### Example: Multiple Services

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem"],
      "env": {
        "ALLOWED_DIRS": "/home/user/projects:/opt/work"
      }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GH_TOKEN}"
      }
    },
    "custom-api": {
      "command": "node",
      "args": ["/home/user/mcp-servers/custom-api.js"],
      "env": {
        "API_KEY": "${CUSTOM_API_KEY}",
        "API_URL": "https://api.company.com"
      }
    }
  }
}
```

**Manage via CLI:**
```bash
> /mcp show
> /mcp add server-name
> /mcp disable server-name
> /mcp enable server-name
```

### copilot-instructions.md

Global instructions that apply to all projects.

**Location:** `~/.copilot/copilot-instructions.md`

**Example:**
```markdown
# My Global Copilot Instructions

## Personal Preferences

### Code Style
- I prefer TypeScript over JavaScript
- Use async/await instead of promises
- Single quotes for strings
- 2 spaces for indentation

### Comments
- Add comments only for complex logic
- Prefer self-documenting code
- Use JSDoc for public APIs

### Testing
- Write tests for all new features
- Use descriptive test names
- Follow AAA pattern (Arrange, Act, Assert)

### Git
- Conventional commit messages
- Keep commits small and focused
- Always pull before pushing
```

**Priority:** Lower than project-specific instructions.

## Session Storage

### session-state/

Each session is stored in a separate directory.

```
session-state/
├── abc123-def456-789012/          # Session ID
│   ├── context.json               # Context window state
│   ├── conversation.json          # Full conversation
│   ├── plan.md                    # Plan (if created)
│   ├── metadata.json              # Session metadata
│   └── checkpoints/               # Session checkpoints
│       ├── checkpoint-1.json
│       ├── checkpoint-2.json
│       └── checkpoint-3.json
```

### Session Files

#### context.json
```json
{
  "files": [
    {
      "path": "src/app.js",
      "tokens": 2453,
      "lastAccessed": "2026-01-21T19:00:00Z"
    }
  ],
  "totalTokens": 45231,
  "maxTokens": 200000,
  "percentage": 22.6
}
```

#### conversation.json
```json
{
  "messages": [
    {
      "role": "user",
      "content": "Create a hello world function",
      "timestamp": "2026-01-21T19:00:00Z"
    },
    {
      "role": "assistant",
      "content": "I'll create a hello world function...",
      "timestamp": "2026-01-21T19:00:05Z",
      "toolCalls": [...]
    }
  ]
}
```

#### metadata.json
```json
{
  "sessionId": "abc123-def456-789012",
  "created": "2026-01-21T19:00:00Z",
  "lastActive": "2026-01-21T19:30:00Z",
  "workingDirectory": "/home/user/projects/my-app",
  "model": "claude-sonnet-4.5",
  "name": "My App Development",
  "tags": ["react", "typescript"],
  "requestCount": 42
}
```

### Managing Sessions

**View sessions:**
```bash
> /resume
```

**Clean old sessions:**
```bash
# Delete sessions older than 30 days
find ~/.copilot/session-state/ -type d -mtime +30 -exec rm -rf {} +

# Or keep only recent 10
ls -t ~/.copilot/session-state/ | tail -n +11 | xargs -I {} rm -rf ~/.copilot/session-state/{}
```

## Skills Directory

Custom skills storage.

```
skills/
├── languages/
│   ├── python-expert.skill.md
│   └── javascript-expert.skill.md
├── frameworks/
│   ├── react-patterns.skill.md
│   └── django-expert.skill.md
└── domains/
    ├── security-audit.skill.md
    └── performance-opt.skill.md
```

**See:** [Skills System Guide](14-skills-system.md)

## Logs and History

### logs/

Application logs for debugging.

```
logs/
├── copilot.log          # Current log
├── copilot.log.1        # Rotated log (yesterday)
├── copilot.log.2        # Older
└── copilot.log.3        # Oldest
```

**View logs:**
```bash
# Tail current log
tail -f ~/.copilot/logs/copilot.log

# Search for errors
grep ERROR ~/.copilot/logs/copilot.log

# View recent activity
tail -100 ~/.copilot/logs/copilot.log
```

**Log levels:**
- `DEBUG` - Detailed information
- `INFO` - General information
- `WARN` - Warnings
- `ERROR` - Errors

**Enable debug logging:**
```bash
export COPILOT_LOG_LEVEL=debug
copilot
```

### command-history-state.json

Command history across sessions.

```json
{
  "history": [
    {
      "command": "Create a hello world function",
      "timestamp": "2026-01-21T19:00:00Z"
    },
    {
      "command": "/model claude-opus-4.5",
      "timestamp": "2026-01-21T19:05:00Z"
    }
  ],
  "maxSize": 1000
}
```

**Access history:**
```bash
# In CLI, press ↑ to navigate history
> ↑
```

**Clear history:**
```bash
rm ~/.copilot/command-history-state.json
```

## Authentication Data

### auth/

Stores authentication tokens securely.

```
auth/
└── tokens.json
```

**Format:**
```json
{
  "github.com": {
    "token": "ghu_...",
    "expires": "2026-02-21T00:00:00Z",
    "user": "username"
  },
  "github.com/enterprises/company": {
    "token": "ghu_...",
    "expires": "2026-02-21T00:00:00Z",
    "user": "username"
  }
}
```

**Security:**
- Tokens stored with 600 permissions (user read/write only)
- Never commit to version control
- Rotated on login/logout

**Manage:**
```bash
> /login    # Authenticate
> /logout   # Remove tokens
> /user     # Switch users
```

## Managing ~/.copilot

### Viewing Size

```bash
# Total size
du -sh ~/.copilot

# Size by directory
du -sh ~/.copilot/*

# Largest items
du -h ~/.copilot/* | sort -rh | head -10
```

### Cleaning Up

#### Clean Old Sessions
```bash
# Keep only last 30 days
find ~/.copilot/session-state -type d -mtime +30 -exec rm -rf {} +
```

#### Clean Old Logs
```bash
# Keep only last 7 log files
ls -t ~/.copilot/logs/copilot.log.* | tail -n +8 | xargs rm -f
```

#### Reset Everything
```bash
# ⚠️ WARNING: This deletes all data!
rm -rf ~/.copilot

# Then re-authenticate
copilot
> /login
```

### Disk Space Issues

If `~/.copilot` is too large:

```bash
# 1. Check what's using space
du -sh ~/.copilot/*

# 2. Usually it's sessions or logs
du -sh ~/.copilot/session-state
du -sh ~/.copilot/logs

# 3. Clean as needed
rm -rf ~/.copilot/session-state/old-session-id
rm ~/.copilot/logs/copilot.log.*
```

## Backup and Restore

### What to Backup

**Essential:**
- ✅ `config.json` - Your settings
- ✅ `mcp-config.json` - MCP servers
- ✅ `copilot-instructions.md` - Global instructions
- ✅ `skills/` - Custom skills

**Optional:**
- ⚠️  `session-state/` - Session history (can be large)
- ⚠️  `command-history-state.json` - Command history

**Don't Backup:**
- ❌ `auth/` - Tokens (security risk)
- ❌ `logs/` - Temporary data
- ❌ `pkg/` - Auto-managed

### Backup Script

```bash
#!/bin/bash
# backup-copilot.sh

BACKUP_DIR="$HOME/copilot-backup-$(date +%Y%m%d)"
mkdir -p "$BACKUP_DIR"

# Backup configuration
cp ~/.copilot/config.json "$BACKUP_DIR/"
cp ~/.copilot/mcp-config.json "$BACKUP_DIR/" 2>/dev/null
cp ~/.copilot/copilot-instructions.md "$BACKUP_DIR/" 2>/dev/null

# Backup skills
if [ -d ~/.copilot/skills ]; then
  cp -r ~/.copilot/skills "$BACKUP_DIR/"
fi

# Optional: backup recent sessions (last 7 days)
mkdir -p "$BACKUP_DIR/session-state"
find ~/.copilot/session-state -type d -mtime -7 -exec cp -r {} "$BACKUP_DIR/session-state/" \;

echo "Backup created at: $BACKUP_DIR"
```

### Restore

```bash
#!/bin/bash
# restore-copilot.sh

BACKUP_DIR="$1"

if [ -z "$BACKUP_DIR" ]; then
  echo "Usage: $0 /path/to/backup"
  exit 1
fi

# Restore configuration
cp "$BACKUP_DIR/config.json" ~/.copilot/
cp "$BACKUP_DIR/mcp-config.json" ~/.copilot/ 2>/dev/null
cp "$BACKUP_DIR/copilot-instructions.md" ~/.copilot/ 2>/dev/null

# Restore skills
if [ -d "$BACKUP_DIR/skills" ]; then
  cp -r "$BACKUP_DIR/skills" ~/.copilot/
fi

echo "Restore complete"
```

### Sync Across Machines

Use version control for configuration:

```bash
# On machine 1
cd ~/.copilot
git init
git add config.json mcp-config.json copilot-instructions.md skills/
git commit -m "My Copilot config"
git remote add origin https://github.com/username/copilot-config.git
git push -u origin main

# On machine 2
cd ~/.copilot
git clone https://github.com/username/copilot-config.git .
```

## Troubleshooting

### Config Not Loading

**Problem:** Changes to config.json not taking effect.

**Solutions:**
```bash
# 1. Validate JSON syntax
cat ~/.copilot/config.json | jq .

# 2. Check permissions
ls -l ~/.copilot/config.json
# Should be readable

# 3. Restart CLI
# Exit and relaunch

# 4. Check for syntax errors
> /context
# Look for error messages
```

### Corrupted Session

**Problem:** Session won't load or crashes CLI.

**Solutions:**
```bash
# 1. List sessions
ls ~/.copilot/session-state/

# 2. Identify problematic session
# (usually the most recent)

# 3. Delete corrupted session
rm -rf ~/.copilot/session-state/<session-id>

# 4. Start new session
copilot
```

### Directory Permission Issues

**Problem:** Cannot write to ~/.copilot.

**Solutions:**
```bash
# Check ownership
ls -ld ~/.copilot

# Fix permissions
chmod 755 ~/.copilot
chmod -R u+w ~/.copilot

# If owned by wrong user
sudo chown -R $USER:$USER ~/.copilot
```

### Space Issues

**Problem:** ~/.copilot is using too much disk space.

**Solutions:**
```bash
# 1. Check size
du -sh ~/.copilot/*

# 2. Clean sessions
rm -rf ~/.copilot/session-state/*

# 3. Clean logs
rm ~/.copilot/logs/*.log.*

# 4. Start fresh if needed
# Backup config first!
cp ~/.copilot/config.json ~/config-backup.json
rm -rf ~/.copilot
mkdir ~/.copilot
cp ~/config-backup.json ~/.copilot/config.json
```

### MCP Server Issues

**Problem:** MCP servers not working.

**Solutions:**
```bash
# 1. Check config
cat ~/.copilot/mcp-config.json | jq .

# 2. Test MCP server manually
npx -y @modelcontextprotocol/server-postgres

# 3. Check environment variables
echo $DATABASE_URL

# 4. View errors in logs
tail ~/.copilot/logs/copilot.log | grep MCP
```

## Best Practices

### Configuration Management

✅ **DO:**
- Backup config.json regularly
- Use version control for configs
- Document custom settings
- Keep configs minimal
- Test after changes

❌ **DON'T:**
- Commit auth tokens to git
- Share config.json with secrets
- Use absolute paths (use ~/)
- Ignore validation errors

### Session Management

✅ **DO:**
- Clean old sessions regularly
- Use descriptive session names
- Resume sessions for complex work
- Save important sessions

❌ **DON'T:**
- Let sessions accumulate indefinitely
- Store sensitive data in sessions
- Rely solely on auto-save

### Security

✅ **DO:**
- Keep auth/ directory private (600)
- Use environment variables for secrets
- Rotate tokens regularly
- Backup without auth data

❌ **DON'T:**
- Share ~/.copilot directory
- Commit tokens to version control
- Use insecure permissions
- Store passwords in configs

## Quick Reference

### Directory Locations
```bash
~/.copilot/                          # Main directory
~/.copilot/config.json               # Main config
~/.copilot/mcp-config.json           # MCP servers
~/.copilot/copilot-instructions.md   # Global instructions
~/.copilot/skills/                   # Custom skills
~/.copilot/session-state/            # Sessions
~/.copilot/logs/                     # Log files
```

### Common Commands
```bash
# View config
cat ~/.copilot/config.json

# Edit config
vim ~/.copilot/config.json

# Check size
du -sh ~/.copilot

# View logs
tail -f ~/.copilot/logs/copilot.log

# Clean sessions
rm -rf ~/.copilot/session-state/*

# Backup config
cp ~/.copilot/config.json ~/backup/
```

### Environment Variables
```bash
# Log level
export COPILOT_LOG_LEVEL=debug

# Log file location
export COPILOT_LOG_FILE=/tmp/copilot.log

# Config directory (advanced)
export COPILOT_CONFIG_DIR=/custom/path
```

## Summary

The `~/.copilot` directory is the central location for all GitHub Copilot CLI data:

- **Configuration** - Preferences and settings
- **Sessions** - Conversation history and context
- **Authentication** - Secure token storage
- **Skills** - Custom expertise modules
- **Logs** - Debug and troubleshooting information

**Key Takeaways:**
1. `config.json` controls CLI behavior
2. Sessions persist across launches
3. Skills are stored globally
4. Regular cleanup prevents space issues
5. Backup configurations for disaster recovery

---

**Related Guides:**
- [Advanced Features](08-advanced-features.md) - MCP and custom instructions
- [Skills System](14-skills-system.md) - Custom skills
- [Troubleshooting](11-troubleshooting.md) - Common issues

**Previous:** [Skills System Guide](14-skills-system.md)
