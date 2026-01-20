# Code Editing and Development

Learn how to use GitHub Copilot CLI for writing, editing, debugging, and understanding code. This guide covers practical development workflows and techniques.

## Writing New Code

### Creating Files from Scratch

```
> Create a Node.js Express server with CORS and basic routes

> Make a React component called UserProfile with props for name and email

> Create a Python class for database connections with connection pooling
```

**The AI will:**
1. Understand requirements
2. Generate appropriate code
3. Choose suitable patterns
4. Include necessary imports
5. Add basic error handling

### Specifying Requirements

Be specific about what you want:

```
> Create a TypeScript interface for a User with:
  - id (number)
  - username (string)
  - email (string)
  - createdAt (Date)
  - role (enum: admin, user, guest)
```

```typescript
// AI generates:
enum UserRole {
  Admin = 'admin',
  User = 'user',
  Guest = 'guest'
}

interface User {
  id: number;
  username: string;
  email: string;
  createdAt: Date;
  role: UserRole;
}
```

### Language-Specific Code

```
> Create a Rust function that reads a file and returns its contents

> Make a Go HTTP handler for user registration

> Write a Swift struct for a Todo item with Codable conformance

> Create a Java class for XML parsing
```

AI understands language idioms and conventions.

## Editing Existing Code

### Basic Edits

```
> @src/app.js add error handling to the login function

> @components/Button.tsx change the color prop to accept hex values

> @utils.py add type hints to all functions
```

### Refactoring

```
> @src/auth.js refactor to use async/await instead of callbacks

> @components/Dashboard.tsx extract the chart logic into a separate component

> @server.js split the routes into separate files

> @utils/helpers.js convert to ES6 modules
```

### Code Improvements

```
> @src/api.js improve error handling

> @components/Form.tsx make this component more accessible

> @utils.py optimize for performance

> @app.js add JSDoc comments to all functions
```

### Style Changes

```
> @src/**/*.js convert single quotes to double quotes

> @**/*.py format with Black style guide

> @src/**/*.ts add prettier formatting

> @**/*.go ensure gofmt compliance
```

## Common Development Tasks

### Adding Features

```
> @src/users.js add password reset functionality

> @api/products.js add pagination to the list endpoint

> @components/Search.tsx add autocomplete feature

> @server.go add health check endpoint
```

### Removing Features

```
> @src/app.js remove the deprecated authentication method

> @components/OldUI.tsx remove all console.log statements

> @utils.js delete unused helper functions
```

### Updating Dependencies

```
> @package.json update React to version 18

> @requirements.txt add requests library version 2.28

> @go.mod upgrade to Go 1.21

> @pom.xml add JUnit 5 dependency
```

## Debugging

### Finding Bugs

```
> @src/calculator.js find the bug in the divide function

> @api/orders.js why is this endpoint returning 500 errors?

> @components/Cart.tsx debug the issue with item quantities not updating
```

### Analyzing Errors

```
> This error occurred: "Cannot read property 'map' of undefined"
  @src/components/List.tsx

> Getting "ECONNREFUSED" when calling the API
  @src/services/api.js

> "Index out of bounds" error in
  @utils/array-helpers.py
```

### Adding Debug Code

```
> @src/auth.js add logging statements to track the login flow

> @api/payment.js add debugging output for the transaction process

> @app.py add print statements to trace execution
```

### Fixing Issues

```
> @src/utils.js fix the memory leak in the caching function

> @server.py fix the race condition in the worker pool

> @api.go handle the nil pointer dereference error
```

## Testing

### Writing Tests

```
> @src/utils.js write unit tests for all functions

> @api/users.js create integration tests for the user endpoints

> @components/Button.tsx write React Testing Library tests

> @calculator.py add pytest tests
```

### Running Tests

```
> Run the test suite

> Execute npm test and show results

> Run only the auth tests

> Test the API endpoints
```

### Fixing Failing Tests

```
> This test is failing: test_user_creation
  @tests/user.test.js
  Fix the implementation

> All tests in @tests/api/*.test.js are failing after my changes
  Help me fix them
```

