# Skills System Guide

A comprehensive guide to using Skills in GitHub Copilot CLI for enhanced, reusable capabilities.

## Table of Contents

1. [What are Skills?](#what-are-skills)
2. [Skills vs Instruction Files](#skills-vs-instruction-files)
3. [How Skills Work](#how-skills-work)
4. [Managing Skills](#managing-skills)
5. [Creating Custom Skills](#creating-custom-skills)
6. [Built-in Skills](#built-in-skills)
7. [Skill File Format](#skill-file-format)
8. [Best Practices](#best-practices)
9. [Use Cases](#use-cases)
10. [Examples](#examples)
11. [Troubleshooting](#troubleshooting)

## What are Skills?

**Skills** are modular, reusable capability packages that extend GitHub Copilot CLI's abilities in specific domains or tasks. Think of them as specialized expertise modules that can be activated when needed.

### Key Characteristics

- **Modular**: Each skill is self-contained
- **Reusable**: Use across multiple projects
- **Activatable**: Enable/disable as needed
- **Domain-focused**: Specialized knowledge or capabilities
- **Shareable**: Can be distributed to teams

### Skills vs General Knowledge

```
Without Skill:                    With "Python Expert" Skill:
─────────────────────────────    ─────────────────────────────────
Generic Python knowledge         Deep Python expertise
Basic syntax help                Advanced patterns
Common patterns                  PEP compliance checking
                                Type hint recommendations
                                Performance optimization
                                Security best practices
                                Framework-specific patterns
```

## Skills vs Instruction Files

### Conceptual Differences

| Aspect | Skills | AGENTS.md | .instructions.md |
|--------|--------|-----------|------------------|
| **Purpose** | Add specialized capabilities | Configure project behavior | Set project standards |
| **Scope** | Domain expertise (e.g., Python, Security) | Project-specific patterns | Coding conventions |
| **Activation** | Manually enabled/disabled | Always active in project | Always active |
| **Reusability** | Use across any project | Project-specific | Project-specific |
| **Location** | `~/.copilot/skills/` or project | Project root or .github/ | .github/instructions/ |
| **Shareability** | Easy to share as packages | Git repo only | Git repo only |
| **Focus** | "How to be an expert in X" | "How this project works" | "How we write code" |

### When to Use What

#### Use **Skills** for:
```
✅ Domain expertise (Python expert, React patterns, SQL optimization)
✅ Cross-project capabilities (security auditing, performance analysis)
✅ Specialized knowledge (AWS deployment, Kubernetes config)
✅ Reusable patterns (testing strategies, API design)
✅ Tools and frameworks (Django, FastAPI, Next.js)
```

#### Use **AGENTS.md** for:
```
✅ Project architecture and design
✅ Technology stack decisions
✅ Business logic and domain rules
✅ Project-specific conventions
✅ Team workflow and processes
```

#### Use **Instruction files** for:
```
✅ Code style and formatting
✅ Naming conventions
✅ File organization
✅ Commit message format
✅ PR requirements
```

### Example Scenario

Imagine building a Django e-commerce site:

**Skill: "Django Expert"**
```markdown
- Django ORM best practices
- Class-based vs function-based views
- Django REST Framework patterns
- Middleware implementation
- Signal handling
- Cache strategies
```

**AGENTS.md:**
```markdown
- E-commerce business logic
- Order processing workflow
- Payment integration (Stripe)
- Inventory management rules
- Multi-tenant architecture
```

**Instructions:**
```markdown
- Use Black for formatting
- Imports organized with isort
- Type hints required
- Tests in tests/ directory
- Max line length: 100
```

## How Skills Work

### Skill Loading Process

```
1. Launch Copilot CLI
   ↓
2. Check for available skills
   - Global: ~/.copilot/skills/
   - Project: .copilot/skills/
   ↓
3. Load enabled skills
   ↓
4. Skills add to AI's context
   - Patterns
   - Best practices
   - Specialized knowledge
   ↓
5. AI uses skill knowledge in responses
```

### Skill Activation

Skills can be:
- **Always active** (enabled globally)
- **Project-specific** (enabled per project)
- **On-demand** (activated when needed)

### Context Integration

When a skill is active:
```
AI Context Window:
┌─────────────────────────────────┐
│ Core Copilot Knowledge          │
│ + Your Conversation             │
│ + Project Files (@mentions)     │
│ + AGENTS.md                     │
│ + Instruction Files             │
│ + Active Skills ← Added expertise
└─────────────────────────────────┘
```

## Managing Skills

### Listing Available Skills

```bash
# In Copilot CLI
> /skills list
```

**Expected output:**
```
Available Skills:
─────────────────────────────────────
✅ python-expert         (active)
✅ react-patterns        (active)
⭕ security-audit        (available)
⭕ aws-deployment        (available)
⭕ sql-optimization      (available)
⭕ docker-expert         (available)

To activate: /skills add <name>
To learn more: /skills info <name>
```

### Getting Skill Information

```bash
> /skills info python-expert
```

**Expected output:**
```
Skill: Python Expert
──────────────────────────────────
Description:
  Advanced Python development expertise including
  modern patterns, type safety, and performance.

Capabilities:
  • PEP 8 and PEP 484 compliance
  • Type hint recommendations
  • AsyncIO patterns
  • Performance optimization
  • Security best practices
  • Common pitfalls and solutions

Best used for:
  • Python codebases
  • FastAPI/Django/Flask projects
  • Data science projects
  • CLI tools

Author: GitHub
Version: 1.2.0
Last Updated: 2026-01-15
```

### Adding Skills

```bash
# Add a skill
> /skills add python-expert

# Add multiple skills
> /skills add react-patterns
> /skills add security-audit
```

**Response:**
```
✅ Activated skill: python-expert

This skill provides:
- Advanced Python patterns
- Type safety guidance
- Performance optimization
- Security recommendations

The skill is now active and will influence my responses.
```

### Removing Skills

```bash
# Remove a skill
> /skills remove python-expert

# Remove if you don't need it anymore
> /skills remove docker-expert
```

### Reloading Skills

After modifying skill files:

```bash
> /skills reload
```

This refreshes all active skills from disk.

## Creating Custom Skills

### Skill File Location

Store custom skills in:
```
~/.copilot/skills/          # Global skills (all projects)
<project>/.copilot/skills/  # Project-specific skills
```

### Basic Skill Template

Create a file: `~/.copilot/skills/my-skill.skill.md`

```markdown
# Skill Name

## Metadata
- **ID**: my-skill
- **Version**: 1.0.0
- **Author**: Your Name
- **Category**: [Development, Security, DevOps, Testing, etc.]
- **Tags**: keyword1, keyword2, keyword3

## Description
Brief description of what this skill provides.

## Capabilities
List of specific capabilities this skill adds:
- Capability 1
- Capability 2
- Capability 3

## When to Use
Scenarios where this skill is most valuable:
- Use case 1
- Use case 2

## Instructions

### Core Principles
[Core principles this skill follows]

### Patterns and Practices
[Specific patterns to follow]

### Code Examples

#### Example 1: [Title]
```[language]
// Example code showing the pattern
```

Explanation of why this is the preferred approach.

#### Example 2: [Title]
```[language]
// Another example
```

### Common Pitfalls
Things to avoid:
- ❌ Anti-pattern 1
- ❌ Anti-pattern 2

### Best Practices
- ✅ Best practice 1
- ✅ Best practice 2

## Checklists

### [Task Type] Checklist
- [ ] Item 1
- [ ] Item 2
- [ ] Item 3

## Resources
- [Link to documentation]
- [Link to guides]
- [Link to examples]

## Related Skills
Skills that complement this one:
- skill-name-1
- skill-name-2
```

## Built-in Skills

While specific built-in skills may vary by version, common skill categories include:

### Language Skills
```
python-expert       - Advanced Python development
javascript-expert   - Modern JavaScript patterns
typescript-expert   - TypeScript best practices
go-expert          - Go idioms and patterns
rust-expert        - Rust safety and performance
java-expert        - Java enterprise patterns
```

### Framework Skills
```
react-patterns     - React best practices
vue-patterns       - Vue.js patterns
angular-patterns   - Angular architecture
django-expert      - Django development
fastapi-expert     - FastAPI patterns
express-expert     - Express.js patterns
```

### Domain Skills
```
security-audit     - Security review and fixes
performance-opt    - Performance optimization
testing-expert     - Testing strategies
api-design         - RESTful API design
database-expert    - Database optimization
devops-expert      - DevOps and deployment
```

### Cloud/Infrastructure Skills
```
aws-expert         - AWS services and patterns
azure-expert       - Azure cloud patterns
gcp-expert         - Google Cloud Platform
kubernetes-expert  - K8s deployment
docker-expert      - Container best practices
terraform-expert   - Infrastructure as Code
```

## Skill File Format

### Complete Example: Python Expert Skill

`~/.copilot/skills/python-expert.skill.md`:

```markdown
# Python Expert Skill

## Metadata
- **ID**: python-expert
- **Version**: 1.2.0
- **Author**: GitHub Copilot Team
- **Category**: Development
- **Tags**: python, backend, scripting, data-science

## Description
Advanced Python development expertise covering modern patterns,
type safety, asynchronous programming, and performance optimization.

## Capabilities
- PEP 8 and PEP 484 compliance checking
- Type hint recommendations and validation
- AsyncIO and concurrent programming patterns
- Performance optimization suggestions
- Security vulnerability identification
- Common pitfall prevention
- Framework-specific guidance (Django, FastAPI, Flask)
- Testing best practices (pytest, unittest)

## When to Use
Activate this skill when:
- Working on Python codebases
- Building web APIs with FastAPI/Django/Flask
- Developing data science/ML pipelines
- Creating CLI tools
- Writing Python libraries

## Instructions

### Core Principles

1. **Explicit is better than implicit** (Zen of Python)
2. **Type hints are mandatory for public APIs**
3. **Prefer composition over inheritance**
4. **Use context managers for resource management**
5. **Async for I/O, threads for CPU in GIL-constrained scenarios**

### Type Hints

Always use type hints for function signatures:

```python
# ✅ Correct
def calculate_total(items: list[dict[str, Any]], tax_rate: float) -> Decimal:
    """Calculate total with tax."""
    subtotal = sum(Decimal(str(item['price'])) for item in items)
    return subtotal * (1 + Decimal(str(tax_rate)))

# ❌ Avoid
def calculate_total(items, tax_rate):
    subtotal = sum(item['price'] for item in items)
    return subtotal * (1 + tax_rate)
```

### Modern Python Patterns

#### 1. Data Classes (Python 3.7+)
```python
from dataclasses import dataclass
from datetime import datetime

@dataclass
class User:
    id: int
    username: str
    email: str
    created_at: datetime
    is_active: bool = True
    
    def __post_init__(self):
        if not self.email or '@' not in self.email:
            raise ValueError("Invalid email")
```

#### 2. Context Managers
```python
from contextlib import contextmanager

@contextmanager
def database_connection(db_url: str):
    conn = create_connection(db_url)
    try:
        yield conn
    finally:
        conn.close()

# Usage
with database_connection(DB_URL) as conn:
    result = conn.execute(query)
```

#### 3. AsyncIO
```python
import asyncio
from typing import List

async def fetch_data(url: str) -> dict:
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.json()

async def fetch_all(urls: List[str]) -> List[dict]:
    tasks = [fetch_data(url) for url in urls]
    return await asyncio.gather(*tasks)
```

#### 4. Pathlib over os.path
```python
from pathlib import Path

# ✅ Modern approach
config_file = Path(__file__).parent / 'config' / 'settings.json'
if config_file.exists():
    data = config_file.read_text()

# ❌ Old approach
import os
config_file = os.path.join(os.path.dirname(__file__), 'config', 'settings.json')
if os.path.exists(config_file):
    with open(config_file) as f:
        data = f.read()
```

### Error Handling

```python
# ✅ Specific exceptions
try:
    result = process_data(data)
except ValueError as e:
    logger.error(f"Invalid data format: {e}")
    raise
except KeyError as e:
    logger.error(f"Missing required field: {e}")
    return None
except Exception as e:
    logger.exception("Unexpected error in process_data")
    raise ProcessingError("Failed to process data") from e

# ❌ Bare except
try:
    result = process_data(data)
except:
    pass  # Never do this!
```

### Testing with pytest

```python
import pytest
from decimal import Decimal

class TestCalculator:
    @pytest.fixture
    def calculator(self):
        return Calculator()
    
    def test_add_positive_numbers(self, calculator):
        result = calculator.add(Decimal('10.5'), Decimal('5.5'))
        assert result == Decimal('16.0')
    
    @pytest.mark.parametrize('a,b,expected', [
        (0, 0, 0),
        (5, 5, 10),
        (-5, 5, 0),
    ])
    def test_add_various_inputs(self, calculator, a, b, expected):
        assert calculator.add(a, b) == expected
```

### Common Pitfalls

#### 1. Mutable Default Arguments
```python
# ❌ Bug: mutable default
def add_item(item, items=[]):
    items.append(item)
    return items

# ✅ Correct
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

#### 2. Late Binding Closures
```python
# ❌ Bug: all functions reference same i
functions = [lambda: i for i in range(5)]
# All return 4!

# ✅ Correct
functions = [lambda i=i: i for i in range(5)]
```

#### 3. Comparing with None
```python
# ❌ Incorrect
if value == None:
    pass

# ✅ Correct
if value is None:
    pass
```

### Performance Optimization

1. **Use list comprehensions over loops**
```python
# ✅ Fast
squares = [x**2 for x in range(1000)]

# ❌ Slower
squares = []
for x in range(1000):
    squares.append(x**2)
```

2. **Use generators for large datasets**
```python
# ✅ Memory efficient
def read_large_file(file_path: Path):
    with file_path.open() as f:
        for line in f:
            yield line.strip()

# ❌ Memory intensive
def read_large_file(file_path: Path):
    with file_path.open() as f:
        return [line.strip() for line in f]
```

3. **Use sets for membership testing**
```python
# ✅ O(1) lookup
valid_ids = {1, 2, 3, 4, 5}
if user_id in valid_ids:
    ...

# ❌ O(n) lookup
valid_ids = [1, 2, 3, 4, 5]
if user_id in valid_ids:
    ...
```

### Security Best Practices

```python
# ✅ Parameterized queries
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# ❌ SQL injection risk
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")

# ✅ Secure password hashing
import bcrypt
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt())

# ❌ Never store plain text
password_stored = password  # NEVER!
```

## Checklists

### Code Review Checklist
- [ ] Type hints on all public functions
- [ ] Docstrings follow Google/NumPy format
- [ ] No bare except clauses
- [ ] Context managers for resource cleanup
- [ ] Tests cover edge cases
- [ ] No mutable default arguments
- [ ] Comparisons with None use `is/is not`
- [ ] Security: no SQL injection vulnerabilities
- [ ] Performance: appropriate data structures

### New Feature Checklist
- [ ] Type hints complete
- [ ] Unit tests written
- [ ] Integration tests if applicable
- [ ] Documentation updated
- [ ] Error handling implemented
- [ ] Logging added
- [ ] Security considerations reviewed

## Resources
- [PEP 8 Style Guide](https://pep8.org/)
- [PEP 484 Type Hints](https://www.python.org/dev/peps/pep-0484/)
- [Python Best Practices](https://docs.python-guide.org/)
- [Real Python Tutorials](https://realpython.com/)

## Related Skills
- fastapi-expert - FastAPI framework patterns
- django-expert - Django best practices
- testing-expert - Advanced testing strategies
- security-audit - Security vulnerability detection
```

## Best Practices

### 1. Keep Skills Focused

❌ **Too Broad:**
```markdown
# Programming Expert Skill
Covers: Python, JavaScript, Java, Go, Rust, C++, etc.
```

✅ **Focused:**
```markdown
# Python Expert Skill
Covers: Python 3.10+ patterns and best practices
```

### 2. Include Practical Examples

Every recommendation should have code examples showing:
- ✅ The recommended approach
- ❌ What to avoid
- Why one is better than the other

### 3. Version Your Skills

```markdown
## Metadata
- **Version**: 1.2.0
- **Last Updated**: 2026-01-21

## Changelog
- v1.2.0 (2026-01-21): Added asyncio patterns
- v1.1.0 (2026-01-10): Added type hint examples
- v1.0.0 (2026-01-01): Initial release
```

### 4. Make Skills Discoverable

```markdown
## Tags
python, backend, api, async, typing, testing

## Keywords
asyncio, type hints, pytest, dataclasses, context managers
```

### 5. Link to Resources

Always provide links to:
- Official documentation
- Style guides
- Tutorial resources
- Related tools

### 6. Test Your Skills

Before sharing, verify the skill works:

```bash
1. Add the skill: /skills add my-new-skill
2. Test with prompts: "Review this Python code"
3. Verify it uses skill knowledge
4. Remove and test without: /skills remove my-new-skill
5. Compare behavior
```

### 7. Organize by Category

```
~/.copilot/skills/
├── languages/
│   ├── python-expert.skill.md
│   ├── javascript-expert.skill.md
│   └── typescript-expert.skill.md
├── frameworks/
│   ├── react-patterns.skill.md
│   ├── django-expert.skill.md
│   └── fastapi-expert.skill.md
├── domains/
│   ├── security-audit.skill.md
│   ├── performance-opt.skill.md
│   └── testing-expert.skill.md
└── tools/
    ├── docker-expert.skill.md
    ├── kubernetes-expert.skill.md
    └── aws-expert.skill.md
```

## Use Cases

### Use Case 1: Learning a New Framework

**Scenario**: You're learning FastAPI for the first time.

```bash
> /skills add fastapi-expert

> Create a FastAPI endpoint for user registration

[AI uses FastAPI skill to provide idiomatic patterns]
- Pydantic models for validation
- Dependency injection
- Async endpoints
- Proper status codes
- Error handling
```

### Use Case 2: Security Audit

**Scenario**: You want to audit your codebase for security issues.

```bash
> /skills add security-audit

> Review @src/**/*.py for security vulnerabilities

[AI uses security skill to check for]
- SQL injection risks
- XSS vulnerabilities
- Insecure dependencies
- Hardcoded secrets
- Authentication issues
```

### Use Case 3: Performance Optimization

**Scenario**: Your API is slow and needs optimization.

```bash
> /skills add performance-opt
> /skills add database-expert

> Analyze @src/api/**/*.py for performance issues

[AI uses skills to identify]
- N+1 query problems
- Missing indexes
- Inefficient algorithms
- Memory leaks
- Blocking I/O in async code
```

### Use Case 4: Cross-Team Consistency

**Scenario**: Ensure all team members follow same patterns.

```bash
# Create team-specific skill
~/.copilot/skills/company-python-standards.skill.md

# Share via Git
git add ~/.copilot/skills/company-python-standards.skill.md

# Team members activate
> /skills add company-python-standards
```

### Use Case 5: Polyglot Projects

**Scenario**: Monorepo with multiple languages.

```bash
# Working on Python microservice
> /skills add python-expert
> /skills add fastapi-expert

# Switching to React frontend
> /skills remove python-expert
> /skills remove fastapi-expert
> /skills add react-patterns
> /skills add typescript-expert
```

## Examples

### Example 1: React Patterns Skill

`~/.copilot/skills/react-patterns.skill.md`:

```markdown
# React Patterns Skill

## Metadata
- **ID**: react-patterns
- **Version**: 1.0.0
- **Category**: Frontend
- **Tags**: react, frontend, javascript, typescript, hooks

## Description
Modern React development patterns using hooks, context,
and functional components.

## Instructions

### Component Structure

```typescript
// ✅ Functional component with TypeScript
interface UserCardProps {
  user: User;
  onEdit?: (id: string) => void;
}

export function UserCard({ user, onEdit }: UserCardProps) {
  const handleClick = () => {
    onEdit?.(user.id);
  };
  
  return (
    <div className="user-card">
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      {onEdit && <button onClick={handleClick}>Edit</button>}
    </div>
  );
}
```

### State Management

```typescript
// ✅ useState for local state
const [count, setCount] = useState(0);

// ✅ useReducer for complex state
const [state, dispatch] = useReducer(reducer, initialState);

// ✅ Context for shared state
const ThemeContext = createContext<Theme>('light');
```

### Hooks Best Practices

```typescript
// ✅ Custom hook
function useAuth() {
  const [user, setUser] = useState<User | null>(null);
  
  useEffect(() => {
    const unsubscribe = auth.onAuthStateChanged(setUser);
    return unsubscribe;
  }, []);
  
  return { user, loading: user === undefined };
}

// ✅ Proper dependencies
useEffect(() => {
  fetchData(id);
}, [id]); // Include all dependencies
```

## Checklists

### Component Review
- [ ] Props interface defined
- [ ] No prop drilling (use Context if needed)
- [ ] Hooks at top level
- [ ] useEffect dependencies correct
- [ ] Accessible (ARIA labels)
- [ ] Error boundaries for error handling
```

### Example 2: Security Audit Skill

`~/.copilot/skills/security-audit.skill.md`:

```markdown
# Security Audit Skill

## Metadata
- **ID**: security-audit
- **Version**: 1.0.0
- **Category**: Security
- **Tags**: security, audit, vulnerabilities, owasp

## Description
Identifies common security vulnerabilities and provides remediation guidance.

## Instructions

### OWASP Top 10 Checks

1. **Injection (SQL, NoSQL, Command)**
```python
# ❌ SQL Injection risk
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ Parameterized query
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

2. **Broken Authentication**
```python
# ❌ Weak password hashing
import hashlib
hashed = hashlib.md5(password.encode()).hexdigest()

# ✅ Strong password hashing
import bcrypt
hashed = bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))
```

3. **Sensitive Data Exposure**
```python
# ❌ Logging sensitive data
logger.info(f"User {user.email} logged in with password {password}")

# ✅ Never log sensitive data
logger.info(f"User {user.id} logged in successfully")
```

### Security Checklist
- [ ] No hardcoded secrets
- [ ] Input validation on all endpoints
- [ ] SQL injection protection (parameterized queries)
- [ ] XSS protection (output encoding)
- [ ] CSRF protection enabled
- [ ] HTTPS enforced
- [ ] Secure headers configured
- [ ] Authentication on protected routes
- [ ] Rate limiting implemented
- [ ] Dependencies up to date (no known vulnerabilities)
```

## Troubleshooting

### Skills Not Loading

**Problem**: Skill doesn't appear in `/skills list`.

**Solutions**:

```bash
# 1. Check file location
ls -la ~/.copilot/skills/

# 2. Check file extension (.skill.md required)
# Must be: my-skill.skill.md

# 3. Reload skills
> /skills reload

# 4. Check file format (must be valid markdown)
```

### Skill Not Affecting Behavior

**Problem**: Skill is active but AI doesn't follow it.

**Solutions**:

```bash
# 1. Verify skill is active
> /skills list
# Look for ✅ next to skill name

# 2. Check skill content is specific enough
# Skills with vague guidance may not be effective

# 3. Explicitly mention the skill
> As a Python expert, review this code
> Following React patterns, create a component

# 4. Check context isn't too full
> /context
# If >80%, skill might be deprioritized
```

### Conflicting Skills

**Problem**: Multiple skills give contradictory advice.

**Solutions**:

```bash
# 1. Remove conflicting skills
> /skills remove skill-name

# 2. Use project-specific skills to override
.copilot/skills/project-overrides.skill.md

# 3. Be explicit about priority in skill files
## Priority
This skill overrides python-expert for this project.
```

### Skill File Errors

**Problem**: Syntax errors in skill file.

**Solutions**:

```bash
# 1. Validate markdown syntax
# Use markdown linter or preview

# 2. Check for required sections
# Metadata, Description, Instructions minimum

# 3. View error logs
> /skills reload
# Check for error messages
```

## Summary

### Key Differences Quick Reference

```
Skills:
✅ Specialized domain expertise
✅ Reusable across projects
✅ Enable/disable as needed
✅ Packaged knowledge modules
✅ Shareable with team

AGENTS.md:
✅ Project-specific configuration
✅ Architecture and patterns
✅ Business logic and rules
✅ Always active in project

Instructions:
✅ Coding style and conventions
✅ Team standards
✅ File organization
✅ Process guidelines
```

### When to Create a Skill

Create a skill when you have:
- ✅ Specialized knowledge to reuse
- ✅ Best practices for a specific technology
- ✅ Patterns that apply across projects
- ✅ Domain expertise to capture
- ✅ Team standards to enforce

### Benefits of Using Skills

1. **Modularity**: Add/remove expertise as needed
2. **Reusability**: Use across any project
3. **Shareability**: Distribute to team easily
4. **Consistency**: Same patterns everywhere
5. **Learning**: Codify expert knowledge
6. **Efficiency**: Quick domain switching

## Quick Commands Reference

```bash
# List skills
> /skills list

# Get info about a skill
> /skills info python-expert

# Add a skill
> /skills add python-expert

# Remove a skill
> /skills remove python-expert

# Reload all skills (after editing)
> /skills reload
```

## Additional Resources

- [Custom Instructions Guide](08-advanced-features.md#custom-instructions)
- [AGENTS.md Guide](13-agents-file.md)
- [Best Practices](10-best-practices.md)

---

**Note**: The skills system provides a powerful way to extend Copilot CLI with specialized knowledge. Start with built-in skills and create custom ones as you identify recurring patterns in your work.

**Next:** Continue to other guide sections  
**Previous:** [AGENTS.md File Guide](13-agents-file.md)
