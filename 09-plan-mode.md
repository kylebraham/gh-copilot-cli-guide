# Plan Mode

Plan Mode helps you tackle complex tasks by creating detailed implementation plans before writing code. This approach ensures better architecture, fewer mistakes, and clearer execution.

## What is Plan Mode?

**Plan Mode** is a special workflow where:
1. You describe a complex task
2. AI creates a detailed plan
3. Plan is saved to `plan.md`
4. You review and edit the plan
5. AI executes the plan step-by-step

### When to Use Plan Mode

Use Plan Mode for:
- ✅ New features with multiple files
- ✅ Large refactoring efforts
- ✅ Architecture changes
- ✅ Complex bug fixes
- ✅ Multi-step migrations
- ✅ API implementations

Don't use Plan Mode for:
- ❌ Simple one-file edits
- ❌ Quick bug fixes
- ❌ Single-line changes
- ❌ Trivial refactoring

## Entering Plan Mode

### Method 1: /plan Command

```
> /plan Create a REST API for todo management with authentication
```

### Method 2: Shift+Tab Toggle

Press `Shift+Tab` to toggle between:
- **Interactive Mode** (default)
- **Plan Mode**

When in Plan Mode, the prompt shows:
```
[PLAN] >
```

### Method 3: [[PLAN]] Prefix

Messages prefixed with `[[PLAN]]` automatically trigger plan mode:
```
[[PLAN]] Build a user authentication system with JWT tokens
```

## Creating a Plan

### Simple Plan Request

```
> /plan Add user authentication to the API
```

AI will:
1. Analyze your request
2. Ask clarifying questions if needed
3. Examine existing code
4. Create structured plan
5. Save to `plan.md`

### Detailed Plan Request

```
> /plan Create a blog platform with:
  - User registration and login
  - Create, read, update, delete posts
  - Comments on posts
  - Tag system for posts
  - Search functionality
  - Admin dashboard
  Use Node.js, Express, MongoDB
```

## Plan Structure

A typical `plan.md` includes:

```markdown
# Implementation Plan: Blog Platform

## Problem Statement
Create a full-featured blog platform with user auth,
CRUD operations, comments, tags, and search.

## Proposed Approach
- Use Express.js for API
- MongoDB for data storage
- JWT for authentication
- RESTful endpoint design
- Separate routes for admin

## Workplan

- [ ] Set up project structure
  - [ ] Initialize npm project
  - [ ] Install dependencies
  - [ ] Configure TypeScript
  
- [ ] Database models
  - [ ] User model
  - [ ] Post model
  - [ ] Comment model
  - [ ] Tag model
  
- [ ] Authentication
  - [ ] JWT middleware
  - [ ] Register endpoint
  - [ ] Login endpoint
  - [ ] Password hashing
  
- [ ] Post endpoints
  - [ ] GET /api/posts
  - [ ] POST /api/posts
  - [ ] PUT /api/posts/:id
  - [ ] DELETE /api/posts/:id
  
- [ ] Comment endpoints
  - [ ] GET /api/posts/:id/comments
  - [ ] POST /api/posts/:id/comments
  - [ ] DELETE /api/comments/:id
  
- [ ] Search functionality
  - [ ] Search endpoint
  - [ ] Index posts for search
  
- [ ] Admin dashboard
  - [ ] Admin middleware
  - [ ] User management
  - [ ] Content moderation

## Notes and Considerations
- Use bcrypt for password hashing
- Implement rate limiting
- Add input validation with Joi
- Include error handling middleware
- Write tests for each endpoint
- Add API documentation with Swagger
```

## Reviewing and Editing Plans

### Viewing the Plan

```
# Within CLI
> /session plan

# Open in editor
Press Ctrl+Y
```

### Editing the Plan

1. Press `Ctrl+Y` to open plan.md in your editor
2. Make changes:
   - Add/remove tasks
   - Reorder steps
   - Add notes
   - Adjust approach
3. Save and close

> **v1.0.70+:** `Ctrl+Y` now opens the plan file (or the most recent research report) from **any mode** — interactive, autopilot, etc. — not just from Plan mode.
4. Return to CLI

### Plan Review Prompts

