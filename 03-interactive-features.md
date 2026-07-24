# Interactive Features

GitHub Copilot CLI's interactive interface is designed for productive conversations with the AI. This guide covers all interactive features, keyboard shortcuts, and interface elements.

## Interface Overview

When you launch Copilot CLI, you enter an interactive terminal session:

```
┌─────────────────────────────────────────────────────────────────┐
│  GitHub Copilot CLI v1.0.11                                     │
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
| `Ctrl+C` | Cancel / Clear / Copy | Stop AI, clear input, or copy selection; press twice to exit CLI |
| `Ctrl+C ×2` | Exit CLI | Quit from the CLI |
| `Ctrl+D` | Shutdown | Quickly exit the CLI |
| `Ctrl+L` | Clear screen | Clean up visual clutter |
| `Ctrl+T` | Toggle reasoning display | Show/hide model reasoning output *(new)* |
| `Esc` | Cancel input / close picker | Clear the current input line or close a picker; press **twice** (`Esc Esc`) to cancel an in-flight AI operation |

### Navigation Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `↑` | Previous command | Navigate command history backwards |
| `↓` | Next command | Navigate command history forwards |
| `Ctrl+A` | Start of line | Move cursor to beginning (when typing) |
| `Ctrl+E` | End of line | Move cursor to end (when typing); see Timeline Shortcuts when input is empty |
| `Meta+←` | Previous word | Jump cursor left by word (macOS/Linux) |
| `Meta+→` | Next word | Jump cursor right by word (macOS/Linux) |

### Editing Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Ctrl+H` | Delete character | Delete previous character (like Backspace) |
| `Ctrl+W` | Delete word | Delete previous word |
| `Alt+D` | Delete next word | Delete the word in front of the cursor |
| `Ctrl+U` | Delete to start | Delete from cursor to beginning of line |
| `Ctrl+K` | Delete to end | Delete from cursor to end of line (joins lines at end of line) |
| `Ctrl+G` | Open external editor | Edit the current prompt in your `$EDITOR` *(new)* |

### Timeline Shortcuts

> **Note:** `Ctrl+O` and `Ctrl+E` behave differently depending on whether the input prompt is empty.

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Ctrl+O` | Expand all timeline | Expands all timeline entries (when no input) |
| `Ctrl+E` | Expand all timeline | Expands all timeline entries (when no input) |
| `Ctrl+X` → `O` | Open link | Open a link from the most recent timeline event *(new)* |
| `Ctrl+X` → `B` | Background task | Move the current running task or shell command to the background (v1.0.39+) |

### Mode Shortcuts

| Shortcut | Action | Description |
|----------|--------|-------------|
| `Shift+Tab` | Cycle modes | Cycle through interactive → plan (enable `/experimental` to also access autopilot) |

> **Ctrl+T** toggles the display of model reasoning tokens inline in the response — useful for understanding how the AI arrived at a decision.
> **Ctrl+S** stashes the current prompt so you can type something else, then pops it back when you press `Ctrl+S` again (v1.0.60, Claude Code parity). The slash-command picker is still available by typing `/`.

### Completion Acceptance

| Shortcut | Action |
|----------|--------|
| `Tab` | Accept the highlighted option in completion popups (`@`-mentions, paths, slash commands) |
| `Ctrl+Y` | Also accepts the highlighted completion option (v1.0.35+) |

The subcommand picker highlights the selected item with a **❯** indicator (v1.0.36+), making it easy to confirm which option you are about to accept.

## Interaction Modes

`Shift+Tab` cycles through the available interaction modes:

| Mode | Description |
|------|-------------|
| **Interactive** (default) | Conversational mode — the AI responds to each message as you send it |
| **Plan** | The AI drafts a `plan.md` before executing; you review and approve the plan |
| **Autopilot** | The AI acts autonomously with minimal confirmation prompts *(requires `/experimental` first, then `Shift+Tab`)* |

The current mode is shown in the input prompt indicator.

### Enabling Autopilot Mode

Autopilot mode is gated behind the experimental features flag. Enable it first, then use `Shift+Tab` to cycle to it:

```
> /experimental
```

This unlocks the autopilot step in the `Shift+Tab` cycle. Disable at any time with:

```
> /experimental off
```

> **Use autopilot with care** — it will execute changes with fewer confirmation prompts.

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
Ctrl+O          # Expand all timeline entries (when input is empty)
Ctrl+O again    # Collapse timeline
Ctrl+E          # Expand all timeline entries (when input is empty)
Ctrl+E again    # Collapse timeline
Ctrl+X → O     # Open a link from the most recent timeline event
```

> When the input prompt is **not** empty, `Ctrl+E` moves the cursor to the end of line and `Ctrl+O` runs the current command while preserving input.

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

