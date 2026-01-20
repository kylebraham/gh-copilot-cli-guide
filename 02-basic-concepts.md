# Basic Concepts

Understanding the core concepts of GitHub Copilot CLI will help you use it more effectively. This section covers how the CLI works and the principles behind it.

## How GitHub Copilot CLI Works

GitHub Copilot CLI is an **agentic AI assistant** that operates in your terminal. Here's what that means:

### 1. Conversational Interface

You interact with Copilot CLI using natural language, just like chatting with a colleague:

```
> Create a Python function that calculates fibonacci numbers
```

Instead of memorizing complex commands, you describe what you want in plain English.

### 2. Context-Aware

The CLI understands:
- Your **current working directory** and file structure
- **Files you mention** using the `@` symbol
- Your **conversation history** from the current session
- **GitHub context** (if you're in a git repository)

### 3. Preview Before Execution

Copilot CLI shows you what it plans to do before doing it:

```
> Create a package.json file for a React project

AI Response:
I'll create a package.json with React dependencies.

[Preview of file contents shown]

> Execute this action? (y/n)
```

You maintain full control - nothing happens without your approval.

### 4. Agentic Behavior

The CLI can:
- **Plan complex tasks** across multiple steps
- **Execute commands** and interpret results
- **Read and modify files** in your project
- **Search and analyze code** to understand context
- **Iterate on solutions** based on feedback

## Core Components

### Working Directory

Your working directory is where the CLI operates:

```bash
# Start CLI in a specific directory
cd ~/projects/my-app
copilot

# Or change directory within CLI
> /cwd ~/projects/my-app

# Check current directory
> /cwd
```

**Important:** The CLI can only access files within your working directory and subdirectories (unless you explicitly allow other directories).

### Context Window

The context window is the AI's "memory" for your conversation:

- **Stores conversation history** - What you've discussed
- **Included files** - Files you've mentioned with `@`
- **System information** - Directory structure, Git status, etc.

Check your context usage:

```
> /context
```

This shows:
- Token usage (current/maximum)
- Active files in context
- Conversation length

### Models

Copilot CLI supports multiple AI models:

```
> /model
```

**Default:** Claude Sonnet 4.5

**Available models:**
- **Claude Sonnet 4.5** - Balanced performance and cost (default)
- **Claude Haiku 4.5** - Fast and efficient for simple tasks
- **Claude Opus 4.5** - Most capable for complex reasoning
- **GPT-5.2** - OpenAI's latest model
- And more...

Choose based on your task complexity and performance needs.

### Premium Requests

Each prompt you send consumes one premium request from your monthly quota:

```
> What's my usage?
> /usage
```

This shows:
- Requests used this session
- Total monthly quota
- Remaining requests

## Key Concepts

### 1. Natural Language Prompting

You can ask for what you want in plain English:

```
> Add error handling to the login function
> Refactor this code to be more readable
> Find all TODO comments in this project
> Explain how the authentication works
```

**Tips for effective prompts:**
- Be specific about what you want
- Mention file names when relevant
- Describe the desired outcome
- Include constraints or requirements

### 2. File Mentions with @

Use `@` to reference files and include them in context:

```
> @src/app.js add logging to all functions

> Compare @file1.js and @file2.js

> Refactor @components/Button.tsx to use TypeScript
```

**Glob patterns work too:**

```
> Review all @**/*.test.js files

> Find bugs in @src/**/*.py
```

### 3. Sessions and History

Each conversation is a **session** with:
- Persistent history
- Context accumulation
- Checkpoint system

Manage sessions:

```
> /session          # Show session info
> /resume           # Switch sessions
> /clear            # Start fresh conversation
```

### 4. Planning vs. Direct Execution

Copilot CLI can work in two modes:

**Direct Mode (default):**
```
> Create a README file
```
Immediate execution after your approval.

**Plan Mode:**
```
> /plan Create a full CRUD API with authentication
```
Creates a detailed plan first, then executes step-by-step.

Learn more in [Plan Mode](09-plan-mode.md).

## Understanding Responses

When you send a prompt, the CLI goes through several phases:

### Phase 1: Understanding
```
🤔 Analyzing your request...
```
The AI processes your prompt and determines what actions to take.

### Phase 2: Tool Selection
```
🔧 Using tools: edit, bash
```
Shows which tools it will use (file editing, command execution, etc.).

### Phase 3: Preview
```
📋 Preview of changes:
   - Edit src/app.js
   - Run npm test
```
Shows planned actions before execution.

### Phase 4: Confirmation
```
> Proceed with these changes? (y/n)
```
You approve or reject the plan.

### Phase 5: Execution
```
✅ Changes applied successfully
```
Actions are executed and results shown.

## File System Access

The CLI follows security principles:

### Allowed Locations

By default, CLI can access:
- Current working directory
- Subdirectories of working directory

### Adding Additional Directories

```
> /add-dir /path/to/other/directory

> /list-dirs       # See all allowed directories
```

### File Operations

The CLI can:
- **Read files** - View contents, search code
- **Write files** - Create new files
- **Edit files** - Modify existing files
- **Delete files** - Remove files (with confirmation)
- **Execute commands** - Run shell commands

All operations require your approval.

## Git Integration

When in a Git repository, the CLI understands:

- **Current branch** and commit history
- **Staged and unstaged changes**
- **Remote repository** information
- **Issues and Pull Requests** (via GitHub)

Example usage:

```
> What changed in the last commit?

> Create a new branch for this feature

> Show me the diff of staged files
```

## Slash Commands

Special commands start with `/`:

```
/help           # Show all commands
/model          # Switch AI model
/clear          # Clear conversation
/cwd            # Change directory
/context        # Show context usage
```

See [Slash Commands](04-slash-commands.md) for complete reference.

## Shell Commands

Execute shell commands directly:

```
> Run npm install

> Execute: git status

> !ls -la        # Direct execution (bypass AI)
```

The `!` prefix executes commands immediately without AI processing.

## Example: Putting It Together

Let's see these concepts in action:

```bash
# 1. Navigate to your project
cd ~/projects/my-app

# 2. Launch Copilot CLI
copilot

# 3. Set context by mentioning files
> @package.json show me the dependencies

# 4. Ask for changes
> Add prettier as a dev dependency

# 5. Review and approve changes
> y

# 6. Execute related command
> Run npm install

# 7. Check context usage
> /context

# 8. Clear for new task
> /clear
```

## Best Practices

### DO:
✅ Start CLI in your project directory  
✅ Mention specific files with `@` when relevant  
✅ Be clear and specific in your requests  
✅ Review previews carefully before approving  
✅ Use `/plan` for complex multi-step tasks  

### DON'T:
❌ Approve changes without reviewing them  
❌ Include sensitive data in prompts  
❌ Expect the AI to access files outside working directory  
❌ Use vague requests like "fix everything"  

## Conceptual Model

Think of Copilot CLI as a **pair programming partner** who:

1. **Listens** to your requests in natural language
2. **Analyzes** your codebase to understand context
3. **Plans** the necessary changes
4. **Shows** you what it will do
5. **Executes** with your approval
6. **Iterates** based on your feedback

Unlike traditional CLI tools that require exact syntax, Copilot CLI understands intent and can adapt to your workflow.

## Understanding Limitations

Be aware of:

- **Context limits** - Can only "remember" a certain amount (check with `/context`)
- **File size limits** - Very large files may need to be excluded
- **Quota limits** - Monthly premium request quota
- **No internet browsing** - Cannot fetch external data (except GitHub)
- **No persistent memory** - Each session is independent

## Next Steps

Now that you understand the basics:

- Learn the [Interactive Features](03-interactive-features.md)
- Master [Slash Commands](04-slash-commands.md)
- Practice with [Examples and Tutorials](12-examples.md)

## Quick Reference

```bash
# Essential concepts to remember
copilot                    # Launch in current directory
@filename.ext              # Reference files
/command                   # Slash commands
!command                   # Direct shell execution
/context                   # Check memory usage
/clear                     # Start fresh
Ctrl+C                     # Cancel/exit
```

---

**Next:** [Interactive Features](03-interactive-features.md)  
**Previous:** [Getting Started](01-getting-started.md)