Before execution, AI may ask:

```
AI: I've created a plan with 12 main tasks. 
    Press Ctrl+Y to review the plan.
    
    Key decisions needed:
    - Should I use TypeScript or JavaScript?
    - What authentication library? (passport vs jsonwebtoken)
    - Database: MongoDB or PostgreSQL?
    
> Use TypeScript, jsonwebtoken, and MongoDB
```

## Plan Mode Is Read-Only for Built-in Tools (v1.0.71+)

While in Plan Mode, the agent now hard-blocks built-in tool calls that would modify the workspace — it can no longer edit files or run mutating shell commands while drafting a plan. Built-in mutating actions (like opening a pull request) are blocked too. MCP and external tools are still allowed, since Plan Mode can't guarantee what side effects they have.

**Why it matters:** You can review a plan with confidence that nothing in the workspace changed while it was being drafted — Plan Mode is now enforced as read-only for the built-in toolset, not just a convention.

> **v1.0.74+:** Plan Mode makes a narrow exception for planning artifacts written inside the session folder (e.g., `~/.copilot/session-state/<id>/plan.md`), so the agent can save its own planning notes and drafts without leaving Plan Mode. File mutations everywhere else in the workspace are still hard-blocked.

> **v1.0.76:** Resuming a session (`/resume`) now restores Plan Mode if that's the mode the session was in when you left, instead of reverting to interactive mode. See [Autopilot Mode](17-autopilot-mode.md) for the equivalent behavior with autopilot sessions.

## Plan Mode Model Override (v1.0.74+)

Use `/model plan` (or `/model --plan`) to set a model that's used only while you're in Plan Mode:

```
> /model plan claude-opus-4.8
> /model --plan gpt-5.6
```

- Pass a model id to set the override.
- Pass `off` to clear it.
- Pass no argument to open the model picker.