## Code Understanding

### Explaining Code

```
> Explain how @src/auth.js works

> What does this function do? @utils/parser.js parseJSON

> Walk me through @server.py step by step

> Describe the architecture of @src/app.js
```

### Finding Definitions

```
> Where is the User class defined?

> Show me all places where handleSubmit is called

> Find the implementation of loginUser function
```

### Understanding Dependencies

```
> What external libraries does @src/app.js use?

> Show me all imports in @components/Dashboard.tsx

> What does this code depend on? @api/orders.js
```

### Code Review

```
> Review @src/api/**/*.js for security issues

> Check @components/**/*.tsx for performance problems

> Audit @**/*.py for code quality

> Review @pull-request-diff.txt and provide feedback
```

## Code Generation Patterns

### CRUD Operations

```
> Create CRUD endpoints for a Product model with:
  - id, name, price, description, inStock
  Use Express and MongoDB

> Generate CRUD functions for a User in SQLAlchemy

> Create REST API handlers in Go for a Todo resource
```

### Authentication & Authorization

```
> Create JWT authentication middleware for Express

> Add role-based access control to @api/admin.js

> Implement OAuth2 login flow

> Create password hashing utilities using bcrypt
```

### Database Operations

```
> Create a Mongoose schema for a Blog post

> Write SQL queries for a many-to-many relationship between Users and Roles

> Create Prisma migrations for adding a new Comments table

> Write a database connection pool manager
```

### API Integration

```
> Create a service to call the GitHub API

> Write a wrapper for the Stripe payment API

> Add methods to interact with AWS S3

> Create a GraphQL client for the API endpoint
```

## Language-Specific Examples

### JavaScript/TypeScript

```
> Create a React hook for fetching data with loading and error states

> Write a custom Express middleware for request logging

> Create a TypeScript decorator for method validation

> Generate a Next.js API route for user registration
```

### Python

```
> Create a FastAPI endpoint with Pydantic validation

> Write a Django model for a blog with posts and comments

> Create a Python decorator for timing function execution

> Generate SQLAlchemy models from this database schema
```

### Go

```
> Create a Go struct with JSON tags for API responses

> Write a goroutine pool for parallel processing

> Create an HTTP middleware chain in Go

> Generate a Go interface for repository pattern
```

### Other Languages

```
> Create a Java Spring controller for user management

> Write a Rust async function for file operations

> Create a Swift Combine publisher for network requests

> Generate a C# LINQ query for filtering users
```

## Advanced Editing Techniques

### Multi-File Changes

```
> I'm renaming the User model to Account
  Update @models/User.js to @models/Account.js
  Update all references in @api/**/*.js
  Update imports in @components/**/*.tsx
```

### Pattern Application

```
> Apply the singleton pattern to @services/Database.js

> Convert @api/**/*.js to use repository pattern

> Add factory pattern to @models/**/*.js

> Implement observer pattern in @components/StateManager.ts
```

### Code Migration

```
> Migrate @src/**/*.js from JavaScript to TypeScript

> Convert @components/**/*.jsx from class components to hooks

> Port @server.py from Flask to FastAPI

> Migrate @api/*.js from callbacks to async/await
```

### Bulk Operations

```
> Add error handling to all functions in @src/api/**/*.js

> Add TypeScript strict null checks to @**/*.ts

> Add docstrings to all functions in @**/*.py

> Remove all console.log from @src/**/*.js
```

## Working with Frameworks

### React

```
> Create a React context for theme management

> Add React Router to @src/App.jsx with routes for home, about, contact

> Create a custom hook for form validation

> Add Redux Toolkit store with slices for users and posts
```

### Vue

```
> Create a Vue 3 component using Composition API

> Add Vuex store module for shopping cart

> Create Vue Router setup with nested routes

> Generate Pinia store for user state
```

### Angular

```
> Create an Angular service for HTTP requests

> Generate an Angular component with reactive forms

> Add NgRx store for application state

> Create Angular guards for route protection
```

