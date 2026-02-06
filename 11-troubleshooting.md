# Troubleshooting

Common issues and solutions when using GitHub Copilot CLI. This guide helps you diagnose and fix problems quickly.

## Installation Issues

### Command Not Found

**Problem:**
```bash
$ copilot
command not found: copilot
```

**Solutions:**

1. **Check PATH:**
```bash
# Verify installation
which copilot

# Check npm global bin
npm config get prefix

# Add to PATH if needed (add to ~/.bashrc or ~/.zshrc)
export PATH="$PATH:$(npm config get prefix)/bin"
```

2. **Reinstall:**
```bash
# Using npm
npm uninstall -g @github/copilot
npm install -g @github/copilot

# Using Homebrew
brew uninstall copilot-cli
brew install copilot-cli
```

3. **Verify installation:**
```bash
# Check version
npm list -g @github/copilot

# Check executable location
ls -la $(which copilot)
```

### Permission Errors

**Problem:**
```bash
$ npm install -g @github/copilot
Error: EACCES: permission denied
```

**Solutions:**

1. **Use npx (no installation):**
```bash
npx @github/copilot
```

2. **Fix npm permissions:**
```bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
# Add to shell profile

npm install -g @github/copilot
```

3. **Use sudo (not recommended):**
```bash
sudo npm install -g @github/copilot
```

### Version Conflicts

**Problem:**
```bash
$ copilot --version
0.0.395  # Old version

# But you just upgraded!
```

**Solutions:**

1. **Clear npm cache:**
```bash
npm cache clean --force
npm uninstall -g @github/copilot
npm install -g @github/copilot@latest
```

2. **Check for multiple installations:**
```bash
# Find all copilot installations
which -a copilot

# Remove duplicates, keep only one
```

3. **Force Homebrew update:**
```bash
brew update
brew upgrade --force copilot-cli
```

4. **Restart terminal:**
```bash
# Close and reopen terminal
# Or reload shell config
source ~/.zshrc  # or ~/.bashrc
```

### Upgrade Issues

**Problem:**
Upgrade to v0.405 fails or new features not working.

**Solutions:**

1. **Complete upgrade verification:**
```bash
# Check version
copilot --version
# Must show 0.0.405 or higher

# Test new features
copilot
> /init --help
> /delegate --help
```

2. **Clean reinstall:**
```bash
# Remove completely
brew uninstall copilot-cli
# or
npm uninstall -g @github/copilot

# Clear caches
rm -rf ~/.copilot/cache

# Reinstall latest
brew install copilot-cli
# or
npm install -g @github/copilot@latest
```

3. **GitHub CLI conflicts:**
```bash
# Update GitHub CLI first
brew upgrade gh
# or
winget upgrade --id GitHub.cli

# Then upgrade Copilot CLI
```