The override applies only during Plan Mode and automatically reverts to your regular session model once you leave Plan Mode (e.g., via `Shift+Tab`). See [Slash Commands — `/model`](04-slash-commands.md#model-model).

## Executing the Plan

### Starting Execution

After reviewing the plan:

```
> Start implementing the plan

> Begin execution

> Let's get started

> Implement the plan
```

**Note:** If still in Plan Mode, toggle out with `Shift+Tab` first.

### Step-by-Step Execution

AI executes each task sequentially:

```
✅ Task 1/12: Set up project structure
   Created package.json
   Installed dependencies
   Configured TypeScript

✅ Task 2/12: Create User model
   Created src/models/User.ts
   Added validation schema

⏳ Task 3/12: Implement JWT middleware
   Creating src/middleware/auth.ts...
```

### Progress Tracking

The plan.md is updated automatically:

```markdown
## Workplan

- [x] Set up project structure
  - [x] Initialize npm project
  - [x] Install dependencies
  - [x] Configure TypeScript
  
- [x] Database models
  - [x] User model
  - [ ] Post model  <- Currently working
  - [ ] Comment model
  - [ ] Tag model
```

## Iterating on Plans

### Adjusting During Execution

```
> Pause execution

> I want to change the approach for authentication
  Use Passport.js instead of manual JWT

> Update the plan accordingly

> Resume execution
```

### Adding Tasks

```
> Add a new task to the plan:
  Implement password reset functionality

> Continue with the updated plan
```

### Skipping Tasks

```
> Skip the admin dashboard for now

> Mark search functionality as done, I implemented it manually

> Continue with remaining tasks
```

## Plan Mode Best Practices

### Writing Good Plan Requests

✅ **Be specific about requirements:**
```
> /plan Create a REST API for task management with:
  - User auth (JWT)
  - CRUD for tasks
  - Task categories
  - Due dates and priorities
  - PostgreSQL database
```

✅ **Include constraints:**
```
> /plan Refactor authentication system
  - Must maintain backward compatibility
  - Keep existing database schema
  - Don't break current API endpoints
```

✅ **Mention related files:**
```
> /plan Add caching layer to @src/api/users.js
  Use Redis for caching
  Cache TTL: 5 minutes
```

❌ **Avoid vague requests:**
```
> /plan Make the app better
> /plan Fix everything
> /plan Add features
```

### Reviewing Plans

Before execution, check:

1. **Completeness** - All requirements covered?
2. **Order** - Logical sequence of tasks?
3. **Feasibility** - Tasks are achievable?
4. **Scope** - Not too large or small?
5. **Dependencies** - Prerequisites identified?

### Executing Plans

**DO:**
- ✅ Review plan before starting
- ✅ Execute in order
- ✅ Test after major steps
- ✅ Commit frequently
- ✅ Adjust plan as needed

**DON'T:**
- ❌ Execute without review
- ❌ Skip important steps
- ❌ Ignore test failures
- ❌ Make unplanned changes
- ❌ Rush through tasks

## Interactive vs Plan Mode

### Interactive Mode

```
> _                    # Normal prompt
```

**Best for:**
- Quick edits
- Single file changes
- Exploratory work
- Small bug fixes
- Questions and explanations

### Plan Mode

```
[PLAN] > _            # Plan mode prompt
```

**Best for:**
- Complex features
- Multi-file changes
- Architecture work
- Large refactoring
- Systematic implementation

### Switching Modes

Press `Shift+Tab` to toggle:
```
> _                   # Interactive
[Press Shift+Tab]
[PLAN] > _           # Plan Mode
[Press Shift+Tab]
> _                   # Back to Interactive
```

## Practical Examples

### Example 1: New Feature

```
> /plan Add user profile editing functionality

AI creates plan:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# Plan: User Profile Editing

## Tasks
- [ ] Create ProfileEdit component
- [ ] Add PUT /api/users/:id endpoint
- [ ] Add form validation
- [ ] Handle image upload
- [ ] Add update tests
- [ ] Update documentation

Would you like to review? (Press Ctrl+Y)

> Looks good, start implementing

✅ Task 1/6: Creating ProfileEdit component...
```

### Example 2: Refactoring

```
> /plan Refactor @src/api/**/*.js to use async/await

AI creates plan:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# Plan: Async/Await Refactoring

## Approach
1. Identify all callback-based code
2. Convert to promises
3. Refactor to async/await
4. Update error handling
5. Update tests

## Files to Update
- src/api/users.js (15 callbacks)
- src/api/posts.js (12 callbacks)
- src/api/auth.js (8 callbacks)
- src/api/comments.js (6 callbacks)

## Workplan
- [ ] Refactor users.js
- [ ] Refactor posts.js
- [ ] Refactor auth.js
- [ ] Refactor comments.js
- [ ] Update all tests
- [ ] Verify no regressions

> Start execution
```

### Example 3: Bug Fix Investigation

```
> /plan Investigate and fix the memory leak in the server

AI creates plan:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

# Plan: Memory Leak Investigation

## Problem
Server memory usage grows over time until crash.

## Investigation Steps
- [ ] Add memory profiling
- [ ] Identify leak source
- [ ] Review event listeners
- [ ] Check database connections
- [ ] Review caching logic
- [ ] Test with heap snapshot

## Fix Steps
- [ ] Implement fix
- [ ] Add memory monitoring
- [ ] Add tests
- [ ] Verify fix

Press Ctrl+Y to review

> Begin investigation
```

### Example 4: Full Feature Implementation

```
> /plan Build a complete notification system with:
  - In-app notifications
  - Email notifications
  - Push notifications
  - Notification preferences
  - Notification history
  - Mark as read/unread
  - Real-time updates via WebSocket

AI creates comprehensive plan with:
- 8 main sections
- 35 subtasks
- Database migrations
- API endpoints
- Frontend components
- Tests
- Documentation

> Press Ctrl+Y to review

[Edit plan to adjust priorities]

> Start with in-app notifications first
  Skip push notifications for now

> Begin execution
```

## Plan Templates

### API Development Template

```markdown
# API Development Plan

## Endpoints
- [ ] List resource: GET /api/resource
- [ ] Get single: GET /api/resource/:id
- [ ] Create: POST /api/resource
- [ ] Update: PUT /api/resource/:id
- [ ] Delete: DELETE /api/resource/:id

## Components
- [ ] Data model
- [ ] Validation schema
- [ ] Controller
- [ ] Routes
- [ ] Middleware
- [ ] Tests

## Documentation
- [ ] OpenAPI spec
- [ ] Usage examples
```

### Feature Addition Template

```markdown
# Feature Implementation Plan

## Prerequisites
- [ ] Review existing code
- [ ] Identify dependencies
- [ ] Design data structures

## Implementation
- [ ] Backend changes
- [ ] Frontend changes
- [ ] Database migrations
- [ ] API updates

## Quality Assurance
- [ ] Unit tests
- [ ] Integration tests
- [ ] Manual testing
- [ ] Documentation

## Deployment
- [ ] Update dependencies
- [ ] Migration scripts
- [ ] Deployment notes
```

### Refactoring Template

```markdown
# Refactoring Plan

## Current State
[Describe current implementation]

## Target State
[Describe desired implementation]

## Steps
- [ ] Identify affected files
- [ ] Create test baseline
- [ ] Refactor incrementally
- [ ] Verify tests pass
- [ ] Update documentation

## Risks
- [Identify potential issues]

## Rollback Plan
- [How to revert if needed]
```

## Critic Agent (Experimental, v1.0.18+)

The **Critic agent** is an experimental feature that automatically reviews plans and complex implementations using a complementary AI model. When active, the Critic performs a second-pass review before execution begins, catching logical errors, missed edge cases, and architectural issues.

### Enabling the Critic

The Critic requires experimental mode and is available for Claude models only:

```
> /experimental
```

Once experimental mode is on, the Critic activates automatically during plan review and complex multi-step implementations.

### What the Critic Reviews

- Logical gaps or contradictions in the plan
- Missing error handling or edge cases
- Incorrect step ordering or missing dependencies
- Architectural concerns before code is written

### Tips

- ✅ Use the Critic for high-stakes plans (migrations, API changes, security work)
- ✅ The Critic's feedback appears inline before execution — read it carefully
- ❌ Not available with non-Claude models (GPT, Gemini, etc.)
- ❌ May increase latency on complex plans — disable via `/experimental` if speed is critical

---

## Troubleshooting

### Plan Not Saving

```
# Check session folder
> /session

# Manually create plan
Press Ctrl+Y (may create file)

# Check permissions
ls -la ~/.copilot/sessions/
```

### Plan Too Large

```
# Break into phases
> /plan Phase 1: Core functionality
[Execute]

> /plan Phase 2: Advanced features
[Execute]
```

### Execution Stuck

```
# Check current task
> What are you working on?

# Skip problematic task
> Skip this task and continue

# Restart from specific task
> Start from task 5
```

## Critic Agent (Experimental)

Available in v1.0.18+, the **Critic agent** automatically reviews plans using a complementary model before execution begins. It checks for logical gaps, missed dependencies, and architectural issues — catching problems that the primary agent may have overlooked.

### Enabling the Critic Agent

The Critic agent requires **experimental mode** and a **Claude model**:

```
> /experimental
```

Once enabled, the Critic activates automatically when you generate or review a plan. You will see a secondary review pass in the session timeline before Copilot asks you to approve the plan.

### What the Critic Checks

- Logical correctness of each plan step
- Missing prerequisites or ordering problems
- Scope creep or tasks that are too large to execute atomically
- Potential edge cases or failure modes not addressed in the plan

### Tips

- ✅ Use the Critic for large, high-risk plans (migrations, new architecture, security changes)
- ✅ Read the Critic's notes alongside the plan — they appear as a separate timeline entry
- ❌ The Critic is not a replacement for your own plan review — treat its output as a second opinion

---

## Quick Reference

```bash
# Entering Plan Mode
/plan <description>       # Create plan
Shift+Tab                 # Toggle mode
[[PLAN]] message          # Prefix trigger

# Managing Plans
Ctrl+Y                    # Open plan.md
/session plan             # View plan in CLI

# Execution
> Start implementation    # Begin
> Pause execution         # Pause
> Resume execution        # Continue
> Skip this task          # Skip current

# Monitoring
/session checkpoints      # View progress
> What's the status?      # Current state
```

---

**Next:** [Best Practices](10-best-practices.md)  
**Previous:** [Advanced Features](08-advanced-features.md)