### Backend Frameworks

```
> Create Express.js middleware for authentication

> Add Django REST Framework serializers for User model

> Create Flask blueprints for API organization

> Generate NestJS controller with dependency injection
```

## Documentation

### Adding Comments

```
> @src/utils.js add JSDoc comments to all functions

> @api/users.py add docstrings following Google style

> @server.go add GoDoc comments to exported functions

> @Calculator.java add JavaDoc to public methods
```

### Generating README

```
> Create a README.md for this project
  @package.json
  @src/**/*.js

> Generate API documentation from @api/**/*.js

> Create a CONTRIBUTING.md file with guidelines
```

### Code Examples

```
> Add usage examples to @src/api-client.js

> Create example code for @lib/database.py

> Generate sample requests for @api/endpoints.js
```

## Performance Optimization

### Analyzing Performance

```
> @src/data-processor.js identify performance bottlenecks

> @api/search.py optimize the search function

> @components/LargeList.tsx improve rendering performance
```

### Specific Optimizations

```
> Add memoization to @utils/expensive-calculation.js

> Implement lazy loading in @components/ImageGallery.tsx

> Add database indexing suggestions for @models/User.js

> Optimize SQL query in @api/reports.py
```

## Error Handling

### Adding Error Handling

```
> @src/api.js add try-catch blocks to all async functions

> @file-reader.py add exception handling for file operations

> @server.go add error checking for all function calls
```

### Custom Error Classes

```
> Create custom error classes for different API error types

> Generate error handling middleware for Express

> Create error response formatter for @api/**/*.js
```

## Best Practices

### DO:

✅ **Be specific** - Describe exactly what you want  
✅ **Mention files** - Use `@` to reference code  
✅ **Provide context** - Explain the broader goal  
✅ **Review changes** - Check AI output carefully  
✅ **Iterate** - Refine through conversation  

### DON'T:

❌ **Vague requests** - "fix the code"  
❌ **No context** - AI needs to see the code  
❌ **Blind approval** - Review before accepting  
❌ **Huge changes** - Break into smaller tasks  
❌ **Ignore errors** - Address issues when they arise  

## Practical Workflows

### Workflow 1: New Feature

```
1. > /clear
2. > @src/api/users.js
3. > Add password reset functionality with:
     - POST /api/users/reset-password-request
     - POST /api/users/reset-password
     - Email token generation
     - Token expiration (24 hours)
4. [Review and approve]
5. > Create tests for this feature
6. > Run npm test
```

### Workflow 2: Bug Fix

```
1. > The login is broken, getting 401 errors
2. > @src/api/auth.js
3. > @src/middleware/verify.js
4. > Debug why authentication is failing
5. [AI finds issue]
6. > Fix the token verification logic
7. > Test the fix with curl
```

### Workflow 3: Refactoring

```
1. > @src/components/Dashboard.tsx
2. > This component is too large, help me refactor
3. [AI suggests breakdown]
4. > Extract the charts into @components/Charts.tsx
5. > Extract the data fetching into a custom hook
6. > Update Dashboard to use new structure
7. > Run tests to verify nothing broke
```

### Workflow 4: Learning

```
1. > @src/advanced-feature.js
2. > Explain how this code works
3. [AI explains]
4. > What's the purpose of the decorator pattern here?
5. [AI clarifies]
6. > Show me a simpler example of this pattern
7. [AI provides example]
```

## Quick Reference

```bash
# Code creation
Create <file> with <requirements>
Generate <component> using <framework>

# Code editing
@file.js add <feature>
@file.py refactor <section>
@file.ts improve <aspect>

# Debugging
@file.js find the bug
Fix error: <message>
Debug why <behavior>

# Testing
Write tests for @file.js
Run tests
Fix failing test: <name>

# Understanding
Explain @file.js
Where is <function> defined?
Review @file.js for <concern>
```

---

**Next:** [GitHub Integration](07-github-integration.md)  
**Previous:** [File and Context Management](05-file-context.md)
