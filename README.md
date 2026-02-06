# GitHub Copilot CLI Guide

Welcome to the comprehensive guide for GitHub Copilot CLI! This guide will help you understand and master the GitHub Copilot CLI, bringing AI-powered coding assistance directly to your terminal.

## 📚 Guide Structure

This guide is organized into the following sections:

1. **[Getting Started](01-getting-started.md)** - Installation, authentication, and first steps
2. **[Basic Concepts](02-basic-concepts.md)** - Understanding how the CLI works
3. **[Interactive Features](03-interactive-features.md)** - Working with the interactive interface
4. **[Slash Commands](04-slash-commands.md)** - Complete reference of all commands
5. **[File and Context Management](05-file-context.md)** - Managing files and context
6. **[Code Editing and Development](06-code-editing.md)** - Writing and editing code
7. **[GitHub Integration](07-github-integration.md)** - Working with repos, PRs, and issues
8. **[Advanced Features](08-advanced-features.md)** - MCP servers, custom agents, and more
9. **[Plan Mode](09-plan-mode.md)** - Creating implementation plans
10. **[Best Practices](10-best-practices.md)** - Tips and tricks for effective use
11. **[Troubleshooting](11-troubleshooting.md)** - Common issues and solutions
12. **[Examples and Tutorials](12-examples.md)** - Real-world scenarios and walkthroughs
13. **[AGENTS.md File Guide](13-agents-file.md)** - Configuring AI agent behavior for your project
14. **[Skills System Guide](14-skills-system.md)** - Using and creating reusable skill modules
15. **[.copilot Directory Guide](15-copilot-directory.md)** - Understanding config, sessions, and data storage
16. **[New Features in v0.405](16-v0.405-new-features.md)** - Latest features: `/init` and enhanced `/delegate`

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

### 🔄 Upgrading to v0.405

Already have Copilot CLI? Upgrade to get the latest features:

```bash
# Homebrew
brew upgrade copilot-cli

# npm
npm update -g @github/copilot

# Verify new version
copilot --version  # Should show 0.0.405+
```

**New in v0.405:**
- ✨ `/init` - Initialize complete projects with one command
- 🚀 `/delegate` - Enhanced PR creation with test validation
- [See all new features →](16-v0.405-new-features.md)

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
- **Need quick reference?** Check the [Slash Commands](04-slash-commands.md) reference.

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

**Note:** This guide is based on GitHub Copilot CLI version 0.0.405. Features may vary in different versions.
