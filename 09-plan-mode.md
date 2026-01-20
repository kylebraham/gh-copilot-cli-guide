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
