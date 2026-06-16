# Advanced Features

Explore advanced capabilities of GitHub Copilot CLI including MCP servers, custom agents, skills, and extended configurations.

## Advanced Feature Overview

This section covers MCP servers, custom agents, and the Skills system. For the other major advanced features, see their dedicated guides:

| Feature | Guide | What it does |
|---------|-------|-------------|
| **Autopilot mode** | [17-autopilot-mode.md](17-autopilot-mode.md) | Autonomous end-to-end task execution |
| **Fleet mode** | [18-fleet-mode.md](18-fleet-mode.md) | Parallel subagent execution |
| **Research command** | [19-research-command.md](19-research-command.md) | Deep investigation reports |

## Model Context Protocol (MCP)

MCP is an open protocol that allows Copilot CLI to connect to external data sources and tools. This section is a complete setup guide — from the built-in GitHub MCP server to writing your own.

### What is MCP?

**Model Context Protocol** enables:
- Database connections
- API integrations
- Custom tool creation
- Domain-specific knowledge
- Third-party service access

---

### GitHub MCP Server (Built-in)

The GitHub MCP server ships **built-in** with Copilot CLI. You don't need to install anything — it's active automatically and gives the AI access to:
- Pull request operations (create, review, merge, comment)
- Issue management (create, update, close, search)
- Repository search and metadata
- Code search across GitHub

By default, only a subset of GitHub tools are enabled. You can expand them:

```bash
# Enable ALL GitHub MCP tools
copilot --enable-all-github-mcp-tools

# Enable specific toolsets
copilot --add-github-mcp-toolset=issues
copilot --add-github-mcp-toolset=pull_requests

# Enable individual tools
copilot --add-github-mcp-tool=create_issue
copilot --add-github-mcp-tool=search_repositories
```

> **v1.0.56:** When `gh` CLI is on PATH, the GitHub MCP server automatically omits tools that duplicate `gh` CLI capabilities. This reduces token usage by keeping only tools that aren't already covered by the `gh` CLI. Use `--enable-all-github-mcp-tools` to override and load everything.

**Common usage with the GitHub MCP active:**

```
> Create an issue for the login bug we just found
> Show me all open PRs in this repo
> Find repositories using Next.js and Prisma
> Summarise the last 5 commits on the main branch
> Search GitHub for examples of rate-limiting middleware in Express
```

---

### Setting Up an External MCP Server (PostgreSQL Example)

**Step 1 — Install the server package:**

```bash
npm install -g @modelcontextprotocol/server-postgres
```

**Step 2 — Add to `~/.copilot/mcp-config.json`:**

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres",
               "postgresql://localhost/mydb"],
      "env": {
        "PGPASSWORD": "yourpassword"
      }
    }
  }
}
```

**Step 3 — Verify the server is running:**

```
> /mcp show
```

**Step 4 — Use it in prompts:**

```
> Show me the schema of the users table
> How many active users signed up this month?
> Find all orders over $100 from the last 7 days
> Which products have never been ordered?
```

---

### Setting Up a Filesystem MCP Server

Gives the AI access to directories outside your working directory:

```bash
npm install -g @modelcontextprotocol/server-filesystem
```

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem",
               "/path/to/allowed/directory"]
    }
  }
}
```

```
> Search all files in /opt/logs for ERROR entries from today
> Show me the contents of /etc/nginx/nginx.conf
```

---

### Building a Simple Custom MCP Server

Use the MCP SDK to expose any data source or tool to Copilot:

```javascript
// my-mcp-server.js
import { Server } from '@modelcontextprotocol/sdk/server/index.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new Server(
  { name: 'my-server', version: '1.0.0' },
  { capabilities: { tools: {} } }
);

server.setRequestHandler('tools/list', async () => ({
  tools: [{
    name: 'get_deploy_status',
    description: 'Get the current deployment status',
    inputSchema: { type: 'object', properties: {} }
  }]
}));

server.setRequestHandler('tools/call', async (req) => {
  if (req.params.name === 'get_deploy_status') {
    // your logic here
    return { content: [{ type: 'text', text: 'Deployed: v2.1.0 at 14:32 UTC' }] };
  }
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

Register it in `~/.copilot/mcp-config.json`:

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["/path/to/my-mcp-server.js"]
    }
  }
}
```

