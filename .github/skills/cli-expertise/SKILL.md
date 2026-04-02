---
name: cli-expertise
description: Deep expertise in all GitHub Copilot CLI features, workflows, and best practices.
---

# CLI Expertise Skill

## Metadata
- **ID**: cli-expertise
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: Development
- **Tags**: copilot, cli, terminal, ai-assistant, github

## Description

Deep expertise in all GitHub Copilot CLI features, workflows, and best practices. Enables comprehensive guidance on using every aspect of the CLI effectively — from basic slash commands to advanced MCP servers, agent configuration, fleet mode, and GitHub integration.

## Capabilities

- Slash commands mastery (complete reference)
- File context management with `@` syntax
- Plan mode usage and workflows
- Agent configuration via AGENTS.md
- MCP server setup and usage
- Skills system management
- GitHub integration (PRs, issues, reviews)
- Autopilot and fleet subagent patterns
- Model selection and tradeoffs
- Troubleshooting common issues

## When to Use

- User asks about any Copilot CLI feature or command
- User wants to learn best practices for CLI workflows
- User is stuck and needs troubleshooting guidance
- User wants to understand how a feature works
- Companion skill during the quickstart tutorial for deep Q&A answers

## Instructions

### Core Interface

The Copilot CLI interface has a few key elements to understand:

- **Prompt location:** The input prompt is at the **bottom** of the screen; AI responses stream upward above it
- **Mode cycling:** `Shift+Tab` cycles through modes: **Interactive** → **Plan** → **Shell**
- **Shell shortcut:** Prefix any message with `!` for instant shell mode (e.g., `!ls`, `!git status`, `!cat package.json`) — no mode switch needed
- **Cancel / exit:** `Ctrl+C` cancels the current response; `Ctrl+D` exits the CLI
- **Clear screen:** `Ctrl+L` clears the conversation display (history is preserved)
- **History navigation:** `↑` / `↓` arrows cycle through prompt history

### Slash Commands — Complete Reference

| Command | What it does |
|---------|-------------|
| `/help` | Show all available commands and keyboard shortcuts |
| `/clear` | Clear the conversation history and start fresh |
| `/model` | Switch the AI model interactively |
| `/diff` | Show uncommitted file changes (like `git diff`) |
| `/plan` | Enter Plan mode — Copilot drafts a plan for you to approve |
| `/compact` | Summarize the conversation to reduce token usage while preserving context |
| `/context` | Show current token usage and context window status |
| `/update` | Update the CLI to the latest version |
| `/feedback` | Submit feedback or bug reports to the Copilot team |
| `/skills` | Manage skills: `list`, `add <name>`, `remove <name>`, `reload`, `info <name>` |
| `/init` | Initialize an AGENTS.md file in the current project |
| `/delegate` | Delegate a task to a subagent (fleet mode) |
| `/rewind` | Undo the last conversation turn |
| `/undo` | Alias for `/rewind` |
| `/streamer-mode` | Toggle streamer mode — hides sensitive information for screen sharing |
| `/research` | Run a deep investigation report on a topic |
| `/autopilot` | Enter autonomous task execution mode |

### File Context with `@` Syntax

The `@` syntax lets you include file content directly in your message:

**Basic usage:**
```
> Review @app.js for security issues
> Explain what @src/auth/token.ts does
> Compare @old-config.json and @new-config.json
```

**Multiple files:**
```
> Refactor @models/user.js to match the pattern in @models/product.js
```

**Directory:**
```
> What does @src/ contain?
```

**How it works:**
- Type `@` and Copilot autocompletes files from the current directory
- File contents are injected into the context window at send time
- Large files may be truncated — prefer focused, relevant files
- Combine with specific questions for best results: `@filename.js` alone is less effective than `Review @filename.js for X`

### Plan Mode

Plan mode lets Copilot draft a step-by-step implementation plan that you review before any changes are made.

**Entering Plan mode:**
- `/plan` command
- `Shift+Tab` twice from Interactive mode
- Copilot may automatically suggest it for complex tasks

**How it works:**
1. You describe the task
2. Copilot generates a structured plan (saved to `~/.copilot/session-state/<id>/plan.md`)
3. You review the plan
4. **Approve** → Copilot executes the steps
5. **Reject / edit** → Revise and try again

**Best for:**
- Complex multi-step refactoring
- Building new features from scratch
- Tasks that touch many files
- Any time you want to review before Copilot acts

**Tips:**
- Be specific in your task description — vague requests produce vague plans
- You can edit the plan text before approving
- Large plans can be broken into phases

### AGENTS.md

The AGENTS.md file tells Copilot about your project so it behaves correctly from the start.

**Location priority** (Copilot checks in this order):
1. Project root: `AGENTS.md`
2. GitHub directory: `.github/AGENTS.md`
3. Copilot directory: `.copilot/AGENTS.md`

**Auto-generation:**
```
> /init
```
Copilot inspects the project and generates an AGENTS.md with architecture overview, tech stack, conventions, and instructions.

**What to include:**
- Project purpose and tech stack
- Directory structure explanation
- Coding conventions (naming, formatting, patterns)
- Testing instructions
- What NOT to do (guardrails)
- Links to key files

**The file is always loaded when present** — no activation needed.

### Skills System

Skills are modular expertise packages stored as `.skill.md` files.

**Directory locations:**
- `~/.copilot/skills/` — global (available in all projects)
- `.copilot/skills/` — project-specific