> **v1.0.72+:** Typing `$` at the prompt opens a full interactive shell in the current session directory, instead of running a single command. It's opt-in — enable it with `/settings shellShortcut on` (off by default).
>
> ```
> > /settings shellShortcut on
> $
> ```
>
> **v1.0.74+:** The `$` shortcut now opens a shell even while the agent is still working on a response, instead of requiring you to wait for it to finish.

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
> /resume my-feature-work     # Resume by name (v1.0.35+)
```

The session picker (v1.0.35+) shows each session's **branch name** and **idle/in-use status**, making it easy to identify the right session at a glance. The picker also supports improved text search with cursor navigation. In v1.0.39+, the picker has an improved **tab layout**, enhanced **status display**, and **progressive loading** so it opens immediately and populates as sessions are fetched.

Press `s` while the session picker is open to cycle through sort orders: **relevance** (default), **last used**, **created**, or **name** (v1.0.37+).

Delete sessions you no longer need:

```
> /session delete <id>        # Delete a specific session
> /session delete-all         # Delete all sessions
```

In the picker, press `x` on any entry to delete it immediately.

Sessions persist across launches, so you can continue where you left off.

> **v1.0.72+:** The Sessions sidebar is now navigable with the keyboard as well as the mouse — arrow keys open, focus, and move the sidebar selection, and `Enter` (or a click) switches to a session. Press `n` to spawn a new session or `x` twice to close one from the keyboard. `/settings` can disable the sidebar entirely or stop it from restoring remembered sessions.

## Advanced Context & Session Strategies

Knowing *when* to use `/compact`, `/clear`, or `/resume` — and how to structure long-running project sessions — will make you significantly more effective.

### When to /compact vs /clear vs /resume

| Situation | Command | Effect |
|-----------|---------|--------|
| Conversation getting long, want to continue the same task | `/compact` | Summarises history, frees tokens, keeps working |
| Switching to a completely different topic | `/clear` | Wipes history, fresh start in same session |
| Picking up work from yesterday | `/resume` | Restores full session with history and context |
| Context is confusing the AI | `/clear` then re-add only relevant files | Clean slate with targeted context |

### Decision Tree

```
Is the AI giving confused or contradictory responses?
  YES → /clear and start fresh with a focused prompt
  NO  → Is the context window above 80% full? (/context to check)
    YES → /compact to summarise and free space
    NO  → Keep going

Do you need to continue work from a previous session?
  YES → /resume [session-id]  or  copilot --continue
  NO  → Start a new session
```

### The Checkpoint System

Copilot CLI automatically creates checkpoints at key moments. View them with:

```
> /session checkpoints       # List all checkpoints
> /session checkpoints 5     # Last 5 checkpoints
```

Checkpoints capture the state of the conversation and workspace, allowing you to see what the session looked like at a specific point. Useful for understanding what changed during a long autopilot run.

### Long-Running Project Patterns

For projects that span multiple days or weeks:

**Pattern 1: Daily resume**

```bash
# Start each day by resuming your last session
copilot --continue

# Or pick a specific session interactively
> /resume
```

**Pattern 2: Session-per-feature**

```bash
# Name your session for easy retrieval
> /session rename "auth-refactor-sprint-5"

# Tomorrow, resume it by name
> /resume auth-refactor-sprint-5
```

**Pattern 3: Compact at the start of each day**

```
# Resume, then compact to summarise the old conversation
> /resume
> /compact
# Now you have a fresh context window with the key history preserved
```

### Context Window Tips

```
> /context          # See current usage and which files are loaded

# Target context: keep below 50% for best results
# Above 80%: /compact immediately
# Above 95%: /clear and reload only what you need

# Check what files are loaded
> /session files
```

### Recovering from a Bloated Context

If the AI starts repeating itself or losing track of what you're building:

```
> /context          # Check usage — if near limit:
> /compact          # Try compact first
# If still confused:
> /clear            # Nuclear option — fresh start
> Here's what we're building: [brief summary]
> The key files are @src/app.js @src/auth.js
> Continue from where we left off...
```

---

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
  Ctrl+C      Cancel/Clear/Copy selection (×2 to exit)
  Ctrl+D      Shutdown
  Ctrl+L      Clear screen
  Ctrl+T      Toggle model reasoning display
  Esc         Cancel current operation
  ↑↓          Command history

Timeline (when input is empty):
  Ctrl+O      Expand all timeline entries
  Ctrl+E      Expand all timeline entries
  Ctrl+X → O  Open link from most recent timeline event

Editing (when typing):
  Ctrl+A      Move to start of line
  Ctrl+E      Move to end of line
  Ctrl+G      Edit prompt in external editor
  Ctrl+W      Delete previous word
  Ctrl+U      Delete to start of line
  Ctrl+K      Delete to end of line

Modes (Shift+Tab to cycle):
  Interactive → Plan → Autopilot (/experimental required)
  Ctrl+Y      Open plan.md or most recent research report in editor

File References:
  @file.js          Single file
  @**/*.js          Glob pattern
  
Commands:
  /help             Show help
  /cwd              Change directory
  /context          Check context
  /clear            New conversation
  /usage            Check quota
  /experimental     Enable experimental features (incl. autopilot)
  !command          Direct shell execution
```

---

**Next:** [Slash Commands](04-slash-commands.md)  
**Previous:** [Basic Concepts](02-basic-concepts.md)
