# Advanced Features

Explore advanced capabilities of GitHub Copilot CLI including MCP servers, custom agents, skills, and extended configurations.

## Model Context Protocol (MCP)

MCP is an open protocol that allows Copilot CLI to connect to external data sources and tools.

### What is MCP?

**Model Context Protocol** enables:
- Database connections
- API integrations
- Custom tool creation
- Domain-specific knowledge
- Third-party service access

### Viewing MCP Servers

```
> /mcp show
```

Shows:
- Active MCP servers
- Server status
- Available tools/resources
- Configuration details

### Adding MCP Servers

```
> /mcp add my-database-server
```

You'll be prompted to configure:
- Server type
- Connection details
- Authentication
- Available tools

### MCP Server Configuration

MCP servers are configured in:
```
~/.copilot/mcp-config.json
```

Example configuration:
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "postgresql://user:pass@localhost/db"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem"],
      "env": {
        "ALLOWED_DIRS": "/home/user/projects"
      }
    }
  }
}
```

### Managing MCP Servers

```
# Edit server configuration
> /mcp edit postgres

# Disable server temporarily
> /mcp disable postgres

# Re-enable server
> /mcp enable postgres

# Delete server
> /mcp delete postgres
```

### Available MCP Servers

Common MCP servers:
- **@modelcontextprotocol/server-postgres** - PostgreSQL database
- **@modelcontextprotocol/server-sqlite** - SQLite database
- **@modelcontextprotocol/server-filesystem** - Enhanced file access
- **@modelcontextprotocol/server-github** - GitHub API (built-in)
- **@modelcontextprotocol/server-memory** - Persistent memory
- **Custom servers** - Build your own

### Using MCP Tools

Once configured, MCP tools are available automatically:

```
# With postgres MCP server
> Query the users table in the database

> Run this SQL: SELECT * FROM orders WHERE status = 'pending'

# With filesystem MCP server
> Search all files in /opt/project for "TODO"

# With memory MCP server
> Remember that I prefer tabs over spaces

> What's my preferred indentation?
```

### Building Custom MCP Servers

Create your own MCP server:

```typescript
// my-mcp-server.ts
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new Server({
  name: 'my-custom-server',
  version: '1.0.0',
});

// Register tools
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: 'my_tool',
        description: 'Does something useful',
        inputSchema: {
          type: 'object',
          properties: {
            param: { type: 'string' }
          }
        }
      }
    ]
  };
});

// Handle tool calls
server.setRequestHandler('tools/call', async (request) => {
  if (request.params.name === 'my_tool') {
    // Your tool logic here
    return {
      content: [
        { type: 'text', text: 'Tool result' }
      ]
    };
  }
});

// Start server
const transport = new StdioServerTransport();
await server.connect(transport);
```

Add to config:
```json
{
  "my-server": {
    "command": "node",
    "args": ["my-mcp-server.js"]
  }
}
```

## Custom Agents

Agents are specialized AI assistants with specific capabilities.

### Viewing Agents

```
> /agent
```

Shows available agents:
- Agent name
- Description
- Capabilities
- Status

### Agent Types

**Built-in agents:**
- Coding agent (default)
- Exploration agent
- Task execution agent

**Custom agents:**
- Domain-specific experts
- Tool specialists
- Knowledge bases

### Using Agents

```
# Select an agent
> /agent
[Choose from list]

# Agent-specific prompt
> [Agent processes with specialized knowledge]
```

### Creating Custom Agents

Custom agents extend capabilities with:
- Specialized prompts
- Custom tools
- Domain knowledge
- Specific workflows

## Skills System

Skills add reusable capabilities to Copilot CLI.

### Listing Skills

```
> /skills list
```

Shows:
- Available skills
- Installed skills
- Skill descriptions

### Viewing Skill Info

```
> /skills info python-expert
```

Shows:
- Skill description
- Capabilities
- Requirements
- Usage examples

### Adding Skills

```
> /skills add python-expert

> /skills add database-admin

> /skills add security-audit
```

### Removing Skills

```
> /skills remove python-expert
```

### Reloading Skills

After modifying skill files:

```
> /skills reload
```

### Creating Custom Skills

Skills are defined in:
```
~/.copilot/skills/
```

Example skill file `python-expert.skill.md`:
```markdown
# Python Expert Skill

## Description
Expert in Python development, best practices, and optimization.

