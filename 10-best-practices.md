# Best Practices

Learn proven strategies and techniques for getting the most out of GitHub Copilot CLI. These best practices will help you work more efficiently and effectively.

## General Principles

### 1. Be Specific and Clear

❌ **Vague:**
```
> Fix the code
> Make it better
> Update the file
```

✅ **Specific:**
```
> @src/auth.js add input validation to the login function
> @utils/helpers.js optimize the sortArray function for performance
> @package.json update React to version 18.2.0
```

### 2. Provide Context

❌ **No context:**
```
> Add error handling
```

✅ **With context:**
```
> @src/api/users.js add error handling to the createUser function
  Handle duplicate email errors and database connection failures
```

### 3. Break Down Complex Tasks

❌ **Too large:**
```
> Build a complete e-commerce platform
```

✅ **Phased approach:**
```
> /plan Phase 1: User authentication and authorization
[Complete Phase 1]

> /plan Phase 2: Product catalog and search
[Complete Phase 2]

> /plan Phase 3: Shopping cart and checkout
[Complete Phase 3]
```

### 4. Review Before Approving

Always review AI-generated code:
- ✅ Check logic correctness
- ✅ Verify security implications
- ✅ Ensure style consistency
- ✅ Look for edge cases
- ✅ Test the changes

### 5. Iterate and Refine

```
> Create a login form

[AI creates basic form]

> Add email validation

[AI adds validation]

> Add password strength indicator

[AI adds indicator]

> Make it responsive for mobile

[AI adds responsive styles]
```

## Context Management

### Keep Context Relevant

✅ **Good:**
```
> /clear
> @src/auth.js
> @src/middleware/auth.js
> Add JWT token refresh functionality
```

Only includes files related to the task.

❌ **Bad:**
```
> @src/**/*.js
> Add JWT token refresh functionality
```

Includes many unrelated files.

### Monitor Context Usage

```
# Check regularly
> /context

# Compact when needed (>70%)
> /compact

# Clear when switching topics
> /clear
```

### Progressive Context Loading

```
# Start minimal
> @src/app.js explain the routing

# Add as needed
> Also need to see @src/routes.js

# Add more if necessary
> And @src/middleware/*.js
```

## File Management

### Use Appropriate Glob Patterns

✅ **Specific patterns:**
```
> @src/api/**/*.js              # All API files
> @src/components/User*.tsx     # User-related components
> @tests/**/*.test.js           # All test files
```

❌ **Too broad:**
```
> @**/*                         # Everything (too much!)
```

### Reference Files Explicitly

When working with specific files:
```
> I need to modify the authentication
> @src/auth/login.js
> @src/auth/register.js
> @src/auth/middleware.js
> Add rate limiting to all three
```

### Organize Related Changes

```
> Create a new feature: user profiles
> Files needed:
  @src/models/Profile.js (create)
  @src/api/profiles.js (create)
  @src/routes/profiles.js (create)
  @src/api/users.js (modify)
> Implement this feature
```

## Prompting Strategies

### Use Action Verbs

```
✅ Create @components/Button.tsx
✅ Update @package.json
✅ Refactor @src/utils.js
✅ Debug @src/api/orders.js
✅ Optimize @src/data-processor.js
✅ Explain @src/complex-algorithm.js
```

### Specify Constraints

```
> @src/api.js add error handling
  Requirements:
  - Use try-catch blocks
  - Log errors with Winston
  - Return proper HTTP status codes
  - Don't break existing functionality
```

### Provide Examples

```
> @src/validators.js create a validator for email
  Example usage:
  validateEmail('test@example.com') // returns true
  validateEmail('invalid') // returns false
  Should check format and common typos
```

### Ask Follow-up Questions

```
> Create a user registration form

[AI creates form]

> Does this handle duplicate emails?

[AI explains]

> Add duplicate email checking

[AI updates]
```

## Workflow Optimization

### Start Each Session Right

```
1. cd ~/projects/my-app
2. copilot
3. /cwd                    # Verify location
4. /model                  # Check/set model
5. /usage                  # Check quota
6. [Begin work]
```

### Use Keyboard Shortcuts

Master these for speed:
```
Ctrl+C       # Cancel/Clear/Exit
Ctrl+O       # Toggle timeline
Ctrl+L       # Clear screen
↑↓           # Command history
Shift+Tab    # Toggle plan mode
```

### Batch Similar Tasks

```
> @src/api/**/*.js add input validation to all POST endpoints

> @src/components/**/*.tsx add PropTypes to all components

> @**/*.js remove all console.log statements
```

### Chain Commands

