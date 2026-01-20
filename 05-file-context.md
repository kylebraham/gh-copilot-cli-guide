# File and Context Management

Learn how to effectively manage files, directories, and context in GitHub Copilot CLI. Proper context management is crucial for getting the best results from the AI.

## Understanding Context

**Context** is everything the AI "knows" about your current situation:

- Conversation history
- Files you've mentioned
- Working directory structure
- Git repository information
- System information

### Context Window Size

The AI has a **limited context window** (typically ~200,000 tokens):

```
> /context

Context Window Usage:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Used: 45,231 / 200,000 tokens (22.6%)
```

**Token estimates:**
- 1 token ≈ 4 characters
- 100 tokens ≈ 75 words
- 1,000 tokens ≈ 750 words
- Average code file ≈ 2,000-5,000 tokens

## File References with @

The `@` symbol is your primary tool for including files in context.

### Basic File Mention

```
> @src/app.js review this file for bugs
```

**What happens:**
1. CLI reads the file
2. Adds contents to context
3. AI analyzes the code
4. Provides review

### Multiple Files

```
> Compare @src/app.js with @src/app.test.js

> Review @package.json and @README.md and @.env.example
```

The AI can work with multiple files simultaneously.

### Glob Patterns

Use wildcards to reference multiple files:

```
# All JavaScript files in src/
> @src/*.js

# All test files recursively
> @**/*.test.js

# All Python files in specific directory
> @backend/**/*.py

# Multiple patterns
> @src/**/*.{js,ts} review all source files
```

**Common glob patterns:**
- `*` - Any characters (single directory)
- `**` - Any characters (recursive)
- `?` - Single character
- `{a,b}` - Either a or b
- `[abc]` - Any character in brackets

### Relative Paths

Paths are relative to working directory:

```
# If CWD is ~/projects/my-app
> @src/utils.js          # ~/projects/my-app/src/utils.js
> @../other-app/file.js  # ~/projects/other-app/file.js
```

### Absolute Paths

You can use absolute paths (if directory is allowed):

```
> @/home/user/projects/my-app/src/app.js

> Add logging to @/opt/my-lib/core.js
```

But must first allow the directory:

```
> /add-dir /opt/my-lib
> @/opt/my-lib/core.js now this works
```

## Explicit File Inclusion

Sometimes you want to include files without doing anything yet:

```
> I want to work with @src/app.js, @src/utils.js, and @src/config.js

[Files are now in context]

> Add error handling to all three files

[AI has context of all files and can modify them]
```

## Context Management Strategies

### Strategy 1: Minimal Context

Include only what's needed for the current task:

```
> @src/auth.js add token refresh logic
```

**Pros:**
- Faster responses
- Lower token usage
- More focused answers

**Cons:**
- May miss dependencies
- Less holistic understanding

### Strategy 2: Comprehensive Context

Include related files for full understanding:

```
> I need to modify the authentication system
> @src/auth.js
> @src/middleware/auth.js
> @src/models/User.js
> @src/utils/jwt.js
> Now add token refresh logic
```

**Pros:**
- Better understanding
- More coherent changes
- Considers dependencies

**Cons:**
- Uses more tokens
- Slower responses

### Strategy 3: Progressive Context

Start minimal, add files as needed:

```
> @src/auth.js add token refresh logic

[AI suggests changes]

> You'll also need to update the User model
> @src/models/User.js

[AI now updates both files]
```

**Pros:**
- Efficient token usage
- Adaptive approach
- Interactive refinement

### Strategy 4: Pattern-Based

Use globs for similar files:

```
> @src/**/*.test.js review all tests for best practices

> @components/**/*.tsx make all components TypeScript strict
```

**Pros:**
- Quick inclusion of related files
- Consistent operations across files
- Less typing

**Cons:**
- May include too many files
- Can hit context limits
- Less precise control

## Monitoring Context Usage

### Check Current Usage

```
> /context
```

Output shows:
```
Context Window Usage:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Used: 65,432 / 200,000 tokens (32.7%)

Included Files:
  • src/app.js (4,532 tokens)
  • src/utils.js (2,876 tokens)
  • src/config.js (1,234 tokens)
  • tests/app.test.js (5,643 tokens)
  • package.json (456 tokens)

Conversation: 18 messages (12,691 tokens)

Recent Large Additions:
  • Message #15 added 8,432 tokens
  • @src/app.js added 4,532 tokens
```