## Capabilities
- Python code review
- Performance optimization
- Type hint recommendations
- Modern Python patterns

## Instructions
When working with Python code:
1. Use type hints for all functions
2. Follow PEP 8 style guide
3. Prefer f-strings for formatting
4. Use dataclasses for data structures
5. Apply SOLID principles

## Examples
[Include example code and patterns]
```

## Custom Instructions

Customize AI behavior with persistent instructions.

### Instruction Locations

Instructions are read from:

```
.github/instructions/**/*.instructions.md    # Project-specific
.github/copilot-instructions.md             # Repository-wide
AGENTS.md                                    # Agent config
CLAUDE.md                                    # Claude-specific
GEMINI.md                                    # Gemini-specific
~/.copilot/copilot-instructions.md          # Global
```

### Custom Directories

Add more instruction sources:

```bash
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="/path/to/instructions:/another/path"
```

### Example Instructions

**`AGENTS.md`:**
```markdown
# Project Architecture Guidelines

## Technology Stack
- Backend: Node.js + Express + TypeScript
- Database: PostgreSQL with Prisma ORM
- Authentication: JWT tokens

## Coding Patterns
- Use dependency injection
- Repository pattern for data access
- Service layer for business logic
- Controller layer for HTTP handling

## Security
- All inputs validated with Zod
- Passwords hashed with bcrypt (12 rounds)
- JWT tokens expire in 24 hours
```

**`.github/copilot-instructions.md`:**
```markdown
# Project Instructions

## Code Style
- Use 2 spaces for indentation
- Single quotes for strings
- Semicolons required
- Max line length: 100

## Testing
- Write tests for all new features
- Use Jest for unit tests
- Minimum 80% coverage

## Git Workflow
- Feature branches from main
- Prefix branches: feature/, bugfix/, hotfix/
- Commit messages: type(scope): message
```

**`~/.copilot/copilot-instructions.md`:**
```markdown
# Personal Preferences

## General
- I prefer TypeScript over JavaScript
- Use async/await instead of promises
- Add comments only when necessary

## Formatting
- Use Prettier with default settings
- Tabs for indentation
- Double quotes for strings
```

**For comprehensive guidance on AGENTS.md, see [AGENTS.md File Guide](13-agents-file.md).**

### Instruction Priority

When instructions conflict, priority is:
1. `.github/instructions/` (most specific)
2. `.github/copilot-instructions.md`
3. `AGENTS.md`
4. `CLAUDE.md` / `GEMINI.md` (model-specific)
5. `~/.copilot/copilot-instructions.md` (least specific)

## Session Persistence

### Session Files

Sessions are stored in:
```
~/.copilot/sessions/<session-id>/
```

Contains:
- Conversation history
- Context files
- Checkpoints
- plan.md (if created)

### Session Checkpoints

Copilot automatically creates checkpoints:

```
> /session checkpoints
```

Shows:
- Checkpoint timestamps
- Actions taken
- Files modified
- Context state

View specific checkpoint:
```
> /session checkpoints 5
```

### Resuming Sessions

```
> /resume

Sessions:
  1. abc123... - My React App (2 hours ago)
  2. def456... - API Development (yesterday)
  3. ghi789... - Bug Investigation (last week)

> /resume abc123
```

## Advanced Configuration

### Environment Variables

Configure Copilot CLI via environment:

```bash
# Authentication
export GH_TOKEN="your_token"
export GITHUB_TOKEN="your_token"

# Custom instructions
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="/path/to/instructions"

# Logging
export COPILOT_LOG_LEVEL="debug"
export COPILOT_LOG_FILE="/tmp/copilot.log"

# Network
export HTTPS_PROXY="http://proxy:8080"
export NO_PROXY="localhost,127.0.0.1"

# Model selection
export COPILOT_DEFAULT_MODEL="claude-opus-4.5"
```

### Configuration Files

**Global config:**
```
~/.copilot/config.json
```

Example:
```json
{
  "defaultModel": "claude-sonnet-4.5",
  "theme": "dark",
  "allowedDirectories": [
    "/home/user/projects",
    "/opt/work"
  ],
  "autoCompact": true,
  "compactThreshold": 0.8
}
```

### Shell Integration

Add to your shell profile:

**Bash (~/.bashrc):**
```bash
# Copilot CLI aliases
alias cop='copilot'
alias copb='copilot --banner'