---

### MCP Config Source: `.mcp.json` Only (v1.0.22+)

As of v1.0.22, Copilot CLI reads project-level MCP server configuration **only from `.mcp.json`** in the project root. The CLI no longer reads `.vscode/mcp.json` or `.devcontainer/devcontainer.json` as MCP config sources.

If your project has a `.vscode/mcp.json` without a `.mcp.json`, the CLI will show a migration hint at startup that links to platform-specific documentation. To migrate manually:

```bash
cp .vscode/mcp.json .mcp.json
```

> The global `~/.copilot/mcp-config.json` is unaffected — it continues to work as before.

### Auto-load from `.github/mcp.json` (v1.0.61+)

In addition to `.mcp.json` in the project root, Copilot CLI now automatically loads MCP server definitions from `.github/mcp.json`. This lets teams commit a shared MCP server configuration alongside other `.github/` files (Actions workflows, Copilot settings, etc.).

```json
// .github/mcp.json
{
  "mcpServers": {
    "team-tools": {
      "command": "npx",
      "args": ["-y", "@myorg/team-mcp-tools"]
    }
  }
}
```

Both `.mcp.json` and `.github/mcp.json` are loaded when present; definitions from both files are merged.

---

### `deferTools` — Keep Server Tools Always Available (v1.0.63+)

