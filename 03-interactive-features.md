# Interactive Features

GitHub Copilot CLI's interactive interface is designed for productive conversations with the AI. This guide covers all interactive features, keyboard shortcuts, and interface elements.

## Interface Overview

When you launch Copilot CLI, you enter an interactive terminal session:

```
┌─────────────────────────────────────────────────────────────────┐
│  GitHub Copilot CLI v0.0.387                                    │
│  Model: claude-sonnet-4.5                                       │
│  Working Directory: ~/projects/my-app                           │
│  Session: d8b676aa-6cd4-42aa-81c6-603f7f9c7300                 │
└─────────────────────────────────────────────────────────────────┘

Timeline (Ctrl+O to expand):
  ▸ You: Create a hello world function
  ▸ Assistant: Created hello.js

> _
```

### Interface Components

1. **Header Bar** - Shows version, model, and working directory
2. **Timeline** - Conversation history (collapsible)
3. **Input Prompt** - Where you type
4. **Response Area** - AI responses and outputs

## Keyboard Shortcuts

Mastering keyboard shortcuts makes you significantly more efficient:

### Essential Shortcuts

| Shortcut | Action | When to Use |
|----------|--------|-------------|
| `Ctrl+C` | Cancel / Clear / Exit | Stop AI thinking, clear input, or exit CLI |
| `Ctrl+D` | Shutdown | Quickly exit the CLI |
| `Ctrl+L` | Clear screen | Clean up visual clutter |
| `Esc` | Cancel operation | Stop current AI operation |

### Navigation Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `↑` | Previous command | Navigate command history backwards |
| `↓` | Next command | Navigate command history forwards |
| `Ctrl+A` | Start of line | Move cursor to beginning |
| `Ctrl+E` | End of line | Move cursor to end (if supported) |
| `Meta+←` | Previous word | Jump cursor left by word (macOS/Linux) |
| `Meta+→` | Next word | Jump cursor right by word (macOS/Linux) |

### Editing Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Ctrl+H` | Delete character | Delete previous character (like Backspace) |
| `Ctrl+W` | Delete word | Delete previous word |
| `Ctrl+U` | Delete to start | Delete from cursor to beginning of line |
| `Ctrl+K` | Delete to end | Delete from cursor to end of line |

### Timeline Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Ctrl+O` | Toggle timeline | Expand recent/collapse timeline |
| `Ctrl+E` | Toggle all timeline | Expand all/collapse timeline |

### Plan Mode Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Shift+Tab` | Toggle mode | Switch between interactive and plan modes |
| `Ctrl+Y` | Open plan | Open plan.md in your editor |

## Command History

The CLI maintains a history of your commands across sessions.

### Using History

```
# Press ↑ to see previous commands
> ↑  # Shows: Create a hello world function

# Press ↑ again to go further back
> ↑  # Shows: Add error handling to server.js

# Press ↓ to move forward in history
> ↓
```

### Searching History

Type part of a command and use ↑ to find matches:

```
> create  # Type prefix
> ↑       # Finds: Create a hello world function
```

### Repeating Commands

Quickly re-run previous commands:

```
> ↑       # Get previous command
> Enter   # Execute it again
```

## Timeline Management

The timeline shows your conversation history and can be expanded or collapsed.

### Viewing Timeline

```
# Collapsed (default)
Timeline:
  ▸ You: Create a hello world function
  ▸ Assistant: Created hello.js

# Expanded (Ctrl+O)
Timeline:
  You: Create a hello world function
  
  Assistant: I'll create a simple hello world function.
  
  [Tool: create]
  File: hello.js
  Content:
  function helloWorld() {
    console.log("Hello, World!");
  }
  
  ✅ File created successfully
```

### Timeline Commands

```
Ctrl+O          # Expand recent messages
Ctrl+O again    # Collapse timeline
Ctrl+E          # Expand entire history
Ctrl+E again    # Collapse timeline
```

### Why Use Timeline?

- **Review context** - See what was discussed
- **Understand actions** - Review what changes were made
- **Debug issues** - Track down where things went wrong
- **Learn from examples** - See how the AI solved problems

