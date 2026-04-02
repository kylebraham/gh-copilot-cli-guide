---
name: update-cli
description: Expertise in checking, upgrading, and managing GitHub Copilot CLI versions across all platforms and package managers.
---

# Update CLI Skill

## Metadata
- **ID**: update-cli
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: DevOps
- **Tags**: update, upgrade, install, package-manager

## Description

Expertise in checking, upgrading, and managing GitHub Copilot CLI versions across all platforms and package managers. Handles version detection, upgrade execution, rollback, post-upgrade verification, and migration guidance.

## Capabilities

- Detect current installed version
- Run upgrade commands for any package manager (Homebrew, npm, winget, Chocolatey, Scoop, curl installer)
- Use in-CLI `/update` command
- Check for available updates without installing
- Roll back to a previous version
- Verify a successful upgrade
- Switch between stable and prerelease channels
- Handle permission errors during upgrade

## When to Use

- User asks "how do I update Copilot CLI?"
- User reports unexpected behavior that might be a known bug fixed in a newer version
- User wants to check what version they are on
- User needs to downgrade to a specific version
- User hit a permission error while upgrading
- User wants to try prerelease features

## Instructions

### Checking the Current Version

Always start by confirming the installed version:

```bash
copilot --version
```

For npm-managed installations, also check:

```bash
npm list -g @github/copilot
```

### Upgrade Commands by Package Manager

Identify which package manager was used to install the CLI, then run the appropriate command.

**Homebrew (macOS / Linux — stable):**
```bash
brew upgrade copilot-cli
```

**Homebrew (macOS / Linux — prerelease):**
```bash
brew upgrade copilot-cli@prerelease
```

**npm (all platforms — latest stable):**
```bash
npm update -g @github/copilot
# or explicitly:
npm install -g @github/copilot@latest
```

**winget (Windows):**
```bash
winget upgrade GitHub.Copilot
```

**Chocolatey (Windows):**
```bash
choco upgrade copilot-cli
```

**Scoop (Windows):**
```bash
scoop update copilot-cli
```

**curl/bash installer (Linux):**
```bash
curl -fsSL https://gh.io/copilot-install | bash
```

**In-CLI update command (any platform):**
```
> /update
```

### Checking for Available Updates Without Installing

**Homebrew:**
```bash
brew outdated copilot-cli
```

**npm:**
```bash
npm outdated -g @github/copilot
```

**winget:**
```bash
winget list GitHub.Copilot
```

Compare the `Available` column to the installed version.

### Rollback / Downgrade to a Specific Version

**npm:**
```bash
npm install -g @github/copilot@<version>
# Example:
npm install -g @github/copilot@0.0.400
```

**Homebrew:**
```bash
brew install copilot-cli@<version>
```

List available npm versions:
```bash
npm view @github/copilot versions --json
```

### Switching Between Stable and Prerelease Channels

**Homebrew:**
```bash
# Switch to prerelease
brew install copilot-cli@prerelease
brew link --overwrite copilot-cli@prerelease

# Switch back to stable
brew install copilot-cli
brew link --overwrite copilot-cli
```

**npm:**
```bash
# Prerelease (if published with next/beta tag)
npm install -g @github/copilot@next
# Back to stable
npm install -g @github/copilot@latest
```

### Handling Permission Errors During Upgrade

**npm permission errors (EACCES):**

Option 1 — use a version manager (recommended):
```bash
# Install nvm, then reinstall Node/npm under it — no sudo needed
nvm install --lts
npm install -g @github/copilot@latest
```

Option 2 — set a custom npm global prefix:
```bash
mkdir ~/.npm-global
npm config set prefix ~/.npm-global
# Add ~/.npm-global/bin to your PATH in ~/.bashrc or ~/.zshrc
npm install -g @github/copilot@latest
```

Option 3 — sudo (not recommended, but sometimes necessary):
```bash
sudo npm install -g @github/copilot@latest
```

**Homebrew permission errors:**
```bash
sudo chown -R $(whoami) $(brew --prefix)/*
brew upgrade copilot-cli
```

### Post-Upgrade Verification

After upgrading, run through this checklist:

1. **Confirm new version:**
   ```bash
   copilot --version
   ```

2. **Check authentication status:**
   ```bash
   copilot auth status
   ```

3. **Test with a simple prompt** — start the CLI and type a basic question to confirm it responds.

4. **Review breaking changes** — check `16-new-features.md` in this guide for any new features or behavior changes introduced in the new version.

5. **Re-check skills and MCP config** — occasionally a major upgrade changes the skills directory path or MCP config format. Run `/skills list` and confirm MCP tools still appear.

### Common Pitfalls

- ❌ Running `npm update -g` without specifying the package name can silently skip the CLI if npm decides the installed version satisfies constraints
- ❌ Using `sudo npm install -g` when npm was installed under a user-space version manager (breaks permissions permanently)
- ❌ Upgrading mid-session — always upgrade from a fresh terminal, not from inside a running Copilot CLI session
- ❌ Forgetting to re-authenticate after a major version upgrade that resets stored tokens

### Best Practices

- ✅ Pin to a specific version in CI/CD scripts (`npm install -g @github/copilot@<version>`) to avoid unexpected breaking changes
- ✅ Read the release notes before upgrading in production workflows
- ✅ Keep Homebrew up to date (`brew update`) before upgrading any formula
- ✅ Test the new version with a non-critical project first
- ✅ Use `/update` inside the CLI for the simplest in-place upgrade experience

## Checklists

### Pre-Upgrade
- [ ] Note current version (`copilot --version`)
- [ ] Check release notes for breaking changes
- [ ] Ensure stable internet connection
- [ ] Close any running Copilot CLI sessions

### Upgrade
- [ ] Run the correct command for your package manager
- [ ] Watch for errors in the output
- [ ] If permission error occurs, follow the permission fix steps above

### Post-Upgrade Verification
- [ ] `copilot --version` shows the new version
- [ ] `copilot auth status` shows authenticated
- [ ] A simple prompt responds correctly
- [ ] `/skills list` shows expected skills
- [ ] MCP tools appear (if configured)
- [ ] Review `16-new-features.md` for new behavior

## Resources

- [GitHub Copilot CLI Releases](https://github.com/github/copilot-cli/releases)
- [npm package page](https://www.npmjs.com/package/@github/copilot)
- [Homebrew formula](https://formulae.brew.sh/cask/copilot-cli)
- [New Features Guide](../16-new-features.md)
- [Getting Started Guide](../01-getting-started.md)

## Related Skills

- `doc-maintenance` — after upgrading, use this skill to update the documentation guide
- `cli-expertise` — deep feature knowledge for exploring what changed in the new version