**For detailed upgrade instructions, see [New Features Guide - Upgrading to v0.405](16-v0.405-new-features.md#upgrading-to-v0405).**

## Authentication Issues

### Login Fails

**Problem:**
```
> /login
Error: Authentication failed
```

**Solutions:**

1. **Check subscription:**
   - Visit https://github.com/settings/copilot
   - Verify active subscription
   - Check if CLI is enabled for your organization

2. **Clear existing auth:**
```bash
# Remove old tokens
rm -rf ~/.copilot/auth

# Try login again
copilot
> /login
```

3. **Use Personal Access Token:**
```bash
# Create PAT at: https://github.com/settings/personal-access-tokens/new
# Add "Copilot Requests" permission
export GH_TOKEN="your_token_here"
copilot
```

4. **Check organization settings:**
   - Ask admin to enable Copilot CLI
   - Verify you're in the right organization

### Token Expired

**Problem:**
```
Error: Token expired or invalid
```

**Solution:**
```
> /logout
> /login

# Or regenerate PAT if using tokens
```

### Wrong User

**Problem:**
Logged in with wrong GitHub account.

**Solution:**
```
> /user show
Current user: wrong-user

> /logout
> /login
[Authenticate with correct account]

# Or switch if multiple users
> /user list
> /user switch
```

## CLI Operation Issues

### CLI Freezes or Hangs

**Problem:**
CLI stops responding.

**Solutions:**

1. **Cancel operation:**
```
Press Ctrl+C
```

2. **Check for long-running operation:**
```
Press Ctrl+O to expand timeline
See what's running
```

3. **Kill and restart:**
```bash
# In another terminal
ps aux | grep copilot
kill <PID>

# Restart
copilot
> /resume  # Continue where you left off
```

### Slow Responses

**Problem:**
AI takes too long to respond.

**Solutions:**

1. **Switch to faster model:**
```
> /model claude-haiku-4.5
```

2. **Reduce context:**
```
> /context
[Check if context is full]

> /compact
or
> /clear
```

3. **Check network:**
```bash
# Test connection
ping github.com
curl -I https://api.github.com
```

4. **Check system resources:**
```bash
# Check CPU/Memory
top
htop

# Free up resources
```

### Error: Context Window Full

**Problem:**
```
Error: Context window exceeded
Cannot add more files
```

**Solutions:**

1. **Compact conversation:**
```
> /compact
```

2. **Clear and restart:**
```
> /clear
[Include only essential files]
```

3. **Use smaller files:**
```
# Instead of entire file
> Show me just the User class from @large-file.js

# Or summarize
> Summarize @large-file.js
```

### Cannot Access Files

**Problem:**
```
Error: Permission denied accessing /path/to/file
```

**Solutions:**

1. **Check working directory:**
```
> /cwd
[Verify you're in the right place]

> /cwd /correct/path
```

2. **Add directory:**
```
> /add-dir /path/to/directory
```

3. **Check file permissions:**
```bash
ls -la /path/to/file
chmod +r /path/to/file
```

## API and Rate Limiting

### Rate Limit Exceeded

**Problem:**
```
Error: API rate limit exceeded
Please try again later
```

**Solutions:**

1. **Check usage:**
```
> /usage
```

2. **Wait for reset:**
```
Usage resets monthly
Check reset date in /usage output
```

3. **Optimize requests:**
   - Use `/compact` to reduce token usage
   - Batch similar requests
   - Use faster models for simple tasks

### Network Errors

**Problem:**
```
Error: Network request failed
Could not connect to API
```

**Solutions:**

1. **Check connection:**
```bash
ping github.com
curl https://api.github.com
```

2. **Check proxy settings:**
```bash
# If behind proxy
export HTTPS_PROXY=http://proxy:8080
export NO_PROXY=localhost,127.0.0.1

copilot
```

3. **Check firewall:**
```bash
# Ensure these are allowed:
# api.github.com
# *.openai.com
# *.anthropic.com
```

4. **Try different network:**
```bash
# Switch to different WiFi or use mobile hotspot
```

## Git and GitHub Issues

### GitHub CLI Not Installed

**Problem:**
```
Error: GitHub features require GitHub CLI (gh)
gh: command not found
```

**Solution:**
```bash
# Install GitHub CLI
# macOS
brew install gh

# Windows
winget install --id GitHub.cli

# Linux (Debian/Ubuntu)
sudo apt install gh

# Verify
gh --version

# Authenticate
gh auth login
```

### GitHub CLI Not Authenticated

**Problem:**
```
Error: gh is not authenticated
GitHub features unavailable
```

**Solution:**
```bash
# Check status
gh auth status

# Login
gh auth login
# Choose: GitHub.com
# Choose: HTTPS or SSH
# Authenticate via browser

# Verify
gh auth status
```

### Not a Git Repository

**Problem:**
```
Warning: Not in a Git repository
Some features may be limited
```

**Solution:**
```bash
# Initialize Git repo
git init
git remote add origin <url>

# Or navigate to existing repo
cd /path/to/git/repo
copilot
```

### Cannot Access Repository

**Problem:**
```
Error: Cannot access repository
Permission denied
```

**Solutions:**

1. **Check authentication:**
```
> /user show
> /login
```

2. **Verify repository access:**
```bash
# Test with gh CLI
gh repo view owner/repo

# Or git
git fetch
```

3. **Check remote URL:**
```bash
git remote -v
# Ensure it points to correct repo
```

### /delegate Fails

**Problem:**
```
> /delegate Add feature
Error: Could not create pull request
```

**Solutions:**

1. **Verify gh CLI:**
```bash
# Check gh is installed
gh --version

# Check gh is authenticated
gh auth status

# If not, authenticate
gh auth login

# Test gh PR creation
gh pr list
```

2. **Check permissions:**
   - Need write access to repository
   - Check if protected branch rules allow

3. **Verify remote:**
```bash
git remote -v
git pull origin main
```

4. **Check branch:**
```bash
# Ensure not on detached HEAD
git branch
git checkout main
```

5. **Manual PR creation:**
```bash
# Create branch manually
git checkout -b feature-branch

# Make changes with CLI
> Create the feature

# Push and create PR via gh
git push origin feature-branch
gh pr create
```

## File Operation Issues

### File Not Found

**Problem:**
```
> @src/app.js
Error: File not found
```

**Solutions:**

1. **Check path:**
```
> /cwd
[Verify current directory]

> List files in src/
[Verify file exists]
```

2. **Use correct path:**
```bash
# Check actual file location
ls -la src/
find . -name "app.js"
```

3. **Check for typos:**
```
# Case-sensitive!
@src/App.js   ≠   @src/app.js
```

### Cannot Create File

**Problem:**
```
Error: Cannot create file
Permission denied or file exists
```

**Solutions:**

1. **Check permissions:**
```bash
ls -la $(dirname path/to/file)
chmod +w directory/
```

2. **File already exists:**
```
> Update @existing-file.js instead
# Or
> Delete @existing-file.js first
> Then create @existing-file.js
```

3. **Path doesn't exist:**
```bash
# Create directory first
mkdir -p path/to/directory
# Then create file
```

## Model and AI Issues

### Model Unavailable

**Problem:**
```
Error: Selected model is not available
```

**Solution:**
```
> /model
[Select different model]

> /model claude-sonnet-4.5
[Use default model]
```

### Poor Quality Responses

**Problem:**
AI generates incorrect or low-quality code.

**Solutions:**

1. **Be more specific:**
```
❌ > Fix the code

✅ > @src/api.js fix the null pointer error on line 42
    when user object is undefined
```

2. **Provide more context:**
```
> @related-file1.js
> @related-file2.js
> @file-to-fix.js
> Now fix the authentication issue
```

3. **Use better model:**
```
> /model claude-opus-4.5
[Retry request]
```

4. **Break down request:**
```
> First, explain how @src/auth.js works
[Review explanation]
> Now fix the token validation bug
```

### AI Misunderstands Request

**Problem:**
AI does something different than intended.

**Solutions:**

1. **Clarify request:**
```
> That's not what I meant.
  I want to [clear description]
```

2. **Provide example:**
```
> Create a function that takes an array and returns unique values
  Example: [1, 2, 2, 3] → [1, 2, 3]
```

3. **Undo and retry:**
```
> Undo that change
> Let me rephrase: [better description]
```

## Session and State Issues

### Lost Session

**Problem:**
Can't find previous session.

**Solution:**
```
> /resume
[Shows list of recent sessions]

# Check session files
ls -la ~/.copilot/sessions/
```

### Session Corrupted

**Problem:**
```
Error: Cannot load session
Session data corrupted
```

**Solutions:**

1. **Start fresh:**
```
> /clear
```

2. **Resume different session:**
```
> /resume
[Select different session]
```

3. **Delete corrupted session:**
```bash
rm -rf ~/.copilot/sessions/<session-id>
```

### History Not Working

**Problem:**
Cannot navigate command history with ↑↓.

**Solutions:**

1. **Check history file:**
```bash
ls -la ~/.copilot/history
# Ensure it exists and is writable
```

2. **Fix permissions:**
```bash
chmod 644 ~/.copilot/history
```

3. **Recreate history:**
```bash
rm ~/.copilot/history
# History will be recreated on next use
```

## Terminal Issues

### Display Problems

**Problem:**
Garbled text, broken formatting, weird characters.

**Solutions:**

1. **Clear screen:**
```
Press Ctrl+L
```

2. **Check terminal:**
```bash
echo $TERM
# Should be xterm-256color or similar

export TERM=xterm-256color
```

3. **Try different terminal:**
   - iTerm2 (macOS)
   - Windows Terminal (Windows)
   - GNOME Terminal (Linux)

4. **Reset terminal:**
```bash
reset
# or
tput reset
```

### Keyboard Shortcuts Don't Work

**Problem:**
Ctrl+O, Ctrl+Y, etc. don't work.

**Solutions:**

1. **Check terminal settings:**
   - Ensure terminal passes key combinations
   - Check for conflicting shortcuts

2. **Use alternative terminal:**
   - Some terminals don't support all shortcuts
   - Try recommended terminals

3. **Run terminal setup:**
```
> /terminal-setup
```

### Multiline Input Not Working

**Problem:**
Cannot create multiline prompts.

**Solution:**
```
> /terminal-setup
[Follow instructions to configure terminal]
```

## Performance Issues

### High Memory Usage

**Problem:**
Copilot CLI using too much memory.

**Solutions:**

1. **Clear context:**
```
> /clear
```

2. **Compact session:**
```
> /compact
```

3. **Restart CLI:**
```
> /exit
copilot
> /resume
```

### High CPU Usage

**Problem:**
Copilot CLI consuming CPU.

**Solutions:**

1. **Check for stuck operations:**
```
Press Ctrl+C to cancel
```

2. **Check for background processes:**
```bash
ps aux | grep copilot
# Kill if necessary
```

3. **Restart CLI:**
```bash
killall copilot
copilot
```

## Diagnostic Commands

### System Information

```bash
# Check CLI version
copilot --version

# Check gh CLI version
gh --version

# Check gh authentication
gh auth status

# Check Git version
git --version

# Check Node version (for npm install)
node --version

# Check system
uname -a

# Check disk space
df -h ~/.copilot
```

### Enable Debug Mode

```bash
# Set debug level
export COPILOT_LOG_LEVEL=debug

# Set log file
export COPILOT_LOG_FILE=/tmp/copilot-debug.log

# Run CLI
copilot

# Check logs
tail -f /tmp/copilot-debug.log
```

### Check Configuration

```bash
# View config
cat ~/.copilot/config.json

# Check MCP servers
cat ~/.copilot/mcp-config.json

# List sessions
ls -la ~/.copilot/sessions/

# Check auth
ls -la ~/.copilot/auth/
```

## Getting Help

### In-CLI Help

```
> /help                    # Show all commands
> What can you do?         # Capabilities
> How do I [task]?         # Task-specific help
```

### Documentation

- [Official Docs](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)
- [This Guide](README.md)
- [GitHub Discussions](https://github.com/github/copilot-cli/discussions)

### Reporting Issues

```
> /feedback                # Submit feedback

# Or create GitHub issue:
# - Include error message
# - Include steps to reproduce
# - Include system information
# - Include CLI version
```

### Community Support

- GitHub Discussions
- GitHub Issues
- Stack Overflow (tag: github-copilot-cli)

## Quick Diagnostic Checklist

When something goes wrong:

- [ ] Check `copilot --version`
- [ ] Check `gh --version` and `gh auth status`
- [ ] Check `git --version` and `git status`
- [ ] Verify authentication (`/user show`)
- [ ] Check quota (`/usage`)
- [ ] Verify working directory (`/cwd`)
- [ ] Check context (`/context`)
- [ ] Look at error message
- [ ] Try `/clear` and retry
- [ ] Check network connection
- [ ] Restart CLI
- [ ] Check system resources
- [ ] Review recent changes
- [ ] Enable debug logging

## Common Error Messages

### "Authentication required"
→ Run `/login`

### "Rate limit exceeded"
→ Check `/usage`, wait for reset

### "Context window exceeded"
→ Run `/compact` or `/clear`

### "File not found"
→ Check path with `/cwd`

### "Permission denied"
→ Check file/directory permissions

### "Model unavailable"
→ Run `/model` to select different one

### "Network error"
→ Check internet connection

### "Session corrupted"
→ Run `/clear` or `/resume` different session

---

**Next:** [Examples and Tutorials](12-examples.md)  
**Previous:** [Best Practices](10-best-practices.md)
