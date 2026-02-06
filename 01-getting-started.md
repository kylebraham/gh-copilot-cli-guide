# Getting Started with GitHub Copilot CLI

This guide will walk you through installing, setting up, and running GitHub Copilot CLI for the first time.

## Prerequisites

Before you begin, ensure you have:

1. **An active GitHub Copilot subscription**
   - Individual, Business, or Enterprise plan
   - Check your plan at [GitHub Copilot Plans](https://github.com/features/copilot/plans)

2. **Supported operating system:**
   - Linux
   - macOS
   - Windows (with PowerShell v6+)

3. **Command-line access**
   - Terminal on macOS/Linux
   - PowerShell on Windows

4. **Git installed** (required for repository features)
   - Download from [git-scm.com](https://git-scm.com/)
   - Verify: `git --version`

5. **GitHub CLI (gh) installed** (required for GitHub integration)
   - Essential for PR creation, issue management, and GitHub features
   - Download from [cli.github.com](https://cli.github.com/)
   - Verify: `gh --version`

## Installing GitHub CLI (gh)

**Important:** Install GitHub CLI first, as Copilot CLI uses it for GitHub integration.

### macOS

```bash
# Using Homebrew
brew install gh

# Or download from: https://cli.github.com/
```

### Linux

```bash
# Debian/Ubuntu
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
sudo apt update
sudo apt install gh

# Fedora/CentOS/RHEL
sudo dnf install gh

# Arch Linux
sudo pacman -S github-cli

# Or download from: https://cli.github.com/
```

### Windows

```bash
# Using WinGet
winget install --id GitHub.cli

# Using Chocolatey
choco install gh

# Using Scoop
scoop install gh

# Or download from: https://cli.github.com/
```

### Verify Installation

```bash
gh --version
# Should output: gh version X.X.X
```

### Authenticate GitHub CLI

```bash
# Authenticate with GitHub
gh auth login

# Follow prompts:
# 1. Choose GitHub.com
# 2. Choose HTTPS or SSH
# 3. Authenticate via web browser
# 4. Verify success

# Check authentication
gh auth status
```

## Installing Copilot CLI

GitHub Copilot CLI can be installed in several ways. Choose the method that works best for you:

### Option 1: Homebrew (macOS and Linux)

```bash
# Install stable version
brew install copilot-cli

# Or install prerelease version for latest features
brew install copilot-cli@prerelease
```

### Option 2: npm (All Platforms)

```bash
# Install globally with npm
npm install -g @github/copilot

# Or install prerelease version
npm install -g @github/copilot@prerelease
```

### Option 3: WinGet (Windows)

```bash
# Install stable version
winget install GitHub.Copilot

# Or install prerelease version
winget install GitHub.Copilot.Prerelease
```

### Option 4: Install Script (macOS and Linux)

```bash
# Using curl
curl -fsSL https://gh.io/copilot-install | bash

# Or using wget
wget -qO- https://gh.io/copilot-install | bash

# Install with sudo to install to /usr/local/bin
curl -fsSL https://gh.io/copilot-install | sudo bash

# Install to custom directory
curl -fsSL https://gh.io/copilot-install | PREFIX="$HOME/custom" bash

# Install specific version
curl -fsSL https://gh.io/copilot-install | VERSION="v0.0.405" bash
```

## Upgrading Copilot CLI

If you already have Copilot CLI installed and want to upgrade to v0.405:

### Upgrade with Homebrew

```bash
brew update
brew upgrade copilot-cli
copilot --version  # Should show 0.0.405 or higher
```

### Upgrade with npm

```bash
npm update -g @github/copilot
# Or for clean update
npm uninstall -g @github/copilot
npm install -g @github/copilot@latest
copilot --version
```

### Upgrade with WinGet (Windows)

```bash
winget upgrade GitHub.Copilot
copilot --version
```

### Verify Upgrade

```bash
# Check new version
copilot --version

# Test new features
copilot
> /init --help
> /delegate --help
```

**For detailed upgrade instructions, troubleshooting, and rollback options, see [New Features in v0.405](16-v0.405-new-features.md#upgrading-to-v0405).**

## Verifying Installation

After installation, verify that Copilot CLI is available:

```bash
# Check if copilot command is available
which copilot

# Check version
copilot --version
```

## First Launch

Now let's launch Copilot CLI for the first time:

```bash
copilot
```

### What to Expect

On your first launch, you'll see:

1. **Animated banner** - A welcoming animated splash screen (you can see it again with `copilot --banner`)
2. **Authentication prompt** - If not logged in, you'll be asked to authenticate

## Authentication

### Method 1: Interactive Login (Recommended)

If prompted, type the `/login` command:

```
> /login
```

Follow the on-screen instructions:
1. A browser window will open
2. You'll be asked to authorize the CLI
3. Return to your terminal once authorized

### Method 2: Personal Access Token (PAT)

For automated environments or CI/CD:

1. **Create a fine-grained PAT:**
   - Visit https://github.com/settings/personal-access-tokens/new
   - Under "Permissions," add "Copilot Requests"
   - Generate your token

2. **Set environment variable:**

```bash
# In your shell profile (~/.bashrc, ~/.zshrc, etc.)
export GH_TOKEN="your_token_here"

# Or use GITHUB_TOKEN
export GITHUB_TOKEN="your_token_here"
```

3. **Launch Copilot CLI:**

```bash
copilot
```

## Your First Interaction

Once authenticated, try these simple prompts:

### Example 1: Ask About Capabilities

```
> What can you do?
```

The CLI will explain its capabilities, including:
- Code editing and generation
- GitHub integration
- File operations
- Planning and task execution

### Example 2: Create a Simple File

```
> Create a hello.py file that prints "Hello, GitHub Copilot CLI!"
```

Watch as the CLI:
1. Understands your request
2. Generates the code
3. Shows you a preview
4. Executes the file creation

### Example 3: Ask for Help

```
> /help
```

This displays all available slash commands and keyboard shortcuts.

## Understanding the Interface

When you launch Copilot CLI, you'll see:

```
┌─────────────────────────────────────────────────────────────────┐
│  GitHub Copilot CLI (Interactive Mode)                          │
│  Model: claude-sonnet-4.5                                       │
│  Working Directory: /path/to/your/directory                     │
└─────────────────────────────────────────────────────────────────┘

>
```

The interface includes:
- **Header:** Shows current model and working directory
- **Timeline:** Displays conversation history (toggle with Ctrl+O)
- **Input prompt:** Where you type your requests
- **Response area:** Shows AI responses and actions

## Essential Keyboard Shortcuts

Learn these shortcuts for efficient navigation:

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel operation / Clear input / Exit |
| `Ctrl+D` | Shutdown |
| `Ctrl+L` | Clear screen |
| `Esc` | Cancel current operation |
| `↑↓` | Navigate command history |
| `Ctrl+O` | Expand/collapse timeline |
| `!` | Execute shell command directly |

## Setting Your Working Directory

Start Copilot CLI in the directory you want to work with:

```bash
# Navigate to your project
cd ~/projects/my-app

# Launch Copilot CLI
copilot
```

Or change directory within the CLI:

```
> /cwd ~/projects/my-app
```

## Choosing Your AI Model

By default, Copilot CLI uses Claude Sonnet 4.5. To switch models:

```
> /model
```

Available models include:
- Claude Sonnet 4.5 (default)
- Claude Sonnet 4
- Claude Opus 4.5
- GPT-5.2
- And more

## Quick Start Workflows (New in v0.405)

### Initialize a New Project

Use `/init` to scaffold a complete project:

```
> /init react-app

# Answer a few questions:
# - Project name?
# - TypeScript?
# - UI framework?
# - Testing library?

# CLI creates everything:
# ✓ Project structure
# ✓ Configuration files  
# ✓ Dependencies installed
# ✓ Starter code
# ✓ Ready to run!
```

Supported project types:
- `react-app`, `nextjs`, `vue`, `angular` - Frontend frameworks
- `express-api`, `fastify`, `nestjs` - Node.js APIs
- `python-flask`, `python-django`, `python-fastapi` - Python web
- `rust`, `go`, `java-spring` - Backend languages
- And many more!

### Create a Pull Request

Use `/delegate` to automatically create PRs:

```
> /delegate Fix the login button alignment issue

# CLI will:
# 1. Create a new branch
# 2. Make the changes
# 3. Run tests
# 4. Commit and push
# 5. Create a pull request
# 6. Link related issues
```

Perfect for:
- Bug fixes
- Feature implementations
- Documentation updates
- Refactoring tasks

## Quick Start Checklist

- [ ] Install Git and verify with `git --version`
- [ ] Install GitHub CLI with `brew install gh` (or platform equivalent)
- [ ] Authenticate GitHub CLI with `gh auth login`
- [ ] Install Copilot CLI using your preferred method
- [ ] Verify installation with `which copilot`
- [ ] Launch CLI with `copilot`
- [ ] Authenticate with `/login` or PAT
- [ ] Try asking "What can you do?"
- [ ] Create a simple test file
- [ ] Learn basic keyboard shortcuts
- [ ] Set your working directory

## Next Steps

Now that you have Copilot CLI up and running, let's dive deeper into how it works:

- Continue to [Basic Concepts](02-basic-concepts.md) to understand the fundamentals
- Jump to [Interactive Features](03-interactive-features.md) to learn the interface
- Check out [Examples](12-examples.md) for hands-on tutorials

## Troubleshooting

### Command Not Found

If `copilot` command is not found after installation:

```bash
# Check your PATH
echo $PATH

# If using npm, ensure npm global bin is in PATH
npm config get prefix
```

### GitHub CLI Not Found

If GitHub features don't work:

```bash
# Verify gh is installed
gh --version

# Verify gh is authenticated
gh auth status

# If not authenticated
gh auth login
```

### Authentication Issues

If login fails:
- Ensure you have an active Copilot subscription
- Check if your organization allows Copilot CLI
- Try using a Personal Access Token instead

### Permission Errors

If you encounter permission issues:
- Use `sudo` with the install script
- Check file permissions on the installation directory
- On Windows, run PowerShell as Administrator

For more troubleshooting help, see [Troubleshooting Guide](11-troubleshooting.md).

---

**Next:** [Basic Concepts](02-basic-concepts.md)