When [tool search](https://docs.github.com/en/copilot) is enabled, Copilot may filter out MCP tools to reduce token usage. Add `"deferTools": true` to any server entry to ensure that server's tools are **always** included in the context, regardless of tool search settings:

```json
{
  "mcpServers": {
    "audit-logger": {
      "command": "npx",
      "args": ["-y", "@myorg/audit-mcp"],
      "deferTools": true
    }
  }
}
```

**When to use:** For servers whose tools must never be filtered out — audit loggers, compliance checkers, or any tool the model must always be able to call.

---

### MCP Server Config Form: Picker-Based Flow (v1.0.62+)

The interactive MCP server configuration form has been redesigned with a picker-based flow, making it easier to add and configure new servers. Select server type, fill in values, and confirm — all from within the CLI without hand-editing JSON.

```
> /mcp add
```

The picker guides you through each required field and validates input before saving to `.mcp.json`.

---

### Remote MCP Server Config: `type` Field Optional (v1.0.29+)

When configuring a remote HTTP MCP server, the `type` field can now be omitted — it defaults to `http`:

```json
{
  "mcpServers": {
    "my-remote-server": {
      "url": "https://my-mcp-server.example.com/mcp"
    }
  }
}
```

Existing configs that specify `"type": "http"` continue to work. The shorthand is especially convenient for quickly adding remote servers.

---

### `COPILOT_AGENT_SESSION_ID` in Shell Commands and MCP Servers (v1.0.29+)

Shell commands and MCP servers invoked by Copilot CLI automatically receive the `COPILOT_AGENT_SESSION_ID` environment variable. This lets your scripts and server implementations trace which CLI session called them:

```bash
# Inside a shell command or MCP server process:
echo $COPILOT_AGENT_SESSION_ID
# → e.g., "abc123-def456-789"
```

Useful for logging, metrics, or coordinating with external systems that need to track Copilot activity by session.

---

### Session-Only MCP Servers

Add a temporary MCP server without modifying your config file — useful for CI runs or one-off tasks:

```bash
copilot --additional-mcp-config='{"mcpServers":{"temp-db":{"command":"npx","args":["-y","@mcp/postgres","postgresql://localhost/testdb"]}}}'
```

### ACP Client MCP Servers (v1.0.25+)

ACP (Agent Communication Protocol) clients can now supply MCP servers (stdio, HTTP, or SSE transport) when starting or loading a session. This lets external tools and integrations inject their own MCP tooling into the CLI session without any manual config changes.

---

### MCP Management Commands

In addition to the `/mcp` slash commands available inside a session, Copilot CLI provides a top-level `copilot mcp` command for managing MCP servers directly from your shell (v1.0.21+):

```bash
$ copilot mcp          # Show help and available subcommands
```

**Inside a session**, use the `/mcp` slash commands:

```
> /mcp show                    # List configured servers and status
> /mcp add my-server           # Add a server interactively
> /mcp install                 # Browse the MCP registry and install a server with guided configuration
> /mcp edit my-server          # Edit server configuration
> /mcp disable my-server       # Disable without removing (persists across sessions)
> /mcp enable my-server        # Re-enable a disabled server (persists across sessions)
> /mcp delete my-server        # Remove permanently
> /mcp reload                  # Reload all MCP server configs
> /mcp auth my-server          # Authenticate / re-authenticate an MCP OAuth server (v1.0.15+)
```

> **v1.0.55+:** `/mcp` shows **per-server token usage** so you can identify which servers are contributing most to context size. `/context` also breaks out MCP tool tokens as a separate line item.

> **v1.0.55+:** MCP configuration now opens in a **dedicated full-screen view** with scrollable server and tool lists when content exceeds the visible area — no more truncated output in the inline panel.

**Installing from the registry (v1.0.25+):** `/mcp install` opens an interactive browser of the MCP server registry. Select a server, answer the prompted configuration questions, and the CLI adds it to `~/.copilot/mcp-config.json` automatically — no manual JSON editing required.

---

### Available MCP Servers

Common off-the-shelf servers:

| Package | Purpose |
|---------|---------|
| `@modelcontextprotocol/server-postgres` | PostgreSQL database |
| `@modelcontextprotocol/server-sqlite` | SQLite database |
| `@modelcontextprotocol/server-filesystem` | Enhanced file access |
| `@modelcontextprotocol/server-memory` | Persistent cross-session memory |
| `@modelcontextprotocol/server-github` | GitHub API (built-in) |

---

### MCP Server Names with Spaces (v1.0.35+)

Server names with spaces and special characters are now fully supported in MCP config. Quote the name in JSON as normal:

```json
{
  "mcpServers": {
    "my postgres db": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]
    }
  }
}
```

> **v1.0.42+:** When a server with a space in its name fails to connect, the error hint shows a directly runnable `/mcp show "my postgres db"` command (correctly quoted) so you can inspect it immediately.

---

### MCP Failure Warnings Include stderr (v1.0.42+)

When an MCP server fails to start, the connection failure warning now includes the server's **stderr output** inline. This surfaces the root cause (missing environment variable, wrong port, authentication error, etc.) without requiring you to run the server binary manually.

### OpenTelemetry: GenAI Semantic Conventions for MCP Tool Calls (v1.0.45+)

OpenTelemetry output now conforms to the GenAI semantic conventions:

- **MCP tool calls** emit standard `tool_call` spans (previously used custom span types).
- **New `gen_ai.client.operation.duration` metric** tracks tool execution time.

This improves out-of-the-box compatibility with observability platforms (Datadog, Honeycomb, Jaeger, etc.) that consume the standard GenAI conventions.

### Headless OAuth for MCP Servers: `client_credentials` (v1.0.40+)

MCP servers that require OAuth can now authenticate using the **`client_credentials` grant type**, enabling fully headless authentication without a browser. Ideal for CI/CD pipelines and server environments.

```json
{
  "mcpServers": {
    "my-api-server": {
      "url": "https://my-mcp-server.example.com/mcp",
      "auth": {
        "type": "oauth2",
        "grant_type": "client_credentials",
        "client_id": "your-client-id",
        "client_secret": "your-client-secret",
        "token_url": "https://auth.example.com/oauth/token"
      }
    }
  }
}
```

---

### Experimental MCP Tasks (v1.0.41+)

MCP tools that declare `taskSupport: "required"` in their capability metadata now run as **non-blocking background agents** when experimental mode is enabled. This lets long-running MCP operations happen in the background while you continue working.

**Enabling experimental mode:**

```
> /experimental on
```

or at launch:

```bash
copilot --experimental
```

**Tracking background tasks:**

Once running, background MCP tasks are visible and controllable through the agent task tools:

```
> /tasks
```

Or programmatically via the `list_agents` and `read_agent` tools exposed to the model.

**Why it matters:** Compute-heavy MCP tools (e.g., long builds, data processing jobs) no longer block your session. You get results asynchronously when the task finishes.

---

When Copilot CLI detects an **Azure DevOps repository** as the working context, the built-in GitHub MCP server is automatically scaled back to avoid authentication errors and irrelevant GitHub API calls. No configuration change is needed — the detection is automatic. As of v1.0.48, this also applies when running in prompt (`-p`) or headless mode, matching the existing interactive-mode behavior. As of v1.0.57, instead of being fully disabled, the server now exposes **only the `web_search` tool** in Azure DevOps-only repositories, so web search capability is retained.

---

### MCP Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| Server not showing in `/mcp show` | Config file malformed | Validate JSON in `~/.copilot/mcp-config.json` |
| "Connection failed" | Server binary not found | Check `command` path; ensure package is installed |
| Tools not available to model | Server disabled | Run `/mcp enable server-name` |
| Credentials exposed in config | Hardcoded passwords | Use `env` block with environment variable references |
| Server crashes on startup | Incompatible version | Check MCP SDK version compatibility |
| Server loses auth after `/mcp reload` or login | Auth state not persisted | Upgrade to v1.0.16+; servers now reload auth correctly |
| OAuth provider rejects redirect URI | Provider requires HTTPS | v1.0.17+ automatically falls back to a self-signed HTTPS certificate |
| MCP tool names with dots or special characters cause failures | Invalid tool name format | Upgrade to v1.0.40+; tool names are sanitized automatically |
| OAuth tokens lost when multiple servers share same URL | Client ID collision in cache | Upgrade to v1.0.40+; tokens are keyed by URL + client ID |
| Tools silently fail with certain models | Non-standard JSON schema | Upgrade to v1.0.22+; schemas are now sanitized automatically |
| Remote server drops connection on network hiccup | Transient network failure | Upgrade to v1.0.25+; remote MCP connections now automatically retry |
| MCP failure warning gives generic `/mcp show` hint for servers with spaces in their name | Server name quoting | Upgrade to v1.0.42+; hint is now a directly runnable quoted command |
| Orphaned MCP child processes (npx/uvx) linger after session ends | Child process not fully cleaned up | Upgrade to v1.0.43+; child processes are fully terminated on session exit |

## LSP (Language Server Protocol) Support

Copilot CLI integrates with LSP servers to provide code intelligence — giving the AI richer context about your codebase when generating or editing code.

### What LSP Provides

- **Go-to-definition**: Resolve symbols to their declarations
- **Hover information**: Type signatures and documentation for identifiers
- **Diagnostics**: Real-time errors and warnings from the language server
- **Completion context**: Accurate type-aware suggestions

### Installing LSP Servers

LSP servers are not bundled with Copilot CLI — install them separately for each language you work with:

```bash
# TypeScript / JavaScript
npm install -g typescript-language-server typescript

# Python
pip install python-lsp-server

# Go
go install golang.org/x/tools/gopls@latest

# Rust
rustup component add rust-analyzer
```

### Configuration Locations

LSP servers can be configured at two levels:

| Level | Path |
|-------|------|
| User (global) | `~/.copilot/lsp-config.json` |
| Repository | `.github/lsp.json` |

Repository-level config takes precedence and is useful for project-specific language server settings.

### Example Configuration

**`~/.copilot/lsp-config.json`:**
```json
{
  "lspServers": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".ts": "typescript",
        ".tsx": "typescript"
      },
      "spawnTimeout": 10000,
      "initializationTimeout": 30000,
      "warmupTimeout": 5000
    },
    "python": {
      "command": "pylsp",
      "args": [],
      "fileExtensions": {
        ".py": "python"
      }
    },
    "go": {
      "command": "gopls",
      "args": [],
      "fileExtensions": {
        ".go": "go"
      }
    }
  }
}
```

**LSP timeout fields (v1.0.35+):**

| Field | Default | Description |
|-------|---------|-------------|
| `spawnTimeout` | (system default) | Milliseconds to wait for the LSP server process to start |
| `initializationTimeout` | (system default) | Milliseconds to wait for the `initialize` handshake to complete |
| `warmupTimeout` | (system default) | Milliseconds to wait for the server to warm up before the first request |

**Additional server config keys (v1.0.60):**

| Field | Description |
|-------|-------------|
| `bash` | Shell command string to launch the server via a Bash wrapper |
| `powershell` | Shell command string to launch the server via a PowerShell wrapper |
| `cwd` | Working directory for the server process; supports plugin vars like `PLUGIN_ROOT`; defaults to the project root unless set |

### Checking LSP Status

```
> /lsp
```

Displays:
- Configured language servers
- Server status (running / stopped / error)
- File extensions mapped to each server

### Benefits in Practice

With LSP active, Copilot CLI can:
```
# Resolve types accurately when editing
> Refactor this function to use the correct return type

# Use real diagnostics as context
> Fix all the TypeScript errors in this file

# Navigate large codebases more effectively
> Where is the UserService class defined?
```

---

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

Custom agents are defined in your `.copilot/` directory or referenced via the `/agent` command. They let you create specialized personas with specific instructions, tools, and model preferences.

> **v1.0.62+:** Custom agents in nested `.github/agents` and `.claude/agents` directories are automatically discovered when the session is started from any subdirectory of the repository root — you no longer need to be in the exact root directory.

**Built-in agents available to all sessions:**

| Agent | Best for |
|-------|---------|
| `explore` | Codebase questions without polluting main context |
| `task` | Running builds, tests, lints — clean success/failure output |
| `general-purpose` | Complex multi-step tasks needing full reasoning |
| `code-review` | Security, bugs, logic — high signal, no style comments |

**Custom agent use cases:**
- `@test-writer` — specialized in your test framework and conventions
- `@doc-generator` — generates JSDoc/docstrings following your style guide
- `@security-auditor` — focused on OWASP, SQL injection, auth patterns
- `@migration-helper` — knows your DB schema and migration patterns

**Using custom agents with fleet:**
```
> /fleet Use @test-writer to add tests for all files in src/services/,
         use @doc-generator to document src/utils/
```

See [Fleet Mode — Specialisation](18-fleet-mode.md#specialisation-custom-agents-and-models) for detailed examples.

Custom agents extend capabilities with:
- Specialized prompts
- Custom tools
- Domain knowledge
- Specific workflows

### Eager Skill Loading via `skills` Field (v1.0.22+)

Custom agents can declare a `skills` field in their frontmatter to pre-load skill content into the agent's context at startup:

```yaml
---
name: backend-agent
model: claude-sonnet-4.6
skills:
  - python-expert
  - django-expert
  - security-audit
---
This agent specialises in Django backend development with security best practices.
```

Skills listed here are injected before the first prompt — the agent starts with full domain expertise without requiring manual `/skills add` steps.

> See [Skills System Guide](14-skills-system.md) for available skill names and file format.

### Deferred Tool Loading (v1.0.52+)

Custom agents with many MCP servers or plugins can opt into deferred tool loading to speed up startup. Set `deferred-tool-loading: true` in the agent's frontmatter to enable tool-search discovery instead of eagerly loading all tool descriptions at initialization.

```yaml
---
name: large-toolset-agent
model: claude-sonnet-4.6
deferred-tool-loading: true
---
This agent loads tools on demand, keeping startup fast even with large tool lists.
```

**Why it matters:** Agents with large tool lists start faster and avoid exceeding context limits from unused tool descriptions. The agent discovers and loads tools dynamically as they are needed.

## Skills System

**Skills** are modular expertise packages that add specialized capabilities to Copilot CLI. Unlike AGENTS.md (project-specific) or instruction files (style guides), skills provide reusable domain expertise that can be activated across any project.

### What Skills Provide

- **Domain expertise**: Python expert, React patterns, security auditing
- **Cross-project reusability**: Use the same skill in different projects
- **On-demand activation**: Enable only the skills you need
- **Specialized knowledge**: Deep expertise in specific technologies

### Key Differences

```
Skills:              AGENTS.md:           Instructions:
─────────────       ──────────────       ─────────────
✅ Reusable          ❌ Project only      ❌ Project only
✅ Enable/disable    ❌ Always active     ❌ Always active
✅ Domain expertise  ✅ Project config    ✅ Code style
📦 "Python expert"   🏗️  Architecture    📝 Formatting
```

### Basic Usage

```
# List available skills
> /skills list

# Activate a skill
> /skills add python-expert

# Get information about a skill
> /skills info security-audit

# Deactivate a skill
> /skills remove python-expert

# Reload after editing skill files
> /skills reload
```

### Example Workflow

```
# Working on Python API
> /skills add python-expert
> /skills add fastapi-expert
> Create a FastAPI endpoint for user registration

# Switch to React frontend
> /skills remove python-expert
> /skills remove fastapi-expert
> /skills add react-patterns
> /skills add typescript-expert
> Create a user registration form component
```

### Creating Custom Skills

Skills are stored in:
```
~/.copilot/skills/           # Global skills
<project>/.copilot/skills/   # Project-specific
```

Minimal skill file `~/.copilot/skills/my-skill.skill.md`:
```markdown
# My Custom Skill

## Metadata
- **ID**: my-skill
- **Version**: 1.0.0
- **Category**: Development
- **Tags**: python, backend

## Description
Brief description of the skill's purpose.

## Capabilities
- What this skill can do
- Expertise it provides
- Problems it solves

## Instructions

### Core Principles
[Key principles this skill follows]

### Patterns
```python
# ✅ Preferred pattern
def example():
    pass

# ❌ Avoid this
def bad_example():
    pass
```

### Best Practices
- Practice 1
- Practice 2

## Checklists
- [ ] Check 1
- [ ] Check 2
```

**For comprehensive guidance on creating and using skills, see the [Skills System Guide](14-skills-system.md).**

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

## Experimental Mode & Autopilot

Copilot CLI includes an experimental mode that unlocks cutting-edge features before they reach general availability.

### Activating Experimental Mode

**From the command line:**
```bash
copilot --experimental
```

**From within the CLI:**
```
> /experimental
```

Once activated, the setting persists in your config — you don't need to pass `--experimental` on every launch.

### Autopilot Mode

Autopilot is an experimental interaction mode that encourages Copilot to keep working autonomously until a task is fully complete, with minimal back-and-forth.

**Cycling through modes:**

Press `Shift+Tab` to cycle between modes. With experimental mode enabled, autopilot is added to the cycle:

| Mode | Behavior |
|------|----------|
| **Interactive** (default) | Asks for confirmation before each significant action |
| **Plan** | Produces a plan first, then acts after approval |
| **Autopilot** | Continues working until the task is done; minimal interruptions *(requires `/experimental`)* |

> **Note:** Without experimental mode active, `Shift+Tab` only cycles between interactive and plan.

### When to Use Autopilot

✅ **Good for:**
- Long refactoring tasks across many files
- Generating boilerplate or scaffolding
- Writing comprehensive tests for existing code
- Multi-step tasks with a clear end goal

❌ **Avoid for:**
- Security-sensitive changes
- Tasks requiring nuanced human judgment at each step
- Exploratory work where requirements are unclear

### Example

```
# Switch to autopilot, then describe a complex task
[Shift+Tab → Autopilot]

> Refactor the authentication module to use JWT tokens,
  update all tests, and fix any type errors that come up

[Copilot works through the task end-to-end without prompting]
```

---

## Fleet Mode

Fleet mode enables parallel subagent execution — multiple background agents working on different tasks simultaneously.

### Sub-Agent Depth and Concurrency Limits (v1.0.22+)

To prevent runaway agent trees, the CLI enforces:

- **Depth limit** — maximum nesting level for spawned agents (agent → sub-agent → sub-sub-agent)
- **Concurrency limit** — maximum number of agents running in parallel at any time

When either limit is reached, the CLI surfaces a clear error rather than silently queuing more agents. These limits apply to fleet tasks, autopilot delegation chains, and any other sub-agent spawning.

### Enabling Fleet Mode

```
> /fleet
```

This activates fleet mode and allows Copilot to spin up parallel subagents.

### Viewing and Managing Tasks

```
> /tasks
```

Shows all active background tasks, including:
- Running subagents and their current work
- Background shell sessions
- Task status (running / completed / failed)
- Output from each task

### What Fleet Mode Is Good For

- **Large refactors**: Run code changes, test updates, and documentation in parallel
- **Multi-component changes**: Work on frontend, backend, and database layers simultaneously
- **Research + implementation**: One agent researches an API while another writes the code
- **Independent analysis**: Analyze different parts of a codebase at the same time

### Example Workflow

```
> /fleet

# Kick off parallel tasks
> Task 1: Migrate the users module to TypeScript
> Task 2: Write unit tests for the auth service
> Task 3: Update the API documentation

# Monitor progress
> /tasks

Tasks:
  [1] users-migration    ● running   - Converted 12/18 files
  [2] auth-tests         ✓ done      - 24 tests written
  [3] api-docs           ● running   - Updating endpoints
```

---

## Plugin System

Copilot CLI supports a plugin system that extends its capabilities through installable packages.

### Managing Plugins

```
> /plugin
```

Opens the plugin manager, where you can:
- **Browse** available plugins from configured marketplaces
- **Install** plugins to add new tools, slash commands, or integrations
- **Uninstall** plugins you no longer need
- **Update** plugins to their latest versions

### Plugin Marketplaces

Plugins are sourced from marketplaces. You can manage marketplace sources from within the plugin manager:

```
> /plugin
[Select "Manage marketplaces"]
```

This lets you:
- Add marketplace URLs
- Remove marketplaces
- Refresh the plugin catalog

You can also refresh catalogs from the shell without starting a session:

```bash
copilot plugin marketplace update
```

When configuring marketplaces in `config.json`, use the `extraKnownMarketplaces` key:

```json
{
  "extraKnownMarketplaces": ["https://plugins.example.com/registry.json"]
}
```

> ⚠️ **Removed in v1.0.16:** The `marketplaces` config key has been removed. Use `extraKnownMarketplaces` instead.

### What Plugins Can Add

- New slash commands (e.g., `/deploy`, `/storybook`)
- MCP server integrations packaged as one-click installs
- Domain-specific tools and workflows
- Custom UI panels or output formatters

---

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

### HTTP Hooks (v1.0.35+)

In addition to shell-script hooks, hooks can POST JSON payloads to a configured URL. This is useful for centralising hook logic on a server — auditing tool usage, enforcing team policies, or integrating with CI systems.

**Configuration:**

```json
{
  "hooks": {
    "preToolUse": [
      {
        "url": "https://hooks.example.com/copilot/pre-tool",
        "matcher": { "tool_name": "shell" }
      }
    ],
    "postToolUse": [
      {
        "url": "https://audit.example.com/copilot/tool-events"
      }
    ]
  }
}
```

- The hook endpoint receives the same JSON payload that shell-based hooks get via stdin.
- The endpoint should respond with the same JSON structure that shell hooks return via stdout.
- The `url` and `command` (shell script) fields are mutually exclusive per hook entry.
- **`matcher` behaviour (v1.0.36+):** A hook entry with a `matcher` fires **only when the tool name fully matches the regex**. Before v1.0.36 the `matcher` field was ignored and hooks always fired. Verify your matchers after upgrading.
- **`matcher` now correctly honored (v1.0.63+):** `postToolUse` hook matchers (e.g. `Edit|Write`) are now enforced correctly — formatters and linters run only after the tools they target, instead of firing after every tool call. If your hooks were inadvertently running too broadly, check your matchers after upgrading to v1.0.63.
- **`additionalContext` in `postToolUse` (v1.0.49+):** A `postToolUse` hook can return an `additionalContext` field. It is now injected as a system message that the model receives after the tool call, allowing post-tool hooks to meaningfully influence the model's next response. Previously this field was silently discarded.
- **`postToolUse` in successful results (v1.0.51+):** `postToolUse` hooks can now inject `additionalContext` into successful tool results, not just failed ones.
- **`preMcpToolCall` hook (v1.0.51+):** A new `preMcpToolCall` event fires before each outgoing MCP tool call. Hook providers can use it to inspect or modify outgoing request metadata (e.g., add tracing headers, enforce policies).

> See [New Features v1.0.35 → HTTP Hook Support](16-new-features.md#http-hook-support) for a full walkthrough.

### `userPromptSubmitted` Hook — Bypass the LLM (v1.0.44+)

The `userPromptSubmitted` event fires when the user submits a prompt, **before the model is called**. A hook script can inspect the prompt and return a direct response, bypassing the LLM entirely for that turn.

**Configuration:**
```json
{
  "hooks": {
    "userPromptSubmitted": [
      {
        "command": "~/.copilot/hooks/prompt-router.sh"
      }
    ]
  }
}
```

**How it works:**
- The hook receives the user prompt via stdin as a JSON payload.
- If the script exits `0` and writes a non-empty string to stdout, that string is used as the AI reply — no model call is made.
- If the script exits non-zero (or writes nothing), the prompt continues to the LLM as normal.

**Use cases:** Instant responses to common questions, template-based replies, local tool integrations without consuming model quota.

> **v1.0.45+:** The `agentStop` hook now fires correctly when the agent stops via `task_complete`. Previously it was not triggered in that code path.

---

### Shell Integration

> ⚠️ **Breaking change in v1.0.62:** Shell commands now run via **lightweight process spawning** instead of a pseudo-terminal. **Interactive input via `write_bash` is no longer supported.** If your workflows use `write_bash` to send keystrokes to a running shell session, rewrite them to use non-interactive equivalents (e.g., pass `-y` flags, pipe input with `echo`, or restructure the command).

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

# LSP
/lsp                   # Check configured servers and status

# Skills
/skills list           # Available skills
/skills add <skill>    # Add skill
/skills info <skill>   # Skill details

# Agents
/agent                 # Browse agents

# Experimental & Autopilot
copilot --experimental # Launch with experimental features
/experimental          # Enable experimental mode in-session
Shift+Tab              # Cycle: interactive → plan (→ autopilot with /experimental)

# Fleet Mode
/fleet                 # Enable parallel subagent execution
/tasks                 # View and manage background tasks

# Plugins
/plugin                # Open plugin manager (browse/install/uninstall)

# Configuration
~/.copilot/config.json              # Global config
~/.copilot/mcp-config.json          # MCP servers
~/.copilot/lsp-config.json          # LSP servers (user-level)
.github/lsp.json                    # LSP servers (repo-level)
~/.copilot/skills/                  # Custom skills
~/.copilot/copilot-instructions.md  # Instructions

# Environment
export GH_TOKEN=<token>
export COPILOT_LOG_LEVEL=debug
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS=<paths>
```

---

**Next:** [Plan Mode](09-plan-mode.md)  
**Previous:** [GitHub Integration](07-github-integration.md)
