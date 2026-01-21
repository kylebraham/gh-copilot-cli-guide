# AGENTS.MD File Guide

A comprehensive guide to using the `AGENTS.md` file for configuring AI agent behavior in GitHub Copilot CLI.

## Table of Contents

1. [What is AGENTS.md?](#what-is-agentsmd)
2. [Purpose and Use Cases](#purpose-and-use-cases)
3. [File Location and Discovery](#file-location-and-discovery)
4. [Structure and Format](#structure-and-format)
5. [Content Guidelines](#content-guidelines)
6. [Differences from Other Instruction Files](#differences-from-other-instruction-files)
7. [Best Practices](#best-practices)
8. [Common Patterns](#common-patterns)
9. [Examples](#examples)
10. [Advanced Techniques](#advanced-techniques)
11. [Troubleshooting](#troubleshooting)

## What is AGENTS.md?

`AGENTS.md` is a special markdown file that configures how GitHub Copilot CLI's AI agent behaves in your project. It serves as a **project-specific instruction manual** that tells the AI:

- How your project is structured
- What conventions to follow
- What constraints to respect
- How to approach different types of tasks
- Domain-specific knowledge about your codebase

### Key Characteristics

- **Project-scoped**: Instructions apply to the specific project/directory
- **Agent-focused**: Configures AI agent behavior and decision-making
- **Persistent**: Loaded automatically when working in the directory
- **Version-controlled**: Checked into your repository for team consistency
- **Markdown format**: Human-readable and easy to maintain

## Purpose and Use Cases

### Primary Purposes

1. **Codify Project Standards**
   - Enforce coding style and conventions
   - Define architectural patterns
   - Specify naming conventions
   - Document technology stack decisions

2. **Provide Domain Context**
   - Explain business logic
   - Document domain models
   - Describe system architecture
   - Define terminology

3. **Guide AI Behavior**
   - Set constraints and boundaries
   - Prioritize certain approaches
   - Avoid anti-patterns
   - Define success criteria

4. **Improve Team Consistency**
   - Ensure all developers (and AI) follow same patterns
   - Document tribal knowledge
   - Reduce onboarding time
   - Maintain code quality

### Common Use Cases

```markdown
✅ Defining project-wide coding standards
✅ Documenting architecture decisions
✅ Specifying framework-specific patterns
✅ Setting security requirements
✅ Defining testing strategies
✅ Explaining complex business logic
✅ Configuring deployment procedures
✅ Setting performance requirements
```

## File Location and Discovery

### Where to Place AGENTS.md

Copilot CLI searches for `AGENTS.md` in these locations (in order):

1. **Current working directory** (highest priority)
   ```
   ~/projects/my-app/AGENTS.md
   ```

2. **Git repository root**
   ```
   ~/projects/my-app/.git/
   ~/projects/my-app/AGENTS.md
   ```

### Multiple AGENTS.md Files

You can have multiple `AGENTS.md` files for different contexts:

```
my-monorepo/
├── AGENTS.md                 # Root-level (general)
├── backend/
│   └── AGENTS.md            # Backend-specific
├── frontend/
│   └── AGENTS.md            # Frontend-specific
└── infrastructure/
    └── AGENTS.md            # Infrastructure-specific
```

**Behavior**: The CLI uses the `AGENTS.md` closest to your current working directory.

### Verification

Check if your `AGENTS.md` is being loaded:

```bash
# Launch CLI in your project
cd ~/projects/my-app
copilot

# Ask the AI
> What instructions are you following?

> Show me the project standards

> What coding style should I use?
```

## Structure and Format

### Basic Template

```markdown
# Project Name - Agent Instructions

## Overview
Brief description of the project and its purpose.

## Technology Stack
- Framework: [e.g., React, Express, Django]
- Language: [e.g., TypeScript, Python, Go]
- Database: [e.g., PostgreSQL, MongoDB]
- Infrastructure: [e.g., AWS, Docker, Kubernetes]

## Architecture
Description of system architecture and design patterns.

## Coding Standards
### Style Guide
- Indentation: [2 spaces, 4 spaces, tabs]
- Quotes: [single, double]
- Line length: [80, 100, 120 characters]

### Naming Conventions
- Files: [kebab-case, PascalCase, snake_case]
- Variables: [camelCase, snake_case]
- Constants: [UPPER_SNAKE_CASE]
- Classes: [PascalCase]

### Code Organization
- [Directory structure]
- [File organization]
- [Module patterns]

## Development Workflow
- Branch strategy
- Commit message format
- PR requirements
- Code review process

## Testing Strategy
- Unit tests: [requirements]
- Integration tests: [requirements]
- E2E tests: [requirements]
- Coverage: [minimum percentage]

## Security Requirements
- Authentication approach
- Authorization patterns
- Data validation rules
- Security constraints

## Performance Requirements
- Response time targets
- Resource constraints
- Optimization priorities

## Deployment
- Environment setup
- Build process
- Deployment procedure
- Rollback strategy

## Domain-Specific Rules
- Business logic constraints
- Data model relationships
- API contracts
- Integration patterns
```

### Recommended Sections

#### Essential Sections
- **Overview**: Project context
- **Technology Stack**: Tools and frameworks
- **Coding Standards**: Style and conventions
- **Architecture**: System design

#### Recommended Sections
- **Development Workflow**: Git, commits, PRs
- **Testing Strategy**: Test requirements
- **Security Requirements**: Security patterns
- **Domain Rules**: Business logic

#### Optional Sections
- **Performance Requirements**: Optimization goals
- **Deployment**: DevOps procedures
- **Troubleshooting**: Common issues
- **External Resources**: Links to docs

## Content Guidelines

### What to Include

#### ✅ DO Include

**1. Project-Specific Information**
```markdown
## Our API Design Principles
- RESTful endpoints follow resource naming
- Use plural nouns: /users, /orders, /products
- Version via URL: /api/v1/users
- Pagination: ?page=1&limit=20
- Filtering: ?status=active&role=admin
```

**2. Technology-Specific Patterns**
```markdown
## React Component Guidelines
- Use functional components with hooks
- Prefer composition over inheritance
- Props: destructure in function signature
- State: useState for local, Context for shared
- Side effects: useEffect with dependency array
```

**3. Code Quality Standards**
```markdown
## Code Review Checklist
- [ ] No console.log in production code
- [ ] All functions have JSDoc comments
- [ ] Error handling implemented
- [ ] Tests written and passing
- [ ] No magic numbers or strings
- [ ] TypeScript strict mode compliant
```

**4. Business Logic Rules**
```markdown
## Order Processing Rules
1. Orders must have valid payment before processing
2. Inventory is reserved during checkout
3. Failed payments trigger automatic refund
4. Order status: pending → processing → completed → delivered
5. Cancellation only allowed in pending/processing states
```

**5. Security Constraints**
```markdown
## Security Requirements
- All user input must be validated
- Use parameterized queries (no string concatenation)
- Passwords hashed with bcrypt (rounds: 12)
- JWT tokens expire in 24 hours
- Rate limiting: 100 requests/minute per IP
- HTTPS only in production
```

#### ❌ DON'T Include

**1. Secrets or Credentials**
```markdown
❌ API_KEY=sk-abc123456789...
❌ DATABASE_PASSWORD=MyPassword123
❌ JWT_SECRET=super-secret-key
```

**2. Overly Generic Advice**
```markdown
❌ "Write clean code"
❌ "Follow best practices"
❌ "Make it work well"
```

**3. Implementation Details of Every Function**
```markdown
❌ Don't document every function
✅ Document patterns and principles
```

**4. Duplicate Documentation**
```markdown
❌ Copy-pasting from README.md
✅ Reference README and add agent-specific guidance
```

## Differences from Other Instruction Files

### AGENTS.md vs Other Files

| Aspect | AGENTS.md | .github/copilot-instructions.md | CLAUDE.md / GEMINI.md |
|--------|-----------|----------------------------------|------------------------|
| **Scope** | Project-specific agent behavior | GitHub/project integration | Model-specific instructions |
| **Focus** | Architecture, patterns, rules | Copilot-specific workflows | Model capabilities/preferences |
| **Audience** | AI agent + team | Copilot CLI | Specific AI model |
| **Location** | Project root or git root | .github/ directory | Project root |
| **Priority** | Medium | High (more specific) | Low (model-specific) |
| **Version Control** | Yes, always | Yes, always | Yes, always |

### Instruction File Hierarchy

```
Priority (Highest to Lowest):
1. .github/instructions/**/*.instructions.md (most specific)
2. .github/copilot-instructions.md (Copilot-specific)
3. AGENTS.md (project agent config)
4. CLAUDE.md / GEMINI.md (model-specific)
5. ~/.copilot/copilot-instructions.md (user global)
```

### When to Use Each File

#### Use `AGENTS.md` for:
```markdown
✅ Project architecture and patterns
✅ Coding standards and conventions
✅ Business logic and domain rules
✅ Technology stack decisions
✅ Development workflow
✅ Testing and quality requirements
```

#### Use `.github/copilot-instructions.md` for:
```markdown
✅ Copilot CLI-specific workflows
✅ GitHub integration preferences
✅ PR and issue templates
✅ CI/CD interaction patterns
✅ Repository-specific automation
```

#### Use `CLAUDE.md` or `GEMINI.md` for:
```markdown
✅ Model-specific optimizations
✅ Token usage preferences
✅ Output format preferences
✅ Model capability utilization
✅ Fallback behaviors
```

### Example Comparison

**AGENTS.md** (Project patterns):
```markdown
## API Endpoint Structure
All API endpoints follow this pattern:
- GET /api/v1/resources - List all
- GET /api/v1/resources/:id - Get one
- POST /api/v1/resources - Create
- PUT /api/v1/resources/:id - Update
- DELETE /api/v1/resources/:id - Delete

Response format:
{
  "success": true,
  "data": {...},
  "meta": { "page": 1, "total": 100 }
}
```

**.github/copilot-instructions.md** (Copilot workflow):
```markdown
## Working with PRs
When creating PRs via /delegate:
- Use conventional commit format
- Reference issue numbers
- Include test results in PR body
- Add "ready for review" label
- Request review from @team-backend
```

**CLAUDE.md** (Model-specific):
```markdown
## Claude-Specific Preferences
- Prefer detailed explanations with examples
- Use thinking tags for complex logic
- Break down large changes into steps
- Provide alternative approaches when relevant
```

## Best Practices

### 1. Be Specific and Actionable

❌ **Vague:**
```markdown
## Code Quality
Write good code and follow best practices.
```

✅ **Specific:**
```markdown
## Code Quality Standards
- Functions max 50 lines
- Cyclomatic complexity < 10
- All public APIs have JSDoc
- Error handling in all async functions
- Input validation on all endpoints
```

### 2. Use Examples

❌ **Abstract:**
```markdown
Use proper naming conventions.
```

✅ **With Examples:**
```markdown
## Naming Conventions
- Components: `UserProfile.tsx`, `OrderList.tsx` (PascalCase)
- Hooks: `useAuth.ts`, `useFetchData.ts` (camelCase, "use" prefix)
- Utils: `formatDate.ts`, `validateEmail.ts` (camelCase)
- Constants: `API_BASE_URL`, `MAX_RETRIES` (UPPER_SNAKE_CASE)
- Types: `UserData`, `OrderStatus` (PascalCase)

Examples:
```typescript
// ✅ Good
const MAX_LOGIN_ATTEMPTS = 3;
function calculateTotal(items: OrderItem[]): number {...}

// ❌ Bad
const max = 3;
function calc(x: any): any {...}
```
```

### 3. Organize Hierarchically

Structure from general to specific:

```markdown
# MyApp Agent Instructions

## 1. Project Overview (General context)
## 2. Architecture (System-level)
## 3. Technology Stack (Tools)
## 4. Coding Standards (Language-level)
  ### 4.1 TypeScript Standards
  ### 4.2 React Patterns
  ### 4.3 Testing
## 5. Domain Rules (Business-level)
## 6. Workflows (Process-level)
```

### 4. Keep It Current

```markdown
## Maintenance
Last Updated: 2026-01-21
Maintainer: @team-lead

## Changelog
- 2026-01-21: Added API versioning guidelines
- 2026-01-15: Updated testing requirements
- 2026-01-10: Initial version
```

### 5. Make It Scannable

Use formatting for quick reference:

```markdown
## Quick Reference

### File Structure
```
src/
├── api/         # API endpoints
├── components/  # React components
├── hooks/       # Custom hooks
├── utils/       # Helper functions
└── types/       # TypeScript types
```

### Common Commands
- `npm run dev` - Start dev server
- `npm test` - Run tests
- `npm run build` - Production build
- `npm run lint` - Check code style
```

### 6. Include Context

Explain the "why" behind rules:

```markdown
## Database Queries

### Use Parameterized Queries Only
❌ Don't use string concatenation:
```javascript
// NEVER do this - SQL injection risk
const query = `SELECT * FROM users WHERE id = ${userId}`;
```

✅ Use parameterized queries:
```javascript
// Safe from SQL injection
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [userId]);
```

**Why**: Prevents SQL injection attacks. We had a security incident
in Q4 2025 due to string concatenation in queries.
```

### 7. Link to External Resources

```markdown
## Additional Resources

- [API Documentation](./docs/API.md)
- [Architecture Decision Records](./docs/ADRs/)
- [Contributing Guide](./CONTRIBUTING.md)
- [Style Guide](https://github.com/company/style-guide)
- [Security Policy](./SECURITY.md)
```

### 8. Use Checklists

```markdown
## Before Merging PR

- [ ] All tests passing
- [ ] Code coverage ≥ 80%
- [ ] No TypeScript errors
- [ ] ESLint passes
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] PR description complete
- [ ] At least 2 approvals
```

## Common Patterns

### Pattern 1: Monorepo Structure

```markdown
# Monorepo Agent Instructions

## Repository Structure
This is a monorepo containing multiple packages:
- `packages/api` - Backend API (Node.js + Express)
- `packages/web` - Web frontend (React + TypeScript)
- `packages/mobile` - Mobile app (React Native)
- `packages/shared` - Shared utilities and types

## Working with Packages
When making changes:
1. Identify which package(s) are affected
2. Run tests for affected packages only
3. Update shared types if needed
4. Ensure version compatibility

## Cross-Package Dependencies
- `shared` is used by all other packages
- Update `shared` version in package.json when changes are made
- Run `npm run bootstrap` after dependency changes
```

### Pattern 2: Microservices

```markdown
# Microservices Agent Instructions

## Service Architecture
We use microservices architecture with these services:
- `auth-service` - Authentication & authorization
- `user-service` - User management
- `order-service` - Order processing
- `payment-service` - Payment handling
- `notification-service` - Email/SMS notifications

## Inter-Service Communication
- Sync: REST APIs with JSON
- Async: RabbitMQ message broker
- Service discovery: Consul
- API Gateway: Kong

## Service Guidelines
Each service must:
- Have its own database (no shared databases)
- Expose health check endpoint: GET /health
- Implement circuit breakers for external calls
- Log all requests with correlation IDs
- Handle graceful shutdown
```

### Pattern 3: Strict Type Safety

```markdown
# TypeScript Strict Mode Project

## TypeScript Configuration
We use TypeScript in strict mode with additional rules:
- `strict: true`
- `noImplicitAny: true`
- `strictNullChecks: true`
- `noUncheckedIndexedAccess: true`

## Type Safety Rules
1. **No `any` types** - Use `unknown` if type is truly unknown
2. **Explicit return types** - All functions must declare return type
3. **No type assertions** - Use type guards instead
4. **Null safety** - Always check for null/undefined

## Examples
```typescript
// ✅ Good
function getUser(id: string): User | null {
  const user = users.find(u => u.id === id);
  return user ?? null;
}

// ❌ Bad
function getUser(id: any): any {
  return users.find(u => u.id === id);
}
```
```

### Pattern 4: Test-Driven Development

```markdown
# TDD Project - Test First

## Testing Philosophy
We practice Test-Driven Development:
1. Write test first (RED)
2. Write minimal code to pass (GREEN)
3. Refactor (REFACTOR)

## Test Requirements
- **Unit tests**: Every function/class
- **Integration tests**: Every API endpoint
- **E2E tests**: Critical user flows
- **Coverage**: Minimum 90%

## Test Structure
```typescript
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user with valid data', () => {...});
    it('should reject invalid email', () => {...});
    it('should reject duplicate email', () => {...});
    it('should hash password', () => {...});
  });
});
```

## When Writing New Features
1. Start with test file: `feature.test.ts`
2. Write failing tests for requirements
3. Implement feature to pass tests
4. Refactor for code quality
5. Verify all tests still pass
```

### Pattern 5: API-First Design

```markdown
# API-First Development

## Approach
We design APIs before implementation:
1. Write OpenAPI spec first
2. Review API design with team
3. Generate types from spec
4. Implement endpoints
5. Generate documentation

## API Design Principles
- **RESTful**: Use standard HTTP methods
- **Versioned**: /api/v1, /api/v2
- **Consistent**: Same patterns across endpoints
- **Documented**: OpenAPI 3.0 spec
- **Validated**: JSON Schema validation

## Process
```bash
# 1. Design API in OpenAPI
vim api-spec.yaml

# 2. Generate types
npm run generate:types

# 3. Generate docs
npm run generate:docs

# 4. Implement
# Create endpoint matching spec
```
```

## Examples

### Example 1: Full-Stack Web Application

```markdown
# E-Commerce Platform - Agent Instructions

## Project Overview
Full-stack e-commerce platform for online retail.
- Users can browse products, add to cart, checkout
- Admins can manage inventory, orders, customers
- RESTful API backend, React frontend

## Technology Stack
- **Frontend**: React 18 + TypeScript + Vite
- **Backend**: Node.js + Express + TypeScript
- **Database**: PostgreSQL 15
- **ORM**: Prisma
- **Authentication**: JWT
- **Payment**: Stripe
- **Hosting**: AWS (ECS + RDS)

## Architecture
```
┌─────────────┐     ┌──────────────┐     ┌────────────┐
│   React     │────▶│   Express    │────▶│ PostgreSQL │
│   Frontend  │◀────│   API        │◀────│  Database  │
└─────────────┘     └──────────────┘     └────────────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    Stripe    │
                    │    API       │
                    └──────────────┘
```

## Coding Standards

### TypeScript
- Strict mode enabled
- Explicit return types on all functions
- No `any` types - use `unknown` if necessary
- Interfaces for data structures, types for unions

### React Components
```typescript
// ✅ Preferred pattern
interface UserCardProps {
  user: User;
  onEdit: (id: string) => void;
}

export function UserCard({ user, onEdit }: UserCardProps): JSX.Element {
  return <div>{user.name}</div>;
}
```

### API Endpoints
```typescript
// ✅ Standard structure
router.post('/api/v1/products', 
  authenticate,
  validateBody(productSchema),
  async (req: Request, res: Response) => {
    try {
      const product = await productService.create(req.body);
      res.status(201).json({ success: true, data: product });
    } catch (error) {
      res.status(400).json({ success: false, error: error.message });
    }
  }
);
```

## Database Guidelines

### Prisma Schema
- Use meaningful model names (User, Product, Order)
- Add indexes for frequently queried fields
- Use enums for status fields
- Include timestamps: createdAt, updatedAt

### Queries
```typescript
// ✅ Include relations explicitly
const order = await prisma.order.findUnique({
  where: { id },
  include: {
    items: { include: { product: true } },
    user: { select: { id: true, email: true, name: true } },
  },
});
```

## Security Requirements

1. **Authentication**
   - JWT tokens with 24-hour expiry
   - Refresh tokens with 7-day expiry
   - Password: bcrypt with 12 rounds

2. **Authorization**
   - Role-based: USER, ADMIN
   - Middleware checks on protected routes
   - Row-level security for user data

3. **Input Validation**
   - Zod schemas for all inputs
   - Sanitize HTML content
   - Parameterized database queries

4. **Rate Limiting**
   - 100 requests/minute per IP
   - 20 requests/minute for auth endpoints

## Testing Strategy

### Unit Tests (Jest)
- All utility functions
- All service layer functions
- All middleware
- Target: 90% coverage

### Integration Tests (Supertest)
- All API endpoints
- Authentication flows
- Payment processing
- Order workflows

### E2E Tests (Playwright)
- User registration and login
- Product browse and search
- Shopping cart operations
- Checkout and payment
- Order management

## Development Workflow

### Branch Strategy
- `main` - Production
- `develop` - Integration
- `feature/*` - New features
- `bugfix/*` - Bug fixes
- `hotfix/*` - Production fixes

### Commit Messages
```
<type>(<scope>): <subject>

Types: feat, fix, docs, style, refactor, test, chore
Scope: api, frontend, database, auth, etc.

Examples:
feat(api): add product search endpoint
fix(frontend): resolve cart total calculation bug
docs(readme): update installation instructions
```

### Pull Request Requirements
- [ ] All tests passing (npm test)
- [ ] Code coverage maintained (≥90%)
- [ ] ESLint passes (npm run lint)
- [ ] TypeScript builds (npm run build)
- [ ] Documentation updated
- [ ] At least 2 approvals
- [ ] Jira ticket linked

## Domain Rules

### Product Management
- SKU must be unique
- Price cannot be negative
- Stock quantity tracked
- Soft delete (set deleted_at, don't remove)

### Order Processing
1. Order created with status: PENDING
2. Payment processed → PROCESSING
3. Items packed → SHIPPED
4. Delivered → COMPLETED
5. Issues → CANCELLED or REFUNDED

### Inventory
- Decrease stock on order creation
- Reserve stock during checkout (15 min hold)
- Release on payment failure
- Alert when stock < 10 units

## Performance Requirements
- API response time: < 200ms (p95)
- Page load time: < 2 seconds
- Database queries: < 100ms
- Bundle size: < 500KB (frontend)

## Error Handling

### API Errors
```typescript
class AppError extends Error {
  constructor(
    public statusCode: number,
    public message: string,
    public isOperational = true
  ) {
    super(message);
  }
}

// Usage
throw new AppError(400, 'Invalid product ID');
```

### Frontend Errors
```typescript
// Use error boundaries for React
// Show user-friendly messages
// Log errors to monitoring service
```

## Deployment

### Environments
- **Development**: Local (npm run dev)
- **Staging**: AWS ECS (auto-deploy from develop)
- **Production**: AWS ECS (manual deploy from main)

### Environment Variables
```bash
# Required in all environments
DATABASE_URL=
JWT_SECRET=
STRIPE_SECRET_KEY=
AWS_REGION=
```

### Build Process
```bash
# Frontend
cd frontend
npm run build

# Backend
cd backend
npm run build

# Docker
docker build -t ecommerce-api:latest .
```

## Monitoring
- **Logging**: Winston → CloudWatch
- **Metrics**: Prometheus + Grafana
- **Errors**: Sentry
- **Uptime**: StatusPage

## Additional Resources
- [API Documentation](./docs/api.md)
- [Database Schema](./prisma/schema.prisma)
- [Architecture Decisions](./docs/adr/)
- [Runbook](./docs/runbook.md)

---
Last Updated: 2026-01-21
Maintainer: @backend-team
```

### Example 2: Python Data Science Project

```markdown
# ML Pipeline - Agent Instructions

## Project Overview
Machine learning pipeline for customer churn prediction.
Batch processing, model training, and API serving.

## Technology Stack
- **Language**: Python 3.11
- **ML Framework**: scikit-learn, XGBoost
- **Data**: Pandas, NumPy
- **API**: FastAPI
- **Orchestration**: Apache Airflow
- **Storage**: PostgreSQL + S3

## Code Standards

### Python Style
- Follow PEP 8
- Type hints on all functions
- Docstrings (Google style)
- Black formatter (line length: 100)
- isort for imports

```python
# ✅ Good example
from typing import List, Dict
import pandas as pd

def predict_churn(
    customer_ids: List[str],
    features: pd.DataFrame
) -> Dict[str, float]:
    """Predict customer churn probability.
    
    Args:
        customer_ids: List of customer IDs
        features: Feature dataframe
    
    Returns:
        Dictionary mapping customer ID to churn probability
        
    Raises:
        ValueError: If customer_ids length doesn't match features
    """
    if len(customer_ids) != len(features):
        raise ValueError("Mismatched lengths")
    
    predictions = model.predict_proba(features)[:, 1]
    return dict(zip(customer_ids, predictions))
```

## Data Processing

### Feature Engineering
```python
# Standard pipeline
1. Load raw data from S3
2. Clean missing values (impute or drop)
3. Encode categorical features (one-hot)
4. Scale numerical features (StandardScaler)
5. Feature selection (keep top 20)
6. Save processed data to S3
```

### Data Validation
- Check for missing values > 10% → alert
- Check for data drift → retrain model
- Validate schema with Pandera
- Monitor feature distributions

## Model Development

### Training Pipeline
```python
1. Load training data (80% split)
2. Cross-validation (5-fold)
3. Hyperparameter tuning (GridSearchCV)
4. Train final model on full training set
5. Evaluate on test set (20% split)
6. If AUC-ROC > 0.85: deploy
7. If AUC-ROC < 0.85: iterate
```

### Model Registry
- Save models to S3: `s3://models/churn/{version}/`
- Track metrics in MLflow
- Version format: YYYY-MM-DD-v{n}
- Keep last 5 versions

### Model Evaluation
```python
# Required metrics
- AUC-ROC: Target ≥ 0.85
- Precision: Target ≥ 0.75
- Recall: Target ≥ 0.80
- F1-Score: Report only

# Confusion matrix required in reports
```

## API Serving

### Endpoint Structure
```python
from fastapi import FastAPI
from pydantic import BaseModel

class PredictionRequest(BaseModel):
    customer_id: str
    age: int
    tenure_months: int
    monthly_spend: float
    # ... other features

@app.post("/api/v1/predict")
async def predict(request: PredictionRequest) -> Dict:
    features = extract_features(request)
    proba = model.predict_proba([features])[0][1]
    return {
        "customer_id": request.customer_id,
        "churn_probability": float(proba),
        "risk_level": "high" if proba > 0.7 else "medium" if proba > 0.4 else "low"
    }
```

## Testing

### Unit Tests (pytest)
```python
def test_feature_engineering():
    # Given
    raw_data = pd.DataFrame(...)
    
    # When
    features = engineer_features(raw_data)
    
    # Then
    assert features.shape[1] == 20
    assert features.isna().sum().sum() == 0
```

### Model Tests
```python
def test_model_performance():
    # Load test dataset
    X_test, y_test = load_test_data()
    
    # Predict
    y_pred = model.predict(X_test)
    
    # Assert metrics
    assert roc_auc_score(y_test, y_pred) >= 0.85
```

## Airflow DAGs

### Structure
```python
dag = DAG(
    'churn_prediction_pipeline',
    schedule_interval='@daily',
    default_args={
        'retries': 3,
        'retry_delay': timedelta(minutes=5),
    }
)

extract_data >> transform_data >> train_model >> evaluate >> deploy
```

## Monitoring

### Model Monitoring
- Track prediction distribution daily
- Alert if drift > 10%
- Retrainautomatically if AUC drops below 0.80

### Data Quality
- Check null percentage
- Check value ranges
- Check cardinality changes
- Validate schema

---
Last Updated: 2026-01-21
Maintainer: @ml-team
```

## Advanced Techniques

### 1. Conditional Instructions

```markdown
## Context-Aware Guidelines

### When Working on Frontend
- Use React functional components
- Implement accessibility (ARIA labels)
- Ensure responsive design
- Test on mobile viewports

### When Working on Backend
- Use async/await for I/O operations
- Implement request validation
- Add rate limiting
- Log all errors with context

### When Working on Tests
- Follow AAA pattern (Arrange, Act, Assert)
- Use descriptive test names
- Mock external dependencies
- Aim for one assertion per test
```

### 2. Versioned Instructions

```markdown
## API Version Guidelines

### v1 (Legacy - Maintenance Only)
- No new features
- Bug fixes only
- Deprecation warnings in responses

### v2 (Current)
- New features allowed
- Follow REST principles
- Use JSON:API spec

### v3 (Development)
- GraphQL endpoints
- Real-time subscriptions
- Breaking changes allowed
```

### 3. Role-Based Instructions

```markdown
## Role-Specific Guidelines

### For Junior Developers
- Start with tickets labeled "good-first-issue"
- Request code review before pushing
- Ask questions in #help channel
- Pair program for complex tasks

### For Senior Developers
- Review PRs within 24 hours
- Mentor junior developers
- Lead architecture discussions
- Own production incidents

### For Contractors
- Work only on assigned tickets
- No infrastructure changes
- Get approval for new dependencies
- Document all decisions
```

### 4. Dynamic References

```markdown
## External Documentation

See [API Docs](./docs/api.md) for endpoint details.
See [ADR-001](./docs/adr/001-database-choice.md) for database selection rationale.
See [Runbook](./docs/runbook.md) for production issues.

### Quick Links
- Staging: https://staging.myapp.com
- Production: https://myapp.com
- Monitoring: https://grafana.myapp.com
- Logs: https://logs.myapp.com
```

### 5. Decision Trees

```markdown
## When to Use Each Database

```
Need real-time updates?
├─ Yes → Use WebSockets + Redis
└─ No
   └─ Complex queries?
      ├─ Yes → Use PostgreSQL
      └─ No → Use MongoDB
```

## Caching Strategy Decision

```
Data changes frequently?
├─ Yes (>1/min)
│  └─ Cache TTL: 30 seconds
└─ No
   └─ Data critical?
      ├─ Yes → Cache TTL: 5 minutes
      └─ No → Cache TTL: 1 hour
```
```

## Troubleshooting

### Instructions Not Being Followed

**Problem**: AI doesn't seem to follow AGENTS.md instructions.

**Solutions**:

1. **Verify file location**
```bash
# Check if file exists in correct location
ls -la AGENTS.md

# Check git root
git rev-parse --show-toplevel
ls -la $(git rev-parse --show-toplevel)/AGENTS.md
```

2. **Verify file is in Git**
```bash
git ls-files | grep AGENTS.md
```

3. **Test by asking**
```
> What instructions are you following?
> What coding style should I use?
> Tell me about this project's standards
```

4. **Check for conflicts**
```markdown
# If you have multiple instruction files,
# check that they don't contradict
# .github/copilot-instructions.md might override AGENTS.md
```

### Instructions Too Long

**Problem**: AGENTS.md is very large and might not fit in context.

**Solutions**:

1. **Split by concern**
```
AGENTS.md              # Core instructions
AGENTS-FRONTEND.md     # Frontend-specific
AGENTS-BACKEND.md      # Backend-specific
AGENTS-TESTING.md      # Testing-specific
```

2. **Use links to external docs**
```markdown
## Detailed API Patterns
See [API Guidelines](./docs/api-guidelines.md) for comprehensive
endpoint design patterns.

## Security Details
See [Security Policy](./SECURITY.md) for complete security requirements.
```

3. **Prioritize most important rules**
```markdown
## Critical Rules (Always Follow)
[Most important items]

## Standard Guidelines (Follow When Possible)
[Less critical items]

## Nice to Have
[Optional optimizations]
```

### Conflicting Instructions

**Problem**: Different instruction files contradict each other.

**Solutions**:

1. **Understand priority**
```
.github/instructions/**/*.instructions.md (highest)
.github/copilot-instructions.md
AGENTS.md
CLAUDE.md / GEMINI.md
~/.copilot/copilot-instructions.md (lowest)
```

2. **Be explicit about priority in file**
```markdown
# AGENTS.md
## Override Note
These instructions override any conflicting guidance in CLAUDE.md
or personal copilot-instructions.md files.
```

3. **Consolidate when possible**
```markdown
# Move project-specific rules to AGENTS.md
# Move Copilot workflow to .github/copilot-instructions.md
# Remove or minimize model-specific files
```

### AI Ignores Specific Rules

**Problem**: One rule consistently ignored.

**Solutions**:

1. **Make rule more explicit**
```markdown
❌ "Use proper error handling"

✅ "MANDATORY: Every async function must have try-catch block.
    Example:
    async function getData() {
      try {
        const result = await api.fetch();
        return result;
      } catch (error) {
        logger.error('getData failed:', error);
        throw new AppError(500, 'Failed to fetch data');
      }
    }"
```

2. **Add examples**
```markdown
## Rule: No console.log in production

❌ Bad:
console.log('User logged in:', userId);

✅ Good:
logger.info('User logged in', { userId, timestamp: Date.now() });
```

3. **Repeat in multiple places**
```markdown
## Critical Rules
- NO console.log in production code

## Logging Guidelines
- Use Winston logger, not console.log

## Code Review Checklist
- [ ] No console.log statements
```

## Summary

### Key Takeaways

1. **AGENTS.md is project-specific agent configuration**
   - Defines how AI should approach your codebase
   - Version-controlled and team-shared

2. **Focus on patterns, not implementation**
   - Document "how" and "why", not "what"
   - Teach principles, not every function

3. **Be specific and actionable**
   - Use examples
   - Provide code snippets
   - Include checklists

4. **Keep it organized and current**
   - Hierarchical structure
   - Regular updates
   - Clear ownership

5. **Different from other instruction files**
   - AGENTS.md: project patterns and rules
   - .github/copilot-instructions.md: Copilot workflows
   - CLAUDE.md/GEMINI.md: model-specific

### Quick Start Template

```markdown
# [Project Name] - Agent Instructions

## Overview
[Brief description]

## Technology Stack
- [Framework]
- [Language]
- [Database]

## Coding Standards
### Style
- [Indentation]
- [Quotes]
- [Naming]

### Patterns
```[language]
// Example of preferred pattern
```

## Testing
- [Requirements]
- [Coverage target]

## Security
- [Authentication]
- [Validation]

## Deployment
- [Process]

---
Last Updated: [Date]
Maintainer: [Team]
```

## Additional Resources

- [Custom Instructions Guide](08-advanced-features.md#custom-instructions)
- [Best Practices](10-best-practices.md)
- [GitHub Integration](07-github-integration.md)

---

**Next:** Continue to other guide sections  
**Previous:** [Advanced Features](08-advanced-features.md)