## Input Features

### Multi-line Input

The CLI supports multi-line input for complex prompts:

```
> Create a function that:
... - Takes a user ID
... - Fetches user data from database
... - Returns formatted user object
```

To enable enhanced multi-line support:

```
> /terminal-setup
```

This configures your terminal for:
- `Shift+Enter` - Add new line without submitting
- `Ctrl+Enter` - Submit multi-line input

### File Mentions with @

Reference files directly in your prompts:

```
> @src/app.js add error handling

> Review @package.json and @README.md

> Compare @old/version.js with @new/version.js

# Use glob patterns
> Test all @**/*.test.js files

> Find bugs in @src/**/*.py
```

**Auto-completion:**
Type `@` and start typing a filename - the CLI will suggest matches.

### Direct Shell Execution with !

Execute shell commands without AI processing:

```
> !ls -la

> !git status

> !npm run build

> !cat myfile.txt
```

The `!` prefix bypasses the AI and runs commands immediately in your shell.

## Interactive Prompts

### Confirmations

The CLI asks for confirmation before executing actions:

```
> Delete old-file.txt

AI: I'll delete old-file.txt
> Proceed? (y/n): _
```

Response options:
- `y` or `yes` - Proceed with action
- `n` or `no` - Cancel action
- `Ctrl+C` - Cancel operation

### Progress Indicators

Watch progress during long operations:

```
> Run all tests

🤔 Analyzing test files...
🔧 Running test suite...
⏳ Test 1/50 completed...
⏳ Test 25/50 completed...
⏳ Test 50/50 completed...
✅ All tests passed!
```

### Error Handling

If something goes wrong:

```
> Create file.txt

❌ Error: File already exists

AI: Would you like me to:
  1. Overwrite the existing file
  2. Create with a different name
  3. Cancel the operation
  
> Enter choice (1-3): _
```

## Context Management

### Viewing Context

Check what's currently in the AI's context:

```
> /context
```

Output shows:
```
Context Window Usage:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Used: 15,432 tokens / 200,000 tokens (7.7%)

Included Files:
  • src/app.js (1,234 tokens)
  • src/utils.js (856 tokens)
  • package.json (234 tokens)

Conversation Length: 12 messages
```

### Managing Context Size

When context fills up:

```
# Compact the conversation to reduce tokens
> /compact

# Clear conversation but keep context
> /clear

# Remove specific files from context
> I don't need @old-file.js anymore
```

## Session Management

### Viewing Session Info

```
> /session
```

Shows:
- Current session ID
- Working directory
- Files in workspace
- Session duration
- Token usage

### Session Subcommands

```
> /session checkpoints        # View checkpoints
> /session checkpoints 5      # View last 5 checkpoints
> /session files              # List files in session
> /session plan               # Show implementation plan
> /session rename "My Project"  # Rename session
```

### Switching Sessions

Resume a different session:

```
> /resume                     # Show list of sessions
> /resume <session-id>        # Resume specific session
```

Sessions persist across launches, so you can continue where you left off.

## Theming

Customize the visual appearance:

### View Current Theme

```
> /theme show
```

### List Available Themes

```
> /theme list
```

Options:
- `auto` - Follow system preference
- `dark` - Dark theme
- `light` - Light theme

### Set Theme

```
> /theme set dark
> /theme set light
> /theme set auto
```

## Sharing Sessions

Export your conversation for sharing or documentation:

### Share to File

```
> /share file output.md
```

Creates a markdown file with full conversation history.

### Share to GitHub Gist

```
> /share gist
```

Creates a public GitHub Gist and provides the URL.

### Specify Custom Path

```
> /share file ~/Documents/my-session.md
```

## Working Directory Management

### View Current Directory

```
> /cwd
```

### Change Directory

```
> /cwd ~/projects/other-app

# Or use cd alias
> /cd ~/projects/other-app
```

**Note:** Changing directories affects:
- Which files the AI can access
- Git repository context
- Relative paths

## User Management

If you have multiple GitHub accounts:

### View Current User

```
> /user show
```

### List All Users

```
> /user list
```