# Start Copilot in project
project-cop() {
  cd "$1" && copilot
}
```

**Zsh (~/.zshrc):**
```zsh
# Copilot CLI completions
autoload -U compinit && compinit

# Quick launch
alias c='copilot'
```

**Fish (~/.config/fish/config.fish):**
```fish
# Copilot aliases
alias cop='copilot'

# Function to start in directory
function copin
    cd $argv[1]
    copilot
end
```

## Terminal Enhancement

### Multiline Input Setup

```
> /terminal-setup
```

Configures your terminal for:
- `Shift+Enter` for new lines
- `Ctrl+Enter` to submit
- Better text editing

### Terminal Compatibility

**Recommended terminals:**
- **macOS:** iTerm2, Terminal.app
- **Linux:** GNOME Terminal, Konsole, Alacritty
- **Windows:** Windows Terminal, PowerShell

### Terminal Theme

Match CLI theme with terminal:

```
> /theme set auto    # Follow system
> /theme set dark    # Force dark
> /theme set light   # Force light
```

## Debugging and Logging

### Enable Debug Mode

```bash
export COPILOT_LOG_LEVEL=debug
copilot
```

### View Logs

```bash
# Default log location
tail -f ~/.copilot/logs/copilot.log

# Custom log file
export COPILOT_LOG_FILE=/tmp/debug.log
copilot
```

### Verbose Output

```
> Set verbose mode

> Enable detailed logging
```

## Performance Optimization

### Faster Responses

```
# Use faster model
> /model claude-haiku-4.5

# Reduce context
> /compact

# Clear unnecessary history
> /clear
```

### Context Management

```
# Monitor context
> /context

# Auto-compact at 80%
Set in config:
{
  "autoCompact": true,
  "compactThreshold": 0.8
}
```

## Security Best Practices

### Credential Management

❌ **Never:**
```
> My API key is abc123...
> The database password is pass123
```

✅ **Instead:**
```
> Use the API key from environment variable
> Read database password from .env file
```

### File Access Control

```
# Review allowed directories
> /list-dirs

# Be cautious adding directories
> /add-dir /sensitive/data  # Think twice!
```

### Code Review

Always review AI-generated:
- Authentication code
- Security-related changes
- Database queries
- API endpoints
- File operations

## Integration Examples

### Example 1: Database MCP

```json
{
  "postgres": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-postgres"],
    "env": {
      "DATABASE_URL": "postgresql://localhost/mydb"
    }
  }
}
```

Usage:
```
> Query the users table for accounts created this week

> Show me the database schema

> Run this SQL query: ...
```

### Example 2: Custom Skill

Create `~/.copilot/skills/api-expert.skill.md`:
```markdown
# API Expert Skill

## Capabilities
- RESTful API design
- OpenAPI/Swagger specs
- API security
- Rate limiting
- Versioning strategies

## Instructions
When designing APIs:
- Use plural nouns for resources
- Implement proper HTTP status codes
- Include pagination for collections
- Add rate limiting headers
- Version via URL or headers
- Document with OpenAPI 3.0
```

### Example 3: Project Instructions

`.github/copilot-instructions.md`:
```markdown
# Project: E-Commerce Platform

## Architecture
- Microservices with NestJS
- PostgreSQL database
- Redis for caching
- RabbitMQ for messaging

## Code Standards
- Use dependency injection
- Repository pattern for data
- DTOs for all endpoints
- Guards for authorization
- Interceptors for logging

## Testing
- Unit tests with Jest
- E2E tests with Supertest
- Minimum 90% coverage
```

## Quick Reference

```bash
# MCP
/mcp show              # View servers
/mcp add <name>        # Add server
/mcp edit <name>       # Edit config

# Skills
/skills list           # Available skills
/skills add <skill>    # Add skill
/skills info <skill>   # Skill details

# Agents
/agent                 # Browse agents

# Configuration
~/.copilot/config.json           # Global config
~/.copilot/mcp-config.json       # MCP servers
~/.copilot/skills/               # Custom skills
~/.copilot/copilot-instructions.md  # Instructions

# Environment
export GH_TOKEN=<token>
export COPILOT_LOG_LEVEL=debug
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS=<paths>
```

---

**Next:** [Plan Mode](09-plan-mode.md)  
**Previous:** [GitHub Integration](07-github-integration.md)