### Context Warning Signs

Watch for these indicators:

```
⚠️  Context window 75% full
    Consider using /compact or /clear

❌ Context window full
   Please /compact or /clear before continuing
```

## Reducing Context Usage

### Method 1: Compact Conversation

```
> /compact
```

Compresses conversation history while preserving key information.

**What's preserved:**
- Important decisions
- Key code snippets
- Recent context
- Open tasks

**What's compressed:**
- Old messages
- Redundant information
- Resolved issues

### Method 2: Clear Conversation

```
> /clear
```

Starts completely fresh.

**When to use:**
- Switching to unrelated task
- Context is getting messy
- Want clean slate

**What's cleared:**
- All conversation history
- Message context

**What's kept:**
- Session files
- Working directory
- Checkpoints

### Method 3: Selective Removal

```
> I don't need @src/old-file.js anymore

> Forget about the test files we discussed
```

Ask AI to ignore specific files.

### Method 4: Smart Summarization

```
> Summarize what we've accomplished so far, then I want to start fresh

[AI summarizes]

> /clear

> [Paste summary] Continue from here...
```

## Working Directory Management

### Setting Working Directory

The working directory defines your workspace:

```
# Start CLI in desired directory
cd ~/projects/my-app
copilot

# Or change within CLI
> /cwd ~/projects/my-app
```

### Why Working Directory Matters

1. **File Access** - AI can only access files in/below CWD
2. **Relative Paths** - All relative paths start from CWD
3. **Git Context** - Git repository is detected from CWD
4. **Default Scope** - File operations default to CWD

### Viewing Directory Structure

```
> Show me the directory structure

> List all files in src/

> What files are in the current directory?
```

AI can read and understand your file tree.

### Multi-Project Workflows

Working with multiple projects:

```
# Add both project directories
> /add-dir ~/projects/frontend
> /add-dir ~/projects/backend

# Reference files from both
> @~/projects/frontend/src/App.js
> @~/projects/backend/server.js

# Or switch between them
> /cwd ~/projects/frontend
[Work on frontend]

> /cwd ~/projects/backend
[Work on backend]
```

## File System Operations

### Reading Files

```
> Show me @src/app.js

> What's in @package.json?

> Read @README.md
```

AI reads files and includes in context automatically.

### Creating Files

```
> Create a new file src/utils.js with helper functions

> Make a .env.example file with template variables

> Create @tests/new-test.js
```

### Editing Files

```
> @src/app.js add error handling

> Update @package.json to include new dependency

> Refactor @src/utils.js to use async/await
```

### Deleting Files

```
> Delete @old-file.js

> Remove all @temp-*.js files

> Clean up test artifacts
```

AI asks for confirmation before deleting.

### Moving/Renaming Files

```
> Rename @old-name.js to @new-name.js

> Move @src/utils.js to @src/helpers/utils.js

> Reorganize the project structure
```

## Advanced File Patterns

### Excluding Files

```
> Review all @src/**/*.js files except tests

> Analyze @**/*.py but ignore @**/migrations/*.py
```

Use natural language to express exclusions.

### Conditional Inclusion

```
> Include @src/**/*.js only if they import React

> Show me @**/*.ts files that have TODO comments
```

### Nested Patterns

```
> @src/**/components/**/*.tsx

> @{frontend,backend}/**/test/**/*.js
```

## File Content Search

### Finding Code

```
> Find all files that import express

> Where is the login function defined?

> Show me all files with TODO comments
```

### Code Analysis

```
> @**/*.js find all console.log statements

> @src/**/*.py check for security issues

> @**/*.tsx find unused imports
```

## Context Best Practices

### DO:

✅ **Start specific** - Mention only needed files initially  
✅ **Add context progressively** - Include more files as needed  
✅ **Monitor context usage** - Check `/context` regularly  
✅ **Compact when high** - Use `/compact` at 70%+ usage  
✅ **Use globs wisely** - Pattern match related files  

