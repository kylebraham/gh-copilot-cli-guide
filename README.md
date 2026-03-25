# GitHub Copilot CLI Guide

Welcome to the comprehensive guide for GitHub Copilot CLI! This guide will help you understand and master the GitHub Copilot CLI, bringing AI-powered coding assistance directly to your terminal.

## 📚 Guide Structure

> **In a hurry?** Jump straight to the **[⚡ Cheat Sheet](00-cheat-sheet.md)** for a one-page quick reference of every command, shortcut, and flag.

### Foundation
1. **[Getting Started](01-getting-started.md)** - Installation, authentication, and first steps
2. **[Basic Concepts](02-basic-concepts.md)** - Understanding how the CLI works

### Core Features
3. **[Interactive Features](03-interactive-features.md)** - Working with the interactive interface
4. **[Slash Commands](04-slash-commands.md)** - Complete reference of all commands
5. **[File and Context Management](05-file-context.md)** - Managing files and context
6. **[Code Editing and Development](06-code-editing.md)** - Writing and editing code
7. **[GitHub Integration](07-github-integration.md)** - Working with repos, PRs, and issues

### Advanced Features
8. **[Advanced Features](08-advanced-features.md)** - MCP servers, custom agents, and more
9. **[Plan Mode](09-plan-mode.md)** - Creating implementation plans
17. **[Autopilot Mode](17-autopilot-mode.md)** - Complete guide to autonomous task execution
18. **[Fleet Mode](18-fleet-mode.md)** - Running tasks in parallel with subagents
19. **[Research Command](19-research-command.md)** - Deep investigation reports with `/research`

### Reference
10. **[Best Practices](10-best-practices.md)** - Tips, security, and effective use patterns
11. **[Troubleshooting](11-troubleshooting.md)** - Common issues and solutions
12. **[Examples and Tutorials](12-examples.md)** - Real-world scenarios and end-to-end walkthroughs

### Configuration
13. **[AGENTS.md File Guide](13-agents-file.md)** - Configuring AI agent behavior for your project
14. **[Skills System Guide](14-skills-system.md)** - Using and creating reusable skill modules
15. **[.copilot Directory Guide](15-copilot-directory.md)** - Understanding config, sessions, and data storage

### Teams and Automation
20. **[CI/CD and Automation](20-cicd-automation.md)** - Non-interactive mode, GitHub Actions, scripting
21. **[Team Setup](21-team-setup.md)** - Shared AGENTS.md, onboarding checklists, org configuration
22. **[Models and Costs](22-models-and-costs.md)** - Model comparison, cost optimization, team budget strategy

### What's New
16. **[Latest Features](16-new-features.md)** - Recent additions and experimental features

## 🎯 What is GitHub Copilot CLI?

GitHub Copilot CLI is a terminal-native AI coding assistant that brings the power of GitHub Copilot directly to your command line. It enables you to:

- **Build, edit, and debug code** through natural language conversations
- **Access GitHub resources** (repositories, issues, PRs) seamlessly
- **Execute complex tasks** with AI-powered planning and execution
- **Extend capabilities** with MCP (Model Context Protocol) servers
- **Maintain full control** with preview-before-execution

## 🚀 Quick Start

```bash
# Install with Homebrew (macOS/Linux)
brew install copilot-cli

# Or with npm (all platforms)
npm install -g @github/copilot

# Launch the CLI
copilot
```

### 🔄 Upgrading

Already have Copilot CLI? Upgrade to get the latest features:

```bash
# Homebrew
brew upgrade copilot-cli

# npm
npm update -g @github/copilot

# Or use the built-in update command
> /update
```

**Latest features:**
- 🔄 `/rewind` / `/undo` — undo the last turn and revert file changes
- 🧠 `/context` — visualize context window token usage
- 🗜️ `/compact` — summarize conversation history to save tokens
- ✨ New models: Claude Sonnet 4.6, Claude Opus 4.6, GPT-5.4 mini, GPT-4.1
- ⌨️ Updated keyboard shortcuts (`Ctrl+S`, `Ctrl+T`, `Ctrl+O`, `Ctrl+E`)
- 🎙️ `/streamer-mode` — hide model/quota details while screen-sharing
- 🤖 [Autopilot mode](17-autopilot-mode.md) (experimental) - fully autonomous task execution
- 🌐 [Fleet mode](18-fleet-mode.md) - run subtasks in parallel with subagents
- 🔍 LSP support - deep language-aware code intelligence
- 🔬 [Research command](19-research-command.md) - investigate codebases and summarise findings
- ⚡ [Cheat sheet](00-cheat-sheet.md) - one-page quick reference
- [See full feature details →](16-new-features.md)

## 💡 What You'll Learn

By the end of this guide, you'll be able to:

- Set up and authenticate GitHub Copilot CLI
- Navigate the interactive interface efficiently
- Use natural language to write and modify code
- Integrate with GitHub workflows
- Create implementation plans for complex tasks
- Customize and extend the CLI with MCP servers
- Apply best practices for productive AI-assisted development

## 🎓 Prerequisites

To get the most out of this guide, you should have:

- Basic familiarity with command-line interfaces
- An active GitHub Copilot subscription
- Basic programming knowledge (any language)
- **Git installed on your system** (required for Git features)
- **GitHub CLI (gh) installed** (required for GitHub integration)

## 📖 How to Use This Guide

- **New to GitHub Copilot CLI?** Start with [Getting Started](01-getting-started.md) and work through each section sequentially.
- **Looking for specific features?** Jump to the relevant section using the links above.
- **Want hands-on practice?** Each section includes runnable examples you can try immediately.
- **Need quick reference?** Check the [⚡ Cheat Sheet](00-cheat-sheet.md) or [Slash Commands](04-slash-commands.md) reference.
- **Setting up for a team?** Go straight to [Team Setup](21-team-setup.md).

## 🆘 Getting Help

- **In the CLI:** Type `/help` to see available commands
- **Documentation:** Visit [GitHub Docs](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)
- **Feedback:** Use `/feedback` command to submit feedback
- **Issues:** Report bugs on [GitHub](https://github.com/github/copilot-cli)

## 🎬 Let's Begin!

Ready to start? Head over to [Getting Started](01-getting-started.md) to install and configure GitHub Copilot CLI.

## 🔧 Configuration & Customization

Learn how to customize and extend Copilot CLI:

### Configuration Files
- **[.copilot Directory](15-copilot-directory.md)** - Understanding `~/.copilot/` structure, config.json, and data storage
- **config.json** - Main configuration (model, theme, trusted folders, etc.)
- **mcp-config.json** - MCP server configuration

### Instruction Files
- **[Skills System](14-skills-system.md)** - Reusable expertise modules (Python expert, React patterns, etc.)
- **[AGENTS.md](13-agents-file.md)** - Project-specific AI agent instructions
- **.github/copilot-instructions.md** - Copilot CLI workflows  
- **CLAUDE.md / GEMINI.md** - Model-specific preferences

### Comparison

| Feature | Skills | AGENTS.md | config.json | Instructions |
|---------|--------|-----------|-------------|--------------|
| Scope | Domain expertise | Project config | CLI behavior | Coding style |
| Location | ~/.copilot/skills/ | Project root | ~/.copilot/ | .github/ |
| Reusable | ✅ Across projects | ❌ Project only | ✅ Global | ❌ Project only |
| Activatable | ✅ On/off | ❌ Always on | N/A | ❌ Always on |
| Examples | "Python expert" | Architecture | theme, model | Formatting |

---

**Note:** This guide covers GitHub Copilot CLI v1.0.11. Some capabilities may vary by version — run `/update` to stay current.