**Managing skills:**
```
> /skills list          # Show loaded skills
> /skills add <name>    # Activate a skill by name
> /skills remove <name> # Deactivate a skill
> /skills reload        # Reload all skills from disk
> /skills info <name>   # Show skill details
```

**The skill file format:** Markdown with Metadata, Description, Capabilities, and Instructions sections. The Instructions section is the main content the AI uses.

**This repo's skills** are in `.github/` — copy to `~/.copilot/skills/` to activate them.

### MCP Servers (Model Context Protocol)

MCP servers extend the CLI with external tools — databases, APIs, browsers, file systems, and more.

**Config location:** `~/.copilot/mcp-config.json`

**Example config:**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/home/user/projects"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "ghp_..." }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]
    }
  }
}
```

**Common MCP servers:**
| Server | Purpose |
|--------|---------|
| `@modelcontextprotocol/server-filesystem` | Read/write files outside CWD |
| `@modelcontextprotocol/server-github` | GitHub API access (PRs, issues, repos) |
| `@modelcontextprotocol/server-postgres` | PostgreSQL queries |
| `@modelcontextprotocol/server-puppeteer` | Browser automation |
| `@modelcontextprotocol/server-memory` | Persistent key-value memory |

After editing the config, reload with `/mcp reload` or restart the CLI.

### GitHub Integration

Copilot CLI integrates with GitHub via the `gh` CLI under the hood.

**Common GitHub tasks:**
```
> Create a PR for these changes with a description of what I did
> Review PR #123 and summarize the changes
> What issues are assigned to me?
> Create an issue: "Add dark mode support" with label enhancement
> Show me the CI status for the current branch
> Merge PR #456 after checks pass
```

**Requirements:**
- `gh` CLI installed and authenticated (`gh auth login`)
- Must be in a git repository with a GitHub remote

**Supported operations:**
- Create, view, and merge pull requests
- Create, view, and close issues
- View CI/CD check status
- Review and comment on PRs
- Search repositories

### Autopilot Mode

Autopilot enables fully autonomous task execution — Copilot plans AND executes without manual approval at each step.

**How to activate:**
- Use `/autopilot` command
- Or press `Shift+Tab` to reach Plan mode, then choose autopilot execution

**When to use:**
- Well-defined tasks with clear acceptance criteria
- Tasks where you trust Copilot to make reasonable decisions
- Batch operations (e.g., "add JSDoc comments to all functions in src/")

**Safety:**
- Review the task description carefully before confirming
- Copilot will still show you what it did when finished
- Use in a clean git state so you can `git diff` or `git reset` easily

### Fleet Mode (Parallel Subagents)

Fleet mode runs multiple Copilot subagents in parallel for independent tasks.

**How to use:**
```
> /delegate "Add unit tests for the auth module"
> /delegate "Update README with new API endpoints"
```

Each `/delegate` call spawns a subagent that works independently. Results come back to the main session.

**Best for:**
- Tasks that can be split into truly independent parts
- Running analysis on multiple files simultaneously
- Parallel code generation for separate modules

**Tips:**
- Agents don't share state — make each task description self-contained
- Check results carefully; agents may make different style choices
- Works best when tasks don't touch the same files

### Model Selection

Switch models with `/model` or specify one at startup.

**Available models (as of current version):**
| Model | Speed | Quality | Best for |
|-------|-------|---------|---------|
| Claude Sonnet 4.6 | Fast | High | General use, default |
| Claude Opus 4.6 | Slower | Highest | Complex reasoning, architecture |
| GPT-5.4 | Fast | High | General use |
| GPT-5.4 mini | Fastest | Good | Quick tasks, high volume |
| GPT-4.1 | Fast | Good | Cost-effective option |

Full model comparison and cost details: see `22-models-and-costs.md`.

**Tips:**
- Use a faster/cheaper model for quick questions and simple edits
- Switch to a premium model for complex refactoring, architecture decisions, or debugging hard problems
- Model choice persists for the session until changed

### Troubleshooting Quick Reference

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| "Not authenticated" | Token expired | `copilot auth login` |
| Slow responses | Large context | `/compact` to summarize |
| Skills not loading | Wrong directory | Check `~/.copilot/skills/` exists |
| MCP tools missing | Config error | Validate `mcp-config.json` JSON |
| `@file` not completing | File not in CWD | Use full relative path |
| Plan not saving | No session dir | Check `~/.copilot/session-state/` |

Full troubleshooting guide: see `11-troubleshooting.md`.

### Common Pitfalls

- ❌ Using vague prompts like "fix my code" — always specify what's wrong and what you want
- ❌ Including huge files with `@` when only a small function is relevant — excerpt the relevant part
- ❌ Running Autopilot on tasks with unclear acceptance criteria — define "done" explicitly
- ❌ Letting the context get huge without using `/compact` — long contexts degrade response quality
- ❌ Forgetting AGENTS.md — the CLI doesn't know your project's conventions without it

### Best Practices

- ✅ Be specific and include context: "in @auth.js, the `verifyToken` function doesn't handle expired tokens — fix it to return a 401"
- ✅ Use `@` to include relevant files — it dramatically improves response accuracy
- ✅ Use Plan mode for any task that touches more than 2-3 files
- ✅ Keep AGENTS.md updated as your project evolves
- ✅ Use `/compact` when the conversation is more than ~20 turns old
- ✅ Always review diffs before confirming destructive changes
- ✅ Use `/model` to match model to task complexity and cost tolerance

## Related Skills

- `update-cli` — for upgrading and managing CLI versions
- `doc-maintenance` — for keeping this documentation guide current
- `quickstart` — interactive tutorial that uses this skill as a knowledge companion
