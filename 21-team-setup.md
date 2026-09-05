# Team Setup

Deploying Copilot CLI across a team or organization goes beyond individual installation. This guide covers everything engineering leads and managers need to ensure consistent AI behavior, shared standards, efficient onboarding, and cost visibility at scale.

## Table of Contents

1. [Why Team Setup Matters](#1-why-team-setup-matters)
2. [Repository Configuration](#2-repository-configuration)
3. [Shared Skills Setup](#3-shared-skills-setup)
4. [Onboarding Checklist for New Engineers](#4-onboarding-checklist-for-new-engineers)
5. [Model Strategy for Teams](#5-model-strategy-for-teams)
6. [Quota and Cost Management](#6-quota-and-cost-management)
7. [Security Configuration for Teams](#7-security-configuration-for-teams)
8. [Advanced: Organization-Level Setup](#8-advanced-organization-level-setup)
9. [Troubleshooting Team Issues](#9-troubleshooting-team-issues)

---

## 1. Why Team Setup Matters

Without shared configuration, every engineer on the team effectively uses a different AI assistant — one that knows nothing about your tech stack, your conventions, or your standards. The result is inconsistent code, repeated corrections, and AI that generates patterns your team will reject in review.

A well-configured team setup delivers:

- **Consistent AI behavior** — every engineer gets suggestions that match your stack and patterns
- **Shared coding standards baked into AI context** — no more correcting the AI for using `console.log` instead of your logger
- **Faster onboarding** — new engineers can ask Copilot to explain the codebase and get accurate, context-aware answers from day one
- **Cost visibility and quota management** — prevent surprise quota exhaustion with team-wide guidance and CI guardrails

The investment is small: a few configuration files committed to the repository, and a team onboarding checklist. The payoff compounds with every PR.

---

## 2. Repository Configuration

The primary mechanism for teaching Copilot about your project is files committed to the repository. These files are read automatically when engineers run Copilot CLI from the project root.

### Which Files to Commit

| File | Read by | Purpose |
|------|---------|---------|
| `AGENTS.md` | Copilot CLI, Codex, Devin, and most agentic tools | Primary coding standards and project context |
| `.github/copilot-instructions.md` | Copilot CLI, GitHub Copilot IDE Chat, coding agent | Copilot-ecosystem rules, also reaches IDE users |
| `.github/instructions/*.instructions.md` | Copilot CLI, IDE Chat | Per-topic instruction files (e.g., `testing.instructions.md`) |

For a detailed breakdown of how these files are loaded and what each field controls, see [AGENTS.md Guide](13-agents-file.md).

### AGENTS.md — Primary Coding Standards

This is the most important file. It provides the AI with your tech stack, patterns, and requirements upfront. Here is a complete, copy-paste-ready template:

```markdown
# Project: [Your Project Name]

## Tech Stack
- Runtime: Node.js 20 LTS
- Framework: Express 4 / Fastify
- Database: PostgreSQL with Prisma ORM
- Testing: Vitest
- Linting: ESLint + Prettier

## Code Patterns
- Always use async/await — never raw Promises or callbacks
- All database queries must use Prisma — never raw SQL
- Error handling: use the AppError class in src/utils/errors.ts
- Logging: use the logger in src/utils/logger.ts — never console.log

## API Conventions
- REST endpoints: GET/POST/PUT/DELETE /api/v1/resources
- Response format: { success: true, data: {...}, meta: {...} }
- Errors: { success: false, error: { code, message, details } }

## Testing Requirements
- Unit tests required for all service functions
- Integration tests required for all API endpoints
- Use factories in tests/factories/ for test data
- Minimum 80% branch coverage for new code

## Naming Conventions
- Files: kebab-case (user-service.ts, auth-middleware.ts)
- Classes: PascalCase
- Functions and variables: camelCase
- Constants: SCREAMING_SNAKE_CASE
- Database tables: snake_case

## PR Requirements
- PR title must follow Conventional Commits (feat/fix/chore/docs/refactor)
- All PRs must include tests
- Reference the issue number in the PR body
- No PR merges without passing CI

## Paths to Avoid
- Never read or modify .env files
- Never commit secrets or credentials
- Treat src/legacy/ as read-only unless explicitly asked
```

### `.github/copilot-instructions.md` — Copilot-Specific Rules

This file is read by both Copilot CLI and GitHub Copilot in the IDE, making it ideal for rules that apply across the full Copilot ecosystem:

```markdown
# Copilot Instructions

## General Approach
- Always read AGENTS.md before making changes
- Prefer editing existing files over creating new ones
- Run tests after any code change — never assume they pass

## When Opening PRs
- Use `gh pr create` with a descriptive title following Conventional Commits
- Always include "Closes #<issue>" in the PR body
- Request review from the `@team-name/code-owners` team

## What Not to Do
- Do not install new dependencies without confirming with the user
- Do not modify database migrations once merged
- Do not alter CI configuration files without explicit instruction
```

### Per-Topic Instruction Files

For large projects, split instructions by topic:

```
.github/instructions/
├── testing.instructions.md       # Test patterns and requirements
├── api.instructions.md           # API design rules
├── database.instructions.md      # Database and migration rules
└── security.instructions.md      # Security review checklist
```

Each file follows the same format as `copilot-instructions.md` and is loaded based on file-matching patterns you configure.

---

## 3. Shared Skills Setup

Skills are reusable prompt templates that team members can activate with `@skill-name`. See [Skills System](14-skills-system.md) for full documentation.

### Where to Store Shared Skills

Skills are stored in `~/.copilot/skills/` on each engineer's machine. There is no centralized skill server — sharing skills means distributing skill files.

Options for team skill distribution:

1. **Committed to the repo** — store skill files in `.github/skills/` and document that engineers should copy them to `~/.copilot/skills/` during setup
2. **Internal wiki or runbook** — link to skill files for manual installation
3. **Dotfiles repo** — engineers who use a shared dotfiles repo can include skills there
4. **Setup script** — a `scripts/setup-copilot.sh` that copies skill files into place

### Recommended Skill Set for a Team

```
~/.copilot/skills/
├── security-auditor.md     # Audit code for OWASP top 10 vulnerabilities
├── test-writer.md          # Generate full test coverage for a module
├── api-doc-generator.md    # Generate OpenAPI spec from route handlers
├── pr-writer.md            # Draft PR descriptions from staged diff
└── migration-reviewer.md   # Review database migrations for safety
```

**Example skill file — `security-auditor.md`:**

```markdown
---
name: security-auditor
description: Audit code for common security vulnerabilities
---

Review the provided code for security vulnerabilities. Check for:
- SQL injection (even through ORMs — check raw query escapes)
- XSS (unescaped user input in HTML/templates)
- CSRF (missing token validation on state-changing endpoints)
- Auth bypass (missing middleware, incorrect role checks)
- Insecure direct object references
- Secrets in code or logs
- Dependency vulnerabilities (flag outdated packages)

Output a structured report: severity (critical/high/medium/low), location, description, and recommended fix for each finding.
```

### Activating Shared Skills

Once a skill file is in `~/.copilot/skills/`, engineers activate it with:

```
@security-auditor Review src/api/payments.ts
```

---

## 4. Onboarding Checklist for New Engineers

Provide this checklist to every new engineer joining the team. It covers first-day setup through ongoing habits.

```markdown
## Copilot CLI Onboarding Checklist

### Day 1 — Setup
- [ ] Install Copilot CLI: `curl -fsSL https://gh.io/copilot-install | bash`
- [ ] Authenticate: run `copilot` and follow /login
- [ ] Verify AGENTS.md is loaded: start CLI, check it mentions our tech stack
- [ ] Run /terminal-setup for multiline support
- [ ] Set preferred model: /model → Claude Sonnet (default is fine to start)
- [ ] Install team skills: `cp -r .github/skills/* ~/.copilot/skills/`

### Week 1 — Core Workflows
- [ ] Complete the interactive tutorial: ask Copilot "Explain this codebase to me"
- [ ] Use plan mode for your first feature: Shift+Tab → describe task → review plan
- [ ] Use /delegate for your first PR
- [ ] Read 10-best-practices.md
- [ ] Try the @security-auditor skill on a file you're modifying

### Ongoing
- [ ] Use /research before touching unfamiliar modules
- [ ] Run /review before every /delegate
- [ ] Monitor usage with /usage — stay within team quota
- [ ] Run CLI from the project root (so AGENTS.md is always loaded)
- [ ] Check /instructions to confirm context is loaded correctly
```

---

## 5. Model Strategy for Teams

### Recommended Defaults by Role

| Role | Day-to-Day Model | Why |
|------|-----------------|-----|
| Frontend | `claude-sonnet-4-5` | Strong at UI patterns, component logic |
| Backend | `claude-sonnet-4-5` | Good balance of speed and reasoning |
| DevOps / Infra | `claude-sonnet-4-5` | Handles config files and scripting well |
| Security reviews | `claude-opus-4-5` | Best reasoning for vulnerability analysis |
| Architecture / design | `claude-opus-4-5` | Strongest at systems thinking |

### Cost-Conscious Patterns

Match model capability to task complexity — don't use Opus for tasks Haiku handles well:

```bash
# Formatting and doc tasks → Haiku
copilot -p "Add JSDoc to all exported functions in src/utils/" \
  --model claude-haiku-4-5 --allow-all-tools --silent

# General coding → Sonnet (default)
copilot -p "Add pagination to the /api/v1/users endpoint"

# Security audit → Opus
copilot -p "Audit the authentication flow for vulnerabilities" \
  --model claude-opus-4-5
```

### Setting a Team Default Model

Set `COPILOT_MODEL` in a shared environment file that engineers source in their shell profile:

```bash
# .env.shared (checked into repo, no secrets)
export COPILOT_MODEL=claude-sonnet-4-5
```

Engineers add to their `~/.zshrc` or `~/.bashrc`:

```bash
source /path/to/project/.env.shared
```

### Preventing Expensive Models in CI

In CI workflows, always explicitly set the model to avoid accidentally running with Opus:

```yaml
env:
  COPILOT_MODEL: claude-haiku-4-5
  COPILOT_ALLOW_ALL: 'true'
```

---

## 6. Quota and Cost Management

### Premium Request Model

Copilot CLI uses a premium request system where each AI call consumes requests at a rate that varies by model:

| Model | Request multiplier (approximate) |
|-------|--------------------------------|
| Haiku | 1× |
| Sonnet | 1× |
| Opus | 10× |

Multi-step agentic tasks (plan mode, `/delegate`) can consume many requests in a single session. A complex refactor might use 30–100 requests.

### Per-Person Expectations

Set clear expectations with the team:

- **Interactive work:** 200–500 requests/day is reasonable for heavy use
- **CI automation:** Budget separately — each CI run can use 10–50 requests
- **Avoid Opus for routine tasks** — it burns quota 10× faster

### Monitoring Usage

Engineers can check their consumption at any time:

```
/usage
```

This shows requests used in the current billing period. Encourage engineers to check weekly and report if they're approaching limits.

### CI Quota Considerations

CI pipelines run unattended and can consume quota faster than interactive use. Before automating:

1. Prototype the task interactively and measure usage with `/usage`
2. Add `--max-autopilot-continues` to cap steps per run
3. Use the cheapest model that produces acceptable results
4. Limit CI triggers (e.g., only run on labeled PRs, not every push)

### What Happens When Quota Runs Out

- Interactive sessions display a quota exceeded message
- `-p` runs return exit code `3`
- CI jobs fail — design workflows to handle this gracefully with `|| true` or explicit exit code handling
- Quota resets at the start of the next billing period

---

## 7. Security Configuration for Teams

### Restricting Tool Access

Use `--deny-tool` to prevent Copilot from using specific tools in sensitive contexts:

```bash
# Prevent file writes in review-only mode
copilot -p "Review this PR" --deny-tool write_file --deny-tool run_command
```

For interactive sessions, add restrictions to AGENTS.md:

```markdown
## Tool Restrictions
- Do not run database migrations automatically
- Do not push to remote branches without explicit confirmation
- Do not modify files in src/legacy/ without explicit instruction
```

### Pinning Model and Deny Lists via `.github/copilot/settings.json` (v1.0.70+)

A trusted repository can commit `.github/copilot/settings.json` to enforce consistent, org-approved defaults for everyone who runs Copilot CLI in that repo — no per-engineer configuration required:

```json
{
  "model": "claude-sonnet-4.6",
  "effort": "medium",
  "contextTier": "default",
  "denyUrls": ["https://internal-only.example.com/**"],
  "denyMcpServers": ["untrusted-server"],
  "denySkills": ["experimental-skill"]
}
```

**What it does:**
- Pins the **model**, **reasoning effort level**, and **context tier** for anyone working in the repo — engineers can't accidentally run an unapproved or overly expensive model
- **Extends** (not replaces) the existing URL, MCP server, and skill deny lists, so org-wide restrictions still apply on top of repo-level ones

**Why use it:** Centralize cost and security policy at the repository level instead of relying on every engineer to configure `--deny-tool`, `/model`, or `/settings` correctly.

### Protecting Sensitive Paths via AGENTS.md

Explicitly instruct Copilot to avoid sensitive paths:

```markdown
## Paths That Are Off-Limits
- Never read .env, .env.local, .env.production, or any secrets file
- Never read or modify .github/workflows/ without explicit instruction
- Never read files in /secrets/ or /credentials/
- Treat infrastructure/ as read-only
```

### Recommended `.gitignore` Additions

Do not commit Copilot session logs or local config that may contain sensitive context:

```gitignore
# Copilot CLI — do not commit
.copilot/logs/
.copilot/session-state/
~/.copilot/logs/
```

Note: `AGENTS.md` and `.github/copilot-instructions.md` **should** be committed — they contain coding standards, not secrets.

### Enforcing a Managed Sandbox Floor (v1.0.76+)

Enterprise administrators can enforce a restrictive sandbox floor via managed settings. This tightens — but never loosens — an individual user's sandbox policy, so engineers can still restrict things further for themselves but can't weaken org-mandated protections:

- The `/sandbox` dialog surfaces the org-configured managed values with **locked fields** so users can see exactly what's enforced
- Managed filesystem paths (allowed/denied) are shown alongside the user's own settings
- Users retain the ability to add stricter local restrictions on top of the managed floor

**Why use it:** Guarantee a minimum sandbox security baseline across the org (e.g., blocking network egress or sensitive paths) without relying on every engineer to configure `/sandbox` correctly, while still letting teams add their own tighter restrictions per repo or role.

> **v1.0.77+:** The managed sandbox policy can also be enforced via **native macOS and Windows MDM (Mobile Device Management) settings**, in addition to existing managed-settings mechanisms. This lets IT teams roll out and audit the sandbox floor using the same MDM tooling they already use for other endpoint policies.

### Restricting Sign-In to Approved Organizations (v1.0.83+)

Enterprise admins can pin `copilot login` to a set of approved GitHub organizations using the `forceLoginOrgs` managed setting. Users attempting to sign in with an account that isn't a member of one of the listed organizations are blocked from completing authentication.

**Why use it:** Prevents engineers from authenticating Copilot CLI with a personal or non-approved organization account, keeping usage tied to org-managed billing, policy, and audit trails.

### Audit Considerations

- Copilot CLI logs sessions to `~/.copilot/logs/` — review these if an automated job behaves unexpectedly
- For regulated environments, consider running Copilot in Docker with a read-only filesystem mount except for specific output directories
- All code changes made by Copilot must pass the same review process as human-authored code — Copilot is not a bypass for code review

---

## 8. Advanced: Organization-Level Setup

### Global User-Level Instructions

Engineers can set personal global instructions that apply across all projects in `$HOME/.copilot/copilot-instructions.md`:

```markdown
# My Global Copilot Instructions

- Always follow the AGENTS.md in the current project if present
- Prefer TypeScript over JavaScript when creating new files
- Always explain what you're about to do before making changes
```

### `COPILOT_CUSTOM_INSTRUCTIONS_DIRS` — Shared Instruction Directories

This environment variable lets you point Copilot at a directory of instruction files that are loaded in addition to per-repo files. This is the primary mechanism for organization-level standards:

```bash
# In ~/.zshrc or a shared .env.shared
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="/opt/company/copilot-instructions"
```

The directory can be:
- A network mount shared across the team
- A symlink to a checked-out internal standards repository
- A path managed by a configuration management tool (Ansible, Chef, etc.)

### Setting Up a Shared Team Instructions Directory

1. Create an internal repository: `github.com/your-org/copilot-standards`
2. Add instruction files for your organization's cross-cutting standards
3. Have engineers clone it to a standard path (e.g., `~/company/copilot-standards`)
4. Add to their shell profile:
   ```bash
   export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="$HOME/company/copilot-standards"
   ```
5. Update the standards repo periodically — engineers pull changes on their schedule

This approach means your organization-wide standards live in version control, have a review process, and are decoupled from any individual project.

---

## 9. Troubleshooting Team Issues

| Issue | Fix |
|-------|-----|
| Team member gets different AI behavior | Check their AGENTS.md is being read: run `/instructions` and verify the file appears |
| New engineer's CLI doesn't follow team patterns | Ensure they're running CLI from the project root (not a subdirectory) |
| Inconsistent model usage across the team | Set `COPILOT_MODEL` in team `.env.shared` and have everyone source it |
| Shared skill not available for an engineer | Copy skill file to `~/.copilot/skills/` on their machine |
| AI ignores AGENTS.md rules | Check for syntax issues in AGENTS.md; use `/instructions` to verify it loaded |
| Engineer consistently over quota | Review their workflows — likely using Opus for routine tasks or running unthrottled CI |
| CI consuming too much quota | Add `--max-autopilot-continues`, use Haiku, and limit CI trigger conditions |
| Different behavior in CI vs locally | Ensure `COPILOT_MODEL` and `GH_TOKEN` are set consistently in both environments |

---

**Next:** [Models and Costs](22-models-and-costs.md)
**Previous:** [CI/CD and Automation](20-cicd-automation.md)
**Related:** [AGENTS.md Guide](13-agents-file.md) | [Skills System](14-skills-system.md) | [Best Practices](10-best-practices.md)