```
> Create @src/models/User.js with Mongoose schema
  Then create @src/api/users.js with CRUD operations
  Then create @tests/users.test.js with unit tests
  Finally run npm test to verify
```

## Code Quality

### Request Best Practices

```
> @src/api/auth.js review for security issues

> @src/utils/**/*.js ensure all functions have JSDoc comments

> @components/**/*.tsx check for accessibility issues

> @**/*.py format with Black and check PEP 8 compliance
```

### Ask for Explanations

```
> @src/complex-algorithm.js explain this algorithm step by step

> What's the time complexity of @utils/sort.js?

> Why is @src/cache.js using WeakMap instead of Map?
```

### Request Improvements

```
> @src/api.js suggest performance improvements

> @components/LargeList.tsx how can I optimize rendering?

> @src/database.js identify potential memory leaks
```

## Testing

### Write Tests Alongside Code

```
> Create @src/utils/calculator.js with add, subtract, multiply, divide

> Now create @tests/calculator.test.js with comprehensive tests

> Run npm test
```

### Test-Driven Development

```
> Create @tests/auth.test.js with tests for:
  - User registration
  - User login
  - Token validation
  - Password reset

> Now implement @src/auth.js to make these tests pass

> Run tests and iterate
```

### Request Edge Cases

```
> @tests/validator.test.js add tests for edge cases:
  - Empty strings
  - Very long inputs
  - Special characters
  - Unicode
  - Null/undefined
```

## Documentation

### Keep Documentation Updated

```
> @src/api/users.js was just modified
  Update @docs/API.md to reflect the changes

> @package.json dependencies were updated
  Update @README.md installation instructions
```

### Generate Documentation

```
> Create @README.md for this project
  Include: overview, installation, usage, API docs, contributing

> Generate JSDoc for @src/api/**/*.js

> Create @CHANGELOG.md from recent commits
```

### Document Complex Code

```
> @src/algorithm.js add detailed comments explaining:
  - What the algorithm does
  - Time and space complexity
  - Edge cases handled
  - Example inputs and outputs
```

## Error Handling

### Learn from Errors

```
> Run npm test

[Tests fail]

> Why did the auth tests fail?

[AI explains]

> Fix the authentication logic in @src/auth.js

[AI fixes]

> Run npm test again
```

### Provide Error Context

```
> Error when running the app:
  TypeError: Cannot read property 'user' of undefined
  at line 42 in @src/middleware/auth.js
  
  Debug and fix this
```

### Request Defensive Code

```
> @src/api/payment.js add defensive programming:
  - Validate all inputs
  - Handle null/undefined
  - Catch all exceptions
  - Add logging
  - Graceful fallbacks
```

## Collaboration

### Use Consistent Instructions

Create `.github/copilot-instructions.md`:
```markdown
# Project Standards

## Code Style
- TypeScript with strict mode
- ESLint with Airbnb config
- Prettier formatting
- 2 spaces indentation

## Testing
- Jest for unit tests
- 80% minimum coverage
- Test files co-located with source

## Git
- Conventional commits
- Feature branches
- PR required for main
```

### Document Decisions

```
> Why did we choose Redis over Memcached?

[AI explains based on project context]

> Add this to @docs/DECISIONS.md
```

### Share Knowledge

```
> /share file team-session.md
[Share with team]

> Document this pattern in @docs/PATTERNS.md
```

## Performance Tips

### Use Faster Models for Simple Tasks

```
# Simple formatting task
> /model claude-haiku-4.5
> Format @src/**/*.js with Prettier

# Complex refactoring
> /model claude-opus-4.5
> Refactor @src/api/**/*.js to use repository pattern
```

### Compact Regularly

```
# Set up automatic compacting
~/.copilot/config.json:
{
  "autoCompact": true,
  "compactThreshold": 0.7
}

# Or compact manually
> /context
[Shows 75% full]
> /compact
```

### Limit Glob Scope

```
❌ @**/*.js                    # Too broad
✅ @src/**/*.js                # Scoped to src
✅ @src/api/**/*.js            # Even more specific
```

## Security Best Practices

### Never Share Secrets

❌ **Don't:**
```
> My API key is sk-abc123...
> The database password is MyPass123
> Here's the private key: -----BEGIN...
```

✅ **Do:**
```
> Use the API key from environment variable API_KEY
> Read database password from process.env.DB_PASS
> Load private key from file specified in config
```

### Review Security Changes

Always carefully review:
- Authentication code
- Authorization logic
- Input validation
- SQL queries
- File operations
- API endpoints
- Environment configs

### Request Security Audits