### Switch Users

```
> /user switch
```

Select from list of authenticated users.

## Usage Statistics

Track your resource usage:

```
> /usage
```

Shows:
- Requests made this session
- Total requests this month
- Premium requests remaining
- Token usage statistics

## Interactive Examples

### Example 1: File Creation Flow

```
> Create a new React component called UserProfile

🤔 Analyzing request...

AI: I'll create a UserProfile component with props for name and email.

Preview:
─────────────────────────────────────────
File: src/components/UserProfile.jsx

import React from 'react';

export default function UserProfile({ name, email }) {
  return (
    <div className="user-profile">
      <h2>{name}</h2>
      <p>{email}</p>
    </div>
  );
}
─────────────────────────────────────────

> Create this file? (y/n): y

✅ Created src/components/UserProfile.jsx
```

### Example 2: Multi-step Interaction

```
> Set up a new Express server with middleware

🤔 Planning steps...

AI: I'll create:
1. server.js with Express setup
2. Add middleware configuration
3. Create routes folder
4. Install dependencies

> Proceed? (y/n): y

Step 1/4: Creating server.js...
✅ Done

Step 2/4: Configuring middleware...
✅ Done

Step 3/4: Creating routes folder...
✅ Done

Step 4/4: Installing dependencies...
🔧 Running: npm install express cors helmet
✅ All steps completed!
```

### Example 3: Error Recovery

```
> Run build command

🔧 Executing: npm run build

❌ Error: Command failed with exit code 1

Build output:
  Error: Cannot find module 'webpack'

AI: It looks like webpack is missing. Would you like me to:
  1. Install webpack as a dependency
  2. Check package.json for issues
  3. Show build configuration

> 1

🔧 Installing webpack...
✅ Installed successfully

> Run build command again

🔧 Executing: npm run build
✅ Build succeeded!
```

## Tips for Effective Interaction

### DO:
✅ Use keyboard shortcuts to navigate efficiently  
✅ Review timeline to understand context  
✅ Use `@` to reference files explicitly  
✅ Check `/context` when conversations get long  
✅ Use `/clear` to start fresh when switching topics  

### DON'T:
❌ Spam Ctrl+C unnecessarily - it may exit the CLI  
❌ Ignore confirmations - always review what will happen  
❌ Let context window fill up without compacting  
❌ Forget to check `/usage` to monitor quota  

## Advanced Interaction Patterns

### Iterative Refinement

```
> Create a login form

[AI creates basic form]

> Add validation

[AI adds validation]

> Make it responsive

[AI adds responsive CSS]

> Add loading state

[AI adds loading indicators]
```

### Comparative Analysis

```
> Compare @old/implementation.js with @new/implementation.js

[AI shows differences and explains changes]

> Which approach is better for performance?

[AI analyzes and provides recommendation]
```

### Exploratory Learning

```
> Explain how @src/auth.js works

[AI explains authentication flow]

> Show me where sessions are stored

[AI points to relevant code]

> How would I add OAuth support?

[AI provides implementation guidance]
```

## Troubleshooting Interactive Issues

### CLI Not Responding

```
Press Ctrl+C to cancel current operation
Press Ctrl+L to clear screen
If frozen, close terminal and restart
```

### Input Not Working

```
Check if terminal supports ANSI codes
Run /terminal-setup for enhanced support
Try a different terminal emulator
```

### History Not Working

```
History file: ~/.copilot/history
Check file permissions
Verify write access to home directory
```

## Quick Reference Card

```
Essential Shortcuts:
  Ctrl+C    Cancel/Clear/Exit
  Ctrl+D    Shutdown
  Ctrl+L    Clear screen
  Ctrl+O    Toggle timeline
  ↑↓        Command history

File References:
  @file.js          Single file
  @**/*.js          Glob pattern
  
Commands:
  /help             Show help
  /cwd              Change directory
  /context          Check context
  /clear            New conversation
  /usage            Check quota
  !command          Direct shell execution
```

---

**Next:** [Slash Commands](04-slash-commands.md)  
**Previous:** [Basic Concepts](02-basic-concepts.md)
