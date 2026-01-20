# Examples and Tutorials

Practical, hands-on examples and tutorials to help you master GitHub Copilot CLI. Each example is runnable and demonstrates real-world use cases.

## Table of Contents

1. [Getting Started Examples](#getting-started-examples)
2. [Web Development](#web-development)
3. [API Development](#api-development)
4. [Database Operations](#database-operations)
5. [Testing](#testing)
6. [Refactoring](#refactoring)
7. [Debugging](#debugging)
8. [Documentation](#documentation)
9. [DevOps and Scripts](#devops-and-scripts)
10. [Full Project Tutorials](#full-project-tutorials)

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