```
> @src/api/**/*.js audit for security vulnerabilities

> @src/auth.js check for common auth issues:
  - SQL injection
  - XSS vulnerabilities
  - CSRF protection
  - Rate limiting
  - Password security
```

## Common Mistakes to Avoid

### Mistake 1: Vague Requests

❌ **Bad:**
```
> Fix the bug
> Make it work
> Update the code
```

✅ **Good:**
```
> Fix the null pointer error in @src/api.js line 42
> Make the login function return proper error codes
> Update @package.json to use React 18
```

### Mistake 2: No File References

❌ **Bad:**
```
> Add error handling
```

✅ **Good:**
```
> @src/api/users.js add error handling to createUser function
```

### Mistake 3: Ignoring Context Limits

❌ **Bad:**
```
> @**/* review everything
[Context overflows]
```

✅ **Good:**
```
> @src/api/*.js review API files
[Review results]
> @src/models/*.js review model files
[Review results]
```

### Mistake 4: No Review

❌ **Bad:**
```
> Create authentication system
[Approve without reading]
```

✅ **Good:**
```
> Create authentication system
[Review generated code carefully]
[Test thoroughly]
[Ask questions about decisions]
> Approve
```

### Mistake 5: Huge Changes at Once

❌ **Bad:**
```
> Rewrite the entire application in TypeScript
```

✅ **Good:**
```
> Convert @src/models/**/*.js to TypeScript
[Complete and test]
> Convert @src/api/**/*.js to TypeScript
[Complete and test]
> Convert @src/utils/**/*.js to TypeScript
[Complete and test]
```

## Efficiency Patterns

### Pattern 1: Investigation → Plan → Execute

```
1. > @src/api/orders.js explain the current implementation
2. > What would be the best way to add caching?
3. > /plan Add Redis caching to order API
4. [Review plan]
5. > Implement the plan
```

### Pattern 2: Parallel Development

```
> Create feature branches for:
  - User profiles
  - Notification system
  - Settings page

[Work on each in separate CLI sessions]

> /resume <session-id>   # Switch between them
```

### Pattern 3: Incremental Refactoring

```
> @src/old-api.js identify functions to refactor

> Refactor processUser function to async/await

> Run tests to verify

> Refactor processOrder function

> Run tests to verify

[Continue incrementally]
```

### Pattern 4: Test → Fix → Verify

```
> Run npm test

> Show me the failing test output

> @src/api/users.js fix the createUser function

> Run npm test again

> Verify all tests pass
```

## Quick Reference Checklist

### Before Starting
- [ ] In correct directory (`/cwd`)
- [ ] Right model selected (`/model`)
- [ ] Context is clear (`/clear` if needed)
- [ ] Quota checked (`/usage`)

### During Work
- [ ] Reference files with `@`
- [ ] Be specific in requests
- [ ] Review AI responses
- [ ] Test changes
- [ ] Monitor context (`/context`)

### When Stuck
- [ ] Explain the problem clearly
- [ ] Show error messages
- [ ] Reference relevant files
- [ ] Ask for explanation
- [ ] Break into smaller steps

### Before Finishing
- [ ] All tests pass
- [ ] Documentation updated
- [ ] Changes reviewed
- [ ] Session saved (`/share`)
- [ ] Context cleared for next time

## Pro Tips

### 1. Use History

```
> ↑    # Previous command
> ↑    # Go back further
> ↓    # Forward in history
```

### 2. Template Prompts

Save common prompts:
```bash
# In your shell aliases
alias cop-test='copilot "Run all tests and fix failures"'
alias cop-review='copilot "@src/**/*.js review for code quality"'
```

### 3. Context Preloading

```
> Let me show you the files I'm working with
> @src/api/users.js
> @src/models/User.js
> @src/middleware/auth.js
> Now I'm ready for questions
```

### 4. Commit Frequently

```
> Make change A
[Review and approve]
> !git add . && git commit -m "Change A"

> Make change B
[Review and approve]
> !git add . && git commit -m "Change B"
```

### 5. Use Multiple Sessions

```
Terminal 1: Feature development
Terminal 2: Bug investigation  
Terminal 3: Code review

Each with separate CLI session
```

## Summary

Key takeaways:
1. **Be specific** - Clear requests get better results
2. **Manage context** - Keep it relevant and compact
3. **Review everything** - Don't blindly approve
4. **Break down tasks** - Smaller steps = better results
5. **Iterate** - Refine through conversation
6. **Test thoroughly** - Verify all changes
7. **Document** - Keep docs up to date
8. **Learn** - Understand AI suggestions

---

**Next:** [Troubleshooting](11-troubleshooting.md)  
**Previous:** [Plan Mode](09-plan-mode.md)
