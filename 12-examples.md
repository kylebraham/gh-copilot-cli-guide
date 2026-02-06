# Examples and Tutorials

Practical, hands-on examples and tutorials to help you master GitHub Copilot CLI. Each example is runnable and demonstrates real-world use cases.

## Table of Contents

1. [Getting Started Examples](#getting-started-examples)
2. [Project Initialization with /init](#project-initialization-with-init)
3. [Pull Request Workflows with /delegate](#pull-request-workflows-with-delegate)
4. [Web Development](#web-development)
5. [API Development](#api-development)
6. [Database Operations](#database-operations)
7. [Testing](#testing)
8. [Refactoring](#refactoring)
9. [Debugging](#debugging)
10. [Documentation](#documentation)
11. [DevOps and Scripts](#devops-and-scripts)
12. [Full Project Tutorials](#full-project-tutorials)

## Getting Started Examples

### Example 1: Hello World

**Goal:** Create and run your first program.

```bash
# Start CLI in an empty directory
mkdir ~/copilot-tutorial
cd ~/copilot-tutorial
copilot
```

```
> Create a hello.js file that prints "Hello, GitHub Copilot CLI!"

✅ File created

> Run the file

[AI executes: node hello.js]
Hello, GitHub Copilot CLI!
```

**Expected output:**
```javascript
// hello.js
console.log("Hello, GitHub Copilot CLI!");
```

### Example 2: Simple Calculator

**Goal:** Create a calculator with basic operations.

```
> Create calculator.js with functions for add, subtract, multiply, divide
  Each function should take two numbers and return the result
  Add input validation

> Run examples:
  console.log(add(5, 3))
  console.log(subtract(10, 4))
  console.log(multiply(6, 7))
  console.log(divide(20, 4))

> Execute node calculator.js
```

### Example 3: File Operations

**Goal:** Read and write files.

```
> Create data.json with sample user data:
  [
    {"id": 1, "name": "Alice", "email": "alice@example.com"},
    {"id": 2, "name": "Bob", "email": "bob@example.com"}
  ]

> Create reader.js that:
  - Reads data.json
  - Filters users by name
  - Displays results

> Run reader.js with filter "Alice"
```

## Project Initialization with /init

The `/init` command (new in v0.405) scaffolds complete projects with best practices built-in.

### Example 4: React Application with TypeScript

**Goal:** Create a production-ready React app.

```bash
mkdir ~/my-projects
cd ~/my-projects
copilot
```

```
> /init react-app

AI: I'll create a new React application. A few questions:

📝 What's your project name?
> react-dashboard

📝 Use TypeScript?
> yes

📝 Include React Router?
> yes

📝 State management (Redux/Zustand/Context/None)?
> zustand

📝 UI library (Material-UI/Chakra/Tailwind/None)?
> tailwind

📝 Testing library?
> vitest

AI: Creating React + TypeScript app with:
  ✓ Vite as build tool
  ✓ React Router for navigation
  ✓ Zustand for state management
  ✓ Tailwind CSS for styling
  ✓ Vitest + Testing Library for tests

📁 Creating project structure...
   ✓ react-dashboard/
   ✓ src/
     ✓ components/
     ✓ pages/
     ✓ store/
     ✓ utils/
     ✓ __tests__/
   ✓ public/

📦 Generating configuration...
   ✓ package.json
   ✓ tsconfig.json
   ✓ vite.config.ts
   ✓ tailwind.config.js
   ✓ vitest.config.ts
   ✓ .gitignore
   ✓ .eslintrc.json

📝 Creating starter files...
   ✓ src/App.tsx
   ✓ src/main.tsx
   ✓ src/store/useStore.ts
   ✓ src/components/Layout.tsx
   ✓ src/pages/Home.tsx
   ✓ README.md

⬇️  Installing dependencies...
   ✓ react, react-dom, react-router-dom
   ✓ zustand
   ✓ tailwindcss
   ✓ vite, @vitejs/plugin-react
   ✓ vitest, @testing-library/react
   ✓ typescript, @types/react

✅ Project created successfully!

Next steps:
  cd react-dashboard
  npm run dev          # Start development server
  npm run test         # Run tests
  npm run build        # Build for production
```

**What you get:**
- Fully configured TypeScript + React project
- Pre-configured routing with example pages
- State management setup with Zustand
- Tailwind CSS with base configuration
- Testing environment ready to use
- ESLint and Prettier configured
- Example components demonstrating best practices

### Example 5: Express.js REST API

**Goal:** Create a production-ready API server.

```
> /init express-api

AI: Setting up Express.js REST API. Questions:

📝 Project name?
> user-api

📝 Use TypeScript?
> yes

📝 Database (PostgreSQL/MySQL/MongoDB/None)?
> postgresql

📝 ORM (Prisma/TypeORM/Sequelize/None)?
> prisma

📝 Authentication (JWT/OAuth2/Passport/None)?
> jwt

📝 Include API documentation (Swagger)?
> yes

📝 Testing framework (Jest/Mocha/None)?
> jest

AI: Creating Express + TypeScript API with:
  ✓ PostgreSQL database
  ✓ Prisma ORM
  ✓ JWT authentication
  ✓ Swagger/OpenAPI docs
  ✓ Jest for testing

📁 Creating project structure...
   ✓ user-api/
   ✓ src/
     ✓ controllers/
     ✓ routes/
     ✓ middleware/
     ✓ models/
     ✓ services/
     ✓ utils/
     ✓ __tests__/
   ✓ prisma/

📦 Generating configuration...
   ✓ package.json
   ✓ tsconfig.json
   ✓ .env.example
   ✓ .gitignore
   ✓ jest.config.js
   ✓ docker-compose.yml (for local PostgreSQL)

📝 Creating application files...
   ✓ src/index.ts (Server entry point)
   ✓ src/app.ts (Express app configuration)
   ✓ src/routes/index.ts
   ✓ src/routes/users.ts
   ✓ src/routes/auth.ts
   ✓ src/controllers/userController.ts
   ✓ src/controllers/authController.ts
   ✓ src/middleware/auth.ts
   ✓ src/middleware/errorHandler.ts
   ✓ src/middleware/validator.ts
   ✓ src/services/userService.ts
   ✓ src/services/authService.ts
   ✓ src/utils/jwt.ts
   ✓ src/utils/logger.ts
   ✓ prisma/schema.prisma
   ✓ src/swagger.ts (API documentation)
   ✓ README.md

⬇️  Installing dependencies...
   ✓ express, cors, helmet, dotenv
   ✓ prisma, @prisma/client
   ✓ jsonwebtoken, bcrypt
   ✓ swagger-jsdoc, swagger-ui-express
   ✓ winston (logging)
   ✓ jest, supertest, @types/*

✅ API project created!

Next steps:
  cd user-api
  cp .env.example .env          # Configure environment
  docker-compose up -d          # Start PostgreSQL
  npx prisma migrate dev        # Run database migrations
  npm run dev                   # Start API server
  
  API will be available at: http://localhost:3000
  API docs at: http://localhost:3000/api-docs
```

**Generated API structure:**
```
GET    /api/v1/users          - List users
GET    /api/v1/users/:id      - Get user by ID
POST   /api/v1/users          - Create user
PUT    /api/v1/users/:id      - Update user
DELETE /api/v1/users/:id      - Delete user

POST   /api/v1/auth/register  - Register new user
POST   /api/v1/auth/login     - Login user
POST   /api/v1/auth/refresh   - Refresh token
POST   /api/v1/auth/logout    - Logout user
```

### Example 6: Python Flask Application

**Goal:** Create a Flask web application.

```
> /init python-flask

AI: Setting up Flask application. Questions:

📝 Project name?
> blog-app

📝 Database (PostgreSQL/MySQL/SQLite)?
> sqlite

📝 ORM (SQLAlchemy/Peewee/None)?
> sqlalchemy

📝 Template engine (Jinja2/None - API only)?
> jinja2

📝 Authentication (Flask-Login/JWT/None)?
> flask-login

📝 Admin interface (Flask-Admin)?
> yes

AI: Creating Flask application with:
  ✓ SQLite database
  ✓ SQLAlchemy ORM
  ✓ Jinja2 templates
  ✓ Flask-Login authentication
  ✓ Flask-Admin interface

📁 Creating project structure...
   ✓ blog-app/
   ✓ app/
     ✓ models/
     ✓ routes/
     ✓ templates/
     ✓ static/
       ✓ css/
       ✓ js/
     ✓ forms/
   ✓ migrations/
   ✓ tests/

📦 Generating configuration...
   ✓ requirements.txt
   ✓ config.py
   ✓ .env.example
   ✓ .gitignore
   ✓ pytest.ini

📝 Creating application files...
   ✓ run.py
   ✓ app/__init__.py
   ✓ app/models/user.py
   ✓ app/models/post.py
   ✓ app/routes/main.py
   ✓ app/routes/auth.py
   ✓ app/routes/posts.py
   ✓ app/forms/login.py
   ✓ app/forms/post.py
   ✓ app/templates/base.html
   ✓ app/templates/index.html
   ✓ app/templates/login.html
   ✓ app/static/css/style.css
   ✓ README.md

⬇️  Creating virtual environment...
   ✓ python -m venv venv

⬇️  Installing dependencies...
   ✓ Flask
   ✓ Flask-SQLAlchemy
   ✓ Flask-Login
   ✓ Flask-Admin
   ✓ Flask-WTF
   ✓ Flask-Migrate
   ✓ python-dotenv
   ✓ pytest

✅ Flask app created!

Next steps:
  cd blog-app
  source venv/bin/activate      # Activate virtual environment
  cp .env.example .env          # Configure environment
  flask db upgrade              # Initialize database
  python run.py                 # Start development server
  
  App: http://localhost:5000
  Admin: http://localhost:5000/admin
```

### Example 7: Next.js Full-Stack Application

**Goal:** Create a Next.js app with API routes.

```
> /init nextjs

AI: Creating Next.js application. Questions:

📝 Project name?
> ecommerce-store

📝 Use TypeScript?
> yes

📝 App Router or Pages Router?
> app

📝 UI Framework (Tailwind/styled-components/CSS Modules)?
> tailwind

📝 Database (Supabase/Prisma+PostgreSQL/MongoDB/None)?
> prisma

📝 Authentication (NextAuth.js/Clerk/None)?
> nextauth

📝 Include (Select multiple):
  ☑ ESLint
  ☑ Prettier
  ☑ Husky (Git hooks)
  ☑ Testing (Jest + React Testing Library)
  ☑ Storybook

AI: Creating Next.js app with App Router...

📁 Creating project structure...
   ✓ ecommerce-store/
   ✓ app/
     ✓ api/
     ✓ (auth)/
     ✓ products/
     ✓ cart/
     ✓ layout.tsx
     ✓ page.tsx
   ✓ components/
   ✓ lib/
   ✓ prisma/
   ✓ public/
   ✓ __tests__/

📦 Configuration files...
   ✓ next.config.js
   ✓ tsconfig.json
   ✓ tailwind.config.ts
   ✓ postcss.config.js
   ✓ .eslintrc.json
   ✓ .prettierrc
   ✓ jest.config.js
   ✓ prisma/schema.prisma
   ✓ .env.example

📝 Creating application files...
   ✓ app/layout.tsx
   ✓ app/page.tsx
   ✓ app/api/auth/[...nextauth]/route.ts
   ✓ app/api/products/route.ts
   ✓ components/Navbar.tsx
   ✓ components/ProductCard.tsx
   ✓ lib/auth.ts
   ✓ lib/db.ts
   ✓ prisma/schema.prisma

⬇️  Installing dependencies...
   ✓ next, react, react-dom
   ✓ next-auth
   ✓ @prisma/client, prisma
   ✓ tailwindcss
   ✓ jest, @testing-library/react

✅ Next.js app ready!

Next steps:
  cd ecommerce-store
  cp .env.example .env
  # Add DATABASE_URL and NEXTAUTH_SECRET to .env
  npx prisma migrate dev
  npm run dev
  
  Dev server: http://localhost:3000
```

## Pull Request Workflows with /delegate

The `/delegate` command (enhanced in v0.405) automates the entire PR creation workflow.

### Example 8: Quick Bug Fix

**Goal:** Fix a bug and create PR in one command.

```bash
# Setup: You're in a git repository
cd ~/my-project
copilot
```

```
> /delegate Fix the authentication timeout bug reported in issue #234

AI: Analyzing issue #234...

Issue #234: "Users are logged out after 5 minutes"
- Reported by: @user123
- Labels: bug, authentication
- Mentioned files: src/auth/session.js

AI: I found the issue. The session timeout is hardcoded to 5 minutes 
instead of using the config value.

Plan:
1. Update src/auth/session.js to use config.SESSION_TIMEOUT
2. Add fallback default of 30 minutes
3. Update tests

Proceed with these changes? (y/n)
> y

🔧 Creating branch: fix/auth-timeout-234
📝 Making changes...
   ✓ src/auth/session.js - Fixed timeout to use config
   ✓ tests/auth.test.js - Added timeout tests
   
🧪 Running tests...
   ✓ All tests passed (47/47)

💾 Committing changes...
   ✓ Commit: "Fix authentication timeout to use config value"
   ✓ Body: "Fixes #234\n\n- Changed hardcoded 5min timeout to use SESSION_TIMEOUT config\n- Added default fallback of 30 minutes\n- Updated tests"

⬆️  Pushing to origin/fix/auth-timeout-234...

🔗 Creating Pull Request...
   ✓ PR #235 created: "Fix authentication timeout bug"
   ✓ Linked to issue #234
   ✓ Added labels: bug, authentication
   ✓ Requested review from @team-backend
   
   https://github.com/user/my-project/pull/235

✅ Done! PR is ready for review.
```

### Example 9: Feature Implementation

**Goal:** Implement a new feature with comprehensive changes.

```
> /delegate Implement user profile editing feature
  Requirements:
  - Allow users to update name, email, bio, avatar
  - Add form validation
  - Include API endpoint
  - Add tests
  - Update documentation

AI: I'll implement the profile editing feature. Here's my plan:

📋 Implementation Plan:
1. Backend:
   - Create PUT /api/users/:id/profile endpoint
   - Add input validation middleware
   - Implement avatar upload handling
   - Add database migration if needed

2. Frontend:
   - Create ProfileEditForm component
   - Add form validation (React Hook Form)
   - Implement avatar upload with preview
   - Add success/error notifications

3. Testing:
   - Unit tests for API endpoint
   - Component tests for form
   - Integration tests for full flow

4. Documentation:
   - Update API docs
   - Add component documentation

Estimated changes: ~12 files
Continue? (y/n/customize plan)
> y

🔧 Creating branch: feature/profile-editing
📝 Making changes...

Backend:
   ✓ src/api/routes/users.js - Added profile endpoint
   ✓ src/middleware/validation.js - Added profile validators
   ✓ src/services/upload.js - Avatar upload service
   ✓ src/models/User.js - Updated schema
   
Frontend:
   ✓ src/components/ProfileEditForm.jsx - New component
   ✓ src/components/AvatarUpload.jsx - New component
   ✓ src/hooks/useProfileUpdate.js - Custom hook
   ✓ src/pages/ProfileEdit.jsx - New page
   ✓ src/utils/validators.js - Form validation rules
   
Tests:
   ✓ tests/api/profile.test.js - API tests
   ✓ tests/components/ProfileEditForm.test.jsx - Component tests
   ✓ tests/integration/profile-flow.test.js - E2E tests
   
Documentation:
   ✓ docs/API.md - Updated endpoints
   ✓ docs/components/ProfileEditForm.md - Component docs
   ✓ README.md - Updated features list

📊 Changes summary:
   - 15 files changed
   - 847 lines added
   - 23 lines deleted
   - 4 new files created

🧪 Running test suite...
   ✓ API tests: 12/12 passed
   ✓ Component tests: 8/8 passed
   ✓ Integration tests: 5/5 passed
   ✓ Code coverage: 94%

💾 Committing changes...
   ✓ Commit: "feat: Add user profile editing feature"
   ✓ Body includes detailed description

⬆️  Pushing to origin/feature/profile-editing...

🔗 Creating Pull Request...
   ✓ PR #236: "Add user profile editing feature"
   ✓ Description includes:
     - Feature overview
     - Implementation details
     - Testing notes
     - Screenshots (placeholder)
     - Checklist for reviewers
   ✓ Labels: feature, frontend, backend
   ✓ Linked to project board
   ✓ Requested reviews from @frontend-team, @backend-team
   
   https://github.com/user/my-project/pull/236

✅ Feature PR created! Remember to add screenshots/demo.
```

### Example 10: Refactoring with Tests

**Goal:** Refactor code while ensuring tests pass.

```
> /delegate Refactor authentication module to use dependency injection
  Make sure all tests pass before creating PR

AI: Analyzing authentication module...

Current structure:
- src/auth/AuthService.js - 450 lines, tightly coupled
- src/auth/TokenManager.js - mixed responsibilities
- src/auth/UserValidator.js - hard dependencies

Refactoring plan:
1. Extract interfaces for dependencies
2. Implement dependency injection container
3. Refactor AuthService to accept dependencies
4. Update TokenManager with interface
5. Ensure all tests pass
6. No breaking changes to public API

This is a significant refactor. Proceed? (y/n)
> y

🔧 Creating branch: refactor/auth-dependency-injection
📝 Making changes...

Phase 1: Create interfaces
   ✓ src/auth/interfaces/ITokenManager.js
   ✓ src/auth/interfaces/IUserRepository.js
   ✓ src/auth/interfaces/IPasswordHasher.js

Phase 2: Dependency injection setup
   ✓ src/di/container.js - DI container
   ✓ src/di/authModule.js - Auth module registration

Phase 3: Refactor implementations
   ✓ src/auth/AuthService.js - Now accepts dependencies
   ✓ src/auth/TokenManager.js - Implements ITokenManager
   ✓ src/auth/PasswordHasher.js - Implements IPasswordHasher
   ✓ src/repositories/UserRepository.js - Implements IUserRepository

Phase 4: Update consumers
   ✓ src/api/routes/auth.js - Use DI container
   ✓ src/middleware/auth.js - Use DI container

Phase 5: Update tests
   ✓ tests/auth/AuthService.test.js - Now uses mocks
   ✓ tests/auth/TokenManager.test.js - Updated
   ✓ tests/integration/auth.test.js - Updated

🧪 Running full test suite...
   ✓ Unit tests: 89/89 passed
   ✓ Integration tests: 23/23 passed
   ✓ No breaking changes detected
   ✓ Code coverage: 96% (+2%)

📊 Refactoring metrics:
   - Cyclomatic complexity: 45 → 28 (↓ 38%)
   - Coupling: High → Low
   - Testability: Medium → High
   - Lines changed: ~800

💾 Committing changes...
   ✓ Commit: "refactor: Implement dependency injection in auth module"
   ✓ Detailed commit message with rationale

⬆️  Pushing to origin/refactor/auth-dependency-injection...

🔗 Creating Pull Request...
   ✓ PR #237: "Refactor auth module with dependency injection"
   ✓ Description includes:
     - Refactoring rationale
     - Before/after comparison
     - Breaking changes: None
     - Migration guide: Not needed
     - Test coverage report
   ✓ Labels: refactoring, tech-debt
   ✓ Marked as: Ready for review
   
   https://github.com/user/my-project/pull/237

✅ Refactoring PR created with all tests passing!
```

### Example 11: Multi-Repository Update

**Goal:** Update a dependency across multiple repositories.

```
> I need to update the authentication library from v2 to v3 across all our services

AI: I can help with that! However, /delegate works on one repository at a time.

Here's the recommended approach:

Option 1: Manual iteration (safer)
1. List affected repositories
2. Use /delegate for each repo individually
3. Coordinate reviews and merges

Option 2: Create a script
Let me create a bash script to automate this.

Which would you prefer?
> Option 1, start with the user-service repo

> /delegate Update auth-lib from v2.4.1 to v3.0.0
  Check breaking changes and update code accordingly
  Test all auth flows

AI: Checking auth-lib v3.0.0 breaking changes...

Breaking changes found:
1. `login()` now returns Promise instead of callback
2. `verifyToken()` signature changed
3. Config format updated
4. Deprecated methods removed

I'll update all usage. Proceed? (y/n)
> y

🔧 Creating branch: deps/auth-lib-v3
📝 Making changes...
   ✓ package.json - Update dependency
   ✓ src/services/authService.js - Convert to async/await
   ✓ src/middleware/verifyToken.js - Update signature
   ✓ config/auth.config.js - New config format
   ✓ tests/**/*.test.js - Update all tests

🧪 Running tests...
   ✓ All auth tests passed
   
💾 Committing...
⬆️  Pushing...
🔗 Created PR #101: "Update auth-lib to v3.0.0"

Next: Apply the same pattern to other repos
```

### Example 12: Documentation-Only PR

**Goal:** Update documentation based on code changes.

```
> /delegate Update API documentation to reflect the new pagination format

AI: Analyzing API endpoints for pagination usage...

Found pagination in:
- GET /api/users
- GET /api/products
- GET /api/orders
- GET /api/transactions

Current docs: Old format (page/limit)
Actual code: New format (cursor-based)

I'll update docs to match implementation.

🔧 Creating branch: docs/update-pagination
📝 Making changes...
   ✓ docs/API.md - Updated pagination examples
   ✓ docs/guides/pagination.md - New guide
   ✓ README.md - Updated API overview
   ✓ openapi.yaml - Updated schema

💾 Committing: "docs: Update pagination documentation"
⬆️  Pushing...
🔗 Created PR #238: "Update API pagination documentation"
   Label: documentation
   
   https://github.com/user/my-project/pull/238
```

**Tips for /delegate:**
- Start with small, focused changes to build confidence
- Review the AI's plan before approving
- Use with issue numbers for automatic linking
- Test complex changes locally first
- Great for repetitive tasks across files
- Always review the PR before merging



## Web Development

### Example 4: Express.js API Server

**Goal:** Create a basic REST API.

```bash
mkdir express-api
cd express-api
copilot
```

```
> Initialize a Node.js project with Express

> Create server.js with:
  - Express server on port 3000
  - CORS middleware
  - JSON body parser
  - Health check endpoint at GET /health
  - Start the server

> Run the server

> Test with: curl http://localhost:3000/health
```

**Expected structure:**
```
express-api/
├── package.json
├── server.js
└── node_modules/
```

### Example 5: React Component

**Goal:** Create a React component with props and state.

```bash
mkdir react-example
cd react-example
copilot
```

```
> Initialize a React project with create-react-app

[Wait for completion]

> Create src/components/UserCard.jsx with:
  - Props: name, email, avatar
  - Show user information in a card
  - Include a "Follow" button
  - Track follow state with useState
  - Style with inline CSS

> Show me how to use this component in App.js

> Run npm start
```

### Example 6: HTML/CSS/JS Portfolio Page

**Goal:** Build a simple portfolio page.

```
> Create index.html with:
  - Header with name and title
  - About section
  - Projects section (3 sample projects)
  - Contact section
  - Responsive design

> Create styles.css with:
  - Modern, clean design
  - Mobile-responsive
  - Dark theme
  - Smooth scrolling

> Create script.js with:
  - Smooth scroll to sections
  - Form validation for contact
  - Mobile menu toggle

> Open index.html in browser
```

## API Development

### Example 7: RESTful CRUD API

**Goal:** Build a complete CRUD API for tasks.

```bash
mkdir task-api
cd task-api
git init
gh repo create task-api --private --source=. --remote=origin
copilot
```

```
> /plan Create a REST API for task management with:
  - Express.js framework
  - In-memory data storage (array)
  - CRUD endpoints for tasks
  - Task: id, title, description, completed, createdAt
  - Input validation
  - Error handling

[Review plan]

> Implement the plan

> Test each endpoint:
  POST /api/tasks
  GET /api/tasks
  GET /api/tasks/:id
  PUT /api/tasks/:id
  DELETE /api/tasks/:id

> Show example curl commands for testing
```

### Example 8: GraphQL API

**Goal:** Create a GraphQL API with queries and mutations.

```
> Initialize project with express and graphql

> Create schema.js with GraphQL schema:
  - User type (id, name, email)
  - Query: users, user(id)
  - Mutation: createUser(name, email)

> Create resolvers.js with:
  - In-memory user storage
  - Resolver functions

> Create server.js with Apollo Server

> Run and test in GraphQL Playground

> Show example queries and mutations
```

### Example 9: Authentication API

**Goal:** Add JWT authentication to an API.

```
> @package.json add jsonwebtoken and bcryptjs

> Create auth.js middleware:
  - Generate JWT tokens
  - Verify JWT tokens
  - Hash passwords
  - Compare passwords

> Create routes/auth.js:
  - POST /register (username, email, password)
  - POST /login (email, password)
  - GET /profile (protected, requires token)

> Create example requests with curl
```

## Database Operations

### Example 10: MongoDB with Mongoose

**Goal:** Connect to MongoDB and perform CRUD operations.

```
> Initialize project with mongoose

> Create models/User.js:
  - Schema: username, email, password, createdAt
  - Validation
  - Unique email

> Create db.js:
  - MongoDB connection
  - Connection error handling

> Create userController.js:
  - createUser
  - getUsers
  - getUserById
  - updateUser
  - deleteUser

> Show examples of using each function
```

### Example 11: PostgreSQL with Sequelize

**Goal:** Use PostgreSQL with an ORM.

```
> Install sequelize and pg

> Create config/database.js:
  - Sequelize connection
  - Database: postgres://localhost/myapp

> Create models/Product.js:
  - id, name, price, description, stock
  - Validations

> Create migrations for products table

> Show examples:
  - Create product
  - Find all products
  - Find by price range
  - Update stock
  - Delete product

> Run migrations
```

### Example 12: SQLite with Better-sqlite3

**Goal:** Local database for development.

```
> Install better-sqlite3

> Create database.js:
  - Initialize SQLite database
  - Create tasks table
  - CRUD functions

> Create seed.js:
  - Insert sample data

> Create queries.js:
  - Get all tasks
  - Get completed tasks
  - Get tasks by date
  - Search tasks

> Run seed and test queries
```

## Testing

### Example 13: Unit Tests with Jest

**Goal:** Write comprehensive unit tests.

```
> @src/utils/math.js contains add, subtract, multiply, divide

> Create tests/math.test.js with Jest:
  - Test each function with normal inputs
  - Test edge cases (0, negative, large numbers)
  - Test error cases (divide by zero)
  - Test floating point precision

> Run npm test

> Show coverage report
```

### Example 14: API Integration Tests

**Goal:** Test API endpoints.

```
> @server.js contains Express API

> Install supertest

> Create tests/api.test.js:
  - Test GET /api/users
  - Test POST /api/users (valid data)
  - Test POST /api/users (invalid data)
  - Test PUT /api/users/:id
  - Test DELETE /api/users/:id
  - Test authentication endpoints

> Run tests

> Show test coverage
```

### Example 15: React Component Tests

**Goal:** Test React components.

```
> @src/components/LoginForm.jsx

> Create LoginForm.test.jsx with React Testing Library:
  - Test component renders
  - Test form inputs work
  - Test form validation
  - Test submit handler
  - Test error display
  - Test loading state

> Run npm test
```

## Refactoring

### Example 16: Callback to Async/Await

**Goal:** Modernize legacy code.

```
> @src/api/users.js uses callbacks

> Show me the current implementation

> Refactor all functions to use async/await:
  - getUsers
  - createUser
  - updateUser
  - deleteUser

> Maintain the same functionality

> Run tests to verify nothing broke
```

### Example 17: Class Components to Hooks

**Goal:** Convert React class components to functional components.

```
> @src/components/Timer.jsx is a class component

> Convert to functional component using hooks:
  - Use useState for state
  - Use useEffect for lifecycle
  - Maintain same functionality

> Verify component still works

> Run tests
```

### Example 18: Monolith to Modules

**Goal:** Break large file into modules.

```
> @src/app.js is 800 lines

> Analyze the file and suggest module structure

> Extract into separate files:
  - routes.js
  - controllers/
  - middleware/
  - utils/
  - config/

> Update imports in app.js

> Verify server still runs

> Run tests
```

## Debugging

### Example 19: Find and Fix a Bug

**Goal:** Debug a broken function.

**Setup:**
```javascript
// bug.js
function calculateTotal(items) {
  let total = 0;
  for (let i = 0; i <= items.length; i++) {
    total += items[i].price;
  }
  return total;
}

const items = [
  { name: 'Apple', price: 1.00 },
  { name: 'Banana', price: 0.50 }
];

console.log(calculateTotal(items));
```

```
> @bug.js has an error

> Run node bug.js

[Error shown]

> Debug the calculateTotal function

> What's the bug?

> Fix the bug

> Run node bug.js again

> Verify it works correctly
```

### Example 20: Memory Leak Investigation

**Goal:** Find and fix a memory leak.

```javascript
// leak.js
const users = [];

function addUser(name) {
  const user = { name, timestamp: Date.now() };
  users.push(user);
  setInterval(() => {
    console.log(`User ${name} active`);
  }, 1000);
}

// Adding many users causes memory leak
for (let i = 0; i < 100; i++) {
  addUser(`User${i}`);
}
```

```
> @leak.js has a memory leak

> Analyze the code and identify the leak

> Explain the problem

> Fix the memory leak

> Show the corrected code

> Explain how to prevent this in the future
```

### Example 21: Performance Optimization

**Goal:** Optimize slow code.

```javascript
// slow.js
function findDuplicates(arr) {
  const duplicates = [];
  for (let i = 0; i < arr.length; i++) {
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[i] === arr[j] && !duplicates.includes(arr[i])) {
        duplicates.push(arr[i]);
      }
    }
  }
  return duplicates;
}

const bigArray = Array.from({ length: 10000 }, () => 
  Math.floor(Math.random() * 1000)
);

console.time('findDuplicates');
console.log(findDuplicates(bigArray));
console.timeEnd('findDuplicates');
```

```
> @slow.js is slow with large arrays

> Analyze the time complexity

> Optimize the findDuplicates function

> Run both versions and compare performance

> Explain the optimization
```

## Documentation

### Example 22: Generate README

**Goal:** Create comprehensive README.

```
> @package.json
> @src/**/*.js

> Create README.md with:
  - Project title and description
  - Features list
  - Installation instructions
  - Usage examples
  - API documentation
  - Contributing guidelines
  - License

> Make it professional and complete
```

### Example 23: JSDoc Documentation

**Goal:** Add JSDoc comments to code.

```
> @src/utils/**/*.js

> Add JSDoc comments to all functions:
  - Description
  - @param with types
  - @returns with type
  - @throws for errors
  - @example with usage

> Generate HTML documentation from JSDoc

> Show me the command to generate docs
```

### Example 24: API Documentation

**Goal:** Create OpenAPI/Swagger docs.

```
> @src/routes/**/*.js contains Express routes

> Generate OpenAPI 3.0 specification with:
  - All endpoints documented
  - Request/response schemas
  - Authentication schemes
  - Example requests
  - Error responses

> Create swagger.yaml

> Show how to view docs with Swagger UI
```

## DevOps and Scripts

### Example 25: Build Script

**Goal:** Automate build process.

```
> Create build.sh script that:
  - Cleans dist/ directory
  - Runs linter
  - Runs tests
  - Builds project
  - Copies necessary files
  - Creates zip archive
  - Shows success message

> Make it executable

> Test the script
```

### Example 26: Docker Setup

**Goal:** Containerize an application.

```
> @package.json
> @server.js

> Create Dockerfile:
  - Node.js base image
  - Copy package files
  - Install dependencies
  - Copy source code
  - Expose port
  - Start command

> Create docker-compose.yml:
  - App service
  - MongoDB service
  - Environment variables
  - Volumes

> Create .dockerignore

> Show build and run commands
```

### Example 27: GitHub Actions CI/CD

**Goal:** Set up continuous integration.

```
> Create .github/workflows/ci.yml:
  - Trigger on push and PR
  - Multiple Node versions (16, 18, 20)
  - Install dependencies
  - Run linter
  - Run tests
  - Upload coverage
  - Build project

> Create .github/workflows/deploy.yml:
  - Trigger on release
  - Build Docker image
  - Push to registry
  - Deploy to production

> Explain the workflow
```

## Full Project Tutorials

### Tutorial 1: Todo App (30 minutes)

**Complete full-stack todo application.**

```bash
# Create project and GitHub repository
mkdir todo-app
cd todo-app
git init
gh repo create todo-app --private --source=. --remote=origin

# Launch Copilot CLI
copilot
```

**Step 1: Initialize Project**
```
> /plan Create a full-stack todo application:
  
  Backend (Express + MongoDB):
  - Todo model (title, description, completed, createdAt)
  - REST API endpoints
  - MongoDB connection
  - Input validation
  - Error handling
  
  Frontend (React):
  - Todo list display
  - Add todo form
  - Toggle complete
  - Delete todo
  - Filter (all/active/completed)
  
  Use:
  - Node.js + Express backend
  - React frontend
  - MongoDB database
  - Axios for API calls

[Review plan with Ctrl+Y]

> Start implementation
```

**Step 2: Test as You Build**
```
> After backend is done, test with curl

> After frontend is done, run npm start

> Test all CRUD operations

> Fix any issues
```

**Step 3: Polish**
```
> Add loading states

> Add error handling

> Improve UI styling

> Add README.md

> Create .env.example
```

### Tutorial 2: Blog Platform (60 minutes)

**Multi-user blog with authentication.**

```
> /plan Create a blog platform:
  
  Features:
  - User authentication (JWT)
  - Create/edit/delete posts
  - Comments on posts
  - Like posts
  - User profiles
  - Search functionality
  - Rich text editor
  
  Tech Stack:
  - Express.js backend
  - React frontend
  - PostgreSQL database
  - Sequelize ORM
  - JWT auth
  - bcrypt for passwords
  
  API Endpoints:
  - Auth: register, login, logout
  - Posts: CRUD, search, like
  - Comments: CRUD
  - Users: profile, posts by user

[Review and edit plan]

> Shift+Tab (exit plan mode)

> Start with Phase 1: Setup and Auth
```

Follow the plan step by step, testing after each phase.

### Tutorial 3: Real-Time Chat (45 minutes)

**WebSocket-based chat application.**

```
> /plan Create a real-time chat application:
  
  Backend:
  - Express + Socket.io server
  - Message storage (in-memory)
  - User management
  - Room/channel support
  - Typing indicators
  - Online status
  
  Frontend:
  - React + Socket.io client
  - Message list
  - Send messages
  - Join/leave rooms
  - User list
  - Typing indicators
  
  Features:
  - Real-time messaging
  - Multiple chat rooms
  - Private messages
  - Emoji support
  - Message timestamps

> Implement step by step
```

## Practice Exercises

### Exercise 1: Weather App
```
> Create a weather app that:
  - Takes city name as input
  - Fetches from OpenWeatherMap API
  - Displays current weather
  - Shows 5-day forecast
  - Handles errors gracefully
```

### Exercise 2: URL Shortener
```
> Build a URL shortener service:
  - Generate short codes
  - Store URL mappings
  - Redirect short URLs
  - Track click counts
  - Expire old URLs
```

### Exercise 3: File Upload Service
```
> Create file upload service:
  - Express + Multer
  - Upload images
  - Resize images
  - Store in /uploads
  - Serve uploaded files
  - Validate file types
```

### Exercise 4: Quiz Application
```
> Build a quiz app:
  - Multiple choice questions
  - Timer per question
  - Score tracking
  - High scores
  - Different categories
  - Results summary
```

### Exercise 5: Expense Tracker
```
> Create expense tracker:
  - Add expenses (amount, category, date)
  - View expenses
  - Filter by date/category
  - Calculate totals
  - Charts and statistics
  - Export to CSV
```

## Tips for Learning

### Start Small
Begin with simple examples and gradually increase complexity.

### Experiment
Try variations of examples to understand how things work.

### Read Generated Code
Don't just approve - understand what the AI creates.

### Ask Questions
```
> Why did you use this approach?
> What's an alternative way to do this?
> Explain this code line by line
```

### Build Your Own
Use examples as starting points for your own projects.

### Practice Regularly
```
Daily: Simple scripts and utilities
Weekly: Small applications
Monthly: Complete projects
```

## Next Steps

After completing these examples:

1. **Build your own projects** using what you've learned
2. **Explore advanced topics** in other guide sections
3. **Contribute examples** to help others learn
4. **Share your experience** in GitHub Discussions

## Quick Reference

```bash
# Start tutorial
cd tutorial-folder
copilot

# Create with plan
> /plan <description>
[Review, then implement]

# Test as you go
> Run tests
> Start server
> Check results

# Debug issues
> What's wrong with this?
> Fix the error
> Explain this bug

# Polish
> Add error handling
> Improve styling
> Add documentation
```

---

**Previous:** [Troubleshooting](11-troubleshooting.md)  
**Back to:** [Main Guide](README.md)