### DON'T:

❌ **Include everything upfront** - Wastes context  
❌ **Ignore context warnings** - Will cause failures  
❌ **Reference huge files** - May hit limits  
❌ **Forget to clear** - When switching topics  
❌ **Use globs blindly** - May include too much  

## Practical Examples

### Example 1: Focused Refactoring

```
> /clear
> I want to refactor the authentication module
> @src/auth/login.js
> @src/auth/register.js
> @src/auth/middleware.js

> Refactor these files to use async/await instead of callbacks

[AI refactors all three files]

> /context
Context: 15,432 / 200,000 tokens (7.7%)
```

**Efficient** - Only included what's needed.

### Example 2: Comprehensive Review

```
> I need a security audit of the API

> @src/routes/**/*.js
> @src/middleware/**/*.js
> @src/models/**/*.js
> @src/utils/auth.js

> Review all these files for security issues

[AI performs comprehensive security analysis]

> /context
Context: 85,234 / 200,000 tokens (42.6%)
```

**Thorough** - Included everything for complete analysis.

### Example 3: Iterative Development

```
> @src/api/users.js add input validation

[AI adds validation]

> Also need to validate in @src/api/posts.js

[AI adds validation to second file]

> And @src/api/comments.js

[AI adds validation to third file]

> /context
Context: 22,456 / 200,000 tokens (11.2%)
```

**Efficient iteration** - Added files as needed.

### Example 4: Cross-Project Work

```
> /add-dir ~/projects/shared-lib
> /cwd ~/projects/my-app

> @src/utils.js uses functions from @~/projects/shared-lib/helpers.js

> Update to use the latest API from shared-lib

[AI understands both projects and updates accordingly]
```

**Multi-project** - Working across repositories.

## File Size Considerations

### Small Files (<2KB)

```
> @config.json
> @.env.example
```

**No issues** - Include freely.

### Medium Files (2KB-50KB)

```
> @src/app.js
> @src/components/Dashboard.tsx
```

**Monitor** - Check context after inclusion.

### Large Files (>50KB)

```
> @dist/bundle.js
> @docs/api-full.md
```

**Be careful** - May consume significant context.

**Solutions:**
```
> Show me just the exports from @large-file.js

> Summarize @docs/long-document.md

> Extract the User class from @large-file.js
```

### Very Large Files (>500KB)

**Avoid direct inclusion:**
```
❌ @data/large-dataset.json
❌ @dist/vendor.bundle.js
```

**Instead:**
```
✅ Describe the structure of data/large-dataset.json
✅ Parse the first 100 lines of large-file.csv
✅ Show me the schema of data/database-dump.sql
```

## Directory Structure Examples

### Typical Web App

```
my-app/
├── src/
│   ├── components/     # @src/components/**/*.tsx
│   ├── pages/          # @src/pages/**/*.tsx
│   ├── utils/          # @src/utils/**/*.ts
│   ├── api/            # @src/api/**/*.ts
│   └── App.tsx         # @src/App.tsx
├── tests/              # @tests/**/*.test.ts
├── public/             # @public/**/*
└── package.json        # @package.json
```

### Python Project

```
my-project/
├── src/
│   ├── models/         # @src/models/**/*.py
│   ├── views/          # @src/views/**/*.py
│   ├── utils/          # @src/utils/**/*.py
│   └── __init__.py
├── tests/              # @tests/**/*.py
├── requirements.txt    # @requirements.txt
└── README.md           # @README.md
```

## Quick Reference

```bash
# File reference patterns
@file.js                 # Single file
@dir/*.js               # All JS in dir
@**/test/*.js           # All tests recursively
@**/*.{ts,tsx}          # Multiple extensions

# Context management
/context                # Check usage
/compact                # Compress history
/clear                  # Fresh start
/cwd /path              # Change directory
/add-dir /path          # Allow directory
/list-dirs              # Show allowed dirs

# File operations
Create @file.js         # New file
Update @file.js         # Edit file
Delete @file.js         # Remove file
Show @file.js           # Read file
```

---

**Next:** [Code Editing and Development](06-code-editing.md)  
**Previous:** [Slash Commands](04-slash-commands.md)
