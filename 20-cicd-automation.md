# CI/CD and Automation

GitHub Copilot CLI isn't just for interactive use — it's equally powerful in non-interactive, scripted, and CI/CD contexts. This guide covers everything you need to run Copilot CLI reliably in automated pipelines.

## Table of Contents

1. [Non-Interactive Mode](#1-non-interactive-mode)
2. [GitHub Actions Integration](#2-github-actions-integration)
3. [Authentication in CI](#3-authentication-in-ci)
4. [Shell Scripting Patterns](#4-shell-scripting-patterns)
5. [Makefile Integration](#5-makefile-integration)
6. [Docker Usage](#6-docker-usage)
7. [Cost & Quota Considerations in CI](#7-cost--quota-considerations-in-ci)
8. [Troubleshooting CI Issues](#8-troubleshooting-ci-issues)
9. [Quick Reference](#quick-reference)

---

## 1. Non-Interactive Mode

Copilot CLI provides several flags specifically designed for scripted and automated contexts.

### The `-p` / `--prompt` Flag

The `--prompt` (or `-p`) flag accepts a prompt string, runs it, and exits. No REPL, no interaction — just output and an exit code.

```bash
copilot --prompt "Summarize the changes in the last 5 commits"
copilot -p "Fix the failing test in src/auth.test.js"
```

### The `--attachment` Flag (v1.0.41+)

Attach **images or native documents** to the initial prompt in non-interactive mode:

```bash
copilot -p "Describe this architecture diagram" --attachment diagram.png
copilot -p "Summarize the key points" --attachment spec.pdf
```

Supported types include images (PNG, JPG, GIF, WebP) and native documents (PDF). Multiple `--attachment` flags can be provided.

### The `--silent` Flag

Suppresses usage statistics, banners, and informational output that would otherwise clutter CI logs.

```bash
copilot -p "Run linting and fix all errors" --silent
```

### The `--output-format json` Flag

Switches output to newline-delimited JSON (JSONL), making results machine-readable and easy to pipe into other tools.

```bash
copilot -p "List all API endpoints" --output-format json --silent
```

Each output line is a JSON object describing a step or result from the run, suitable for parsing with `jq` or a script.

### Environment Variables

| Variable | Purpose |
|----------|---------|
| `GH_TOKEN` | GitHub auth token (used by CLI and `gh`) |
| `GITHUB_TOKEN` | Alias for `GH_TOKEN`; automatically set in GitHub Actions |
| `COPILOT_GITHUB_TOKEN` | Takes precedence over `GH_TOKEN` for Copilot auth specifically |
| `COPILOT_ALLOW_ALL` | Set to `true` to skip tool-use confirmation prompts |
| `COPILOT_MODEL` | Override the active model (e.g., `claude-haiku-4-5`) |
| `COPILOT_AUTO_UPDATE` | Set to `false` to suppress auto-update checks |
| `NO_COLOR` | Set to `1` to disable ANSI color codes in output |
| `GITHUB_COPILOT_PROMPT_MODE_REPO_HOOKS=1` | Load AGENTS.md and instruction files in prompt mode (v1.0.40+) |
| `GITHUB_COPILOT_PROMPT_MODE_WORKSPACE_MCP=1` | Load workspace `.mcp.json` MCP servers in prompt mode (v1.0.40+) |
| `GITHUB_COPILOT_PROMPT_MODE_EXTENSIONS=true` | Load project extensions and management tools in prompt mode (v1.0.41+) |

### Exit Codes

| Code | Meaning |
|------|---------|
| `0` | Success — task completed without error |
| `1` | General failure — task failed or Copilot returned an error |
| `2` | Auth failure — no valid token found |
| `3` | Quota exceeded — premium request limit reached |

Use exit codes in scripts to branch on success/failure:

```bash
if copilot -p "Run tests" --allow-all-tools --silent; then
  echo "Tests passed"
else
  echo "Tests failed or Copilot encountered an error"
fi
```

### The `--no-auto-update` Flag

Prevents Copilot CLI from checking for or prompting about updates. Essential in CI to avoid interactive prompts blocking the pipeline.

```bash
copilot -p "..." --no-auto-update --silent
```

### The `--allow-all-tools` Flag

Bypasses individual tool-use confirmation prompts. Equivalent to setting `COPILOT_ALLOW_ALL=true`. Required in non-interactive contexts where there's no human to approve tool calls.

### Combining Flags for Clean CI Runs

```bash
# Basic non-interactive prompt
copilot --prompt "Fix the failing test in src/auth.test.js" --allow-all-tools --silent

# JSON output for parsing
copilot -p "List all API endpoints" --output-format json --silent

# Fully non-interactive CI run
COPILOT_ALLOW_ALL=true copilot -p "Run tests and fix any failures" --no-auto-update --silent

# Cap steps and use a specific model
copilot -p "Review this PR for security issues" \
  --model claude-haiku-4-5 \
  --max-autopilot-continues 10 \
  --silent --no-auto-update
```

---

## 2. GitHub Actions Integration

### Setup

1. Ensure `GITHUB_TOKEN` is available (it is by default in all Actions jobs)
2. Install Copilot CLI as a step
3. Pass the token via `GH_TOKEN` environment variable
4. Use `COPILOT_ALLOW_ALL: 'true'` to suppress confirmation prompts

### Example 1: Auto-fix Failing Tests on PR

Triggers on PRs labeled `copilot-fix`, runs the test suite, and pushes fixes automatically.

```yaml
name: Copilot Auto-fix
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  auto-fix:
    runs-on: ubuntu-latest
    if: contains(github.event.pull_request.labels.*.name, 'copilot-fix')
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
      - name: Install Copilot CLI
        run: curl -fsSL https://gh.io/copilot-install | bash
      - name: Run tests and auto-fix failures
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          COPILOT_ALLOW_ALL: 'true'
        run: |
          copilot --prompt "Run the test suite and fix any failing tests. 
          Commit the fixes with message 'fix: auto-fix failing tests'." \
          --allow-all-tools --silent --no-auto-update
      - name: Push fixes
        run: git push
```

### Example 2: Generate Release Notes

Runs on release creation, generates comprehensive release notes, and uploads them as an artifact.

```yaml
name: Generate Release Notes
on:
  release:
    types: [created]

jobs:
  release-notes:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Install Copilot CLI
        run: curl -fsSL https://gh.io/copilot-install | bash
      - name: Generate release notes
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          copilot -p "Compare the changes since the last release tag and 
          write comprehensive release notes. Include: new features, bug fixes, 
          breaking changes, and upgrade instructions. Save to RELEASE_NOTES.md." \
          --allow-all-tools --silent --no-auto-update
      - name: Upload release notes
        uses: actions/upload-artifact@v4
        with:
          name: release-notes
          path: RELEASE_NOTES.md
```

### Example 3: PR Code Review Comment

Runs Copilot as a code reviewer on every PR, outputs JSON findings, then posts them as a PR comment via `github-script`.

```yaml
name: Copilot Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Copilot CLI
        run: curl -fsSL https://gh.io/copilot-install | bash
      - name: Run code review
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          copilot -p "Review the changes in this PR. Focus on bugs, security issues,
          and logic errors. Output findings as JSON." \
          --output-format json --silent --no-auto-update > review.json
      - name: Post review comment
        uses: actions/github-script@v7
        with:
          script: |
            const review = require('./review.json')
            // post as PR comment...
```

### Example 4: Scheduled Documentation Update

Runs weekly, scans for undocumented functions, updates JSDoc and API reference docs, and opens a PR with the changes.

```yaml
name: Weekly Docs Update
on:
  schedule:
    - cron: '0 9 * * MON'

jobs:
  update-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Copilot CLI
        run: curl -fsSL https://gh.io/copilot-install | bash
      - name: Update API documentation
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          COPILOT_ALLOW_ALL: 'true'
        run: |
          copilot -p "Scan all source files for undocumented public functions 
          and add JSDoc comments. Update docs/api-reference.md to match." \
          --allow-all-tools --silent --no-auto-update
      - name: Create PR with changes
        run: |
          git config user.name "Copilot Bot"
          git config user.email "copilot@noreply"
          git checkout -b docs/weekly-update-$(date +%Y%m%d)
          git add -A
          git commit -m "docs: weekly API documentation update"
          gh pr create --title "Weekly docs update" --body "Automated update by Copilot CLI"
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 3. Authentication in CI

### Using `GH_TOKEN` / `GITHUB_TOKEN`

No interactive login is required in CI. Copilot CLI reads auth from environment variables:

- **`GITHUB_TOKEN`** is automatically injected into every GitHub Actions job — no secrets configuration needed for basic use
- **`GH_TOKEN`** is the standard variable used by `gh` CLI and Copilot CLI
- **`COPILOT_GITHUB_TOKEN`** takes precedence over both if set — useful when you need a different token specifically for Copilot calls

```yaml
env:
  GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Fine-Grained PAT Setup

For CI pipelines outside of GitHub Actions, create a fine-grained Personal Access Token with the minimum required permissions:

- **Repository permissions:** `Contents` (read/write if pushing), `Pull requests` (read/write if opening PRs)
- **Account permissions:** `GitHub Copilot` → `Copilot Requests` (required)

Store the token as a repository or organization secret, never hardcoded.

### Secret Management Best Practices

- ✅ Use `secrets.GITHUB_TOKEN` for Actions (automatic, scoped to the job)
- ✅ Use repository secrets for PATs: `secrets.COPILOT_TOKEN`
- ✅ Rotate PATs on a schedule (90-day maximum recommended)
- ❌ Never print tokens in logs (`echo $GH_TOKEN`)
- ❌ Never commit tokens to the repository

### Token Precedence

```
COPILOT_GITHUB_TOKEN > GH_TOKEN > GITHUB_TOKEN > ~/.config/gh/hosts.yml
```

---

## 4. Shell Scripting Patterns

### Basic Wrapper Script

```bash
#!/bin/bash
# Example: wrapper script for automated code fixes

set -euo pipefail

export GH_TOKEN="${GITHUB_TOKEN}"
export COPILOT_ALLOW_ALL=true

# Run fix and capture output
if copilot -p "$1" --silent --no-auto-update; then
  echo "✅ Fix applied successfully"
  exit 0
else
  echo "❌ Fix failed with exit code $?"
  exit 1
fi
```

### Capturing JSON Output

```bash
#!/bin/bash
# Parse structured output from Copilot

set -euo pipefail

OUTPUT=$(copilot -p "List all TODO comments in the codebase as JSON" \
  --output-format json --silent --no-auto-update)

# Parse with jq
echo "$OUTPUT" | jq '.[] | select(.type == "result") | .content'
```

### Conditional Automation

```bash
#!/bin/bash
# Only auto-fix if tests are currently failing

set -euo pipefail

if ! npm test --silent 2>/dev/null; then
  echo "Tests failing — invoking Copilot auto-fix..."
  copilot -p "Fix all failing tests in the test suite" \
    --allow-all-tools --silent --no-auto-update
  npm test  # verify fixes worked
else
  echo "Tests passing — no action needed"
fi
```

---

## 5. Makefile Integration

```makefile
.PHONY: ai-fix ai-review ai-docs ai-ci

ai-fix:
	copilot -p "Fix all lint errors and failing tests" --allow-all-tools --silent

ai-review:
	copilot -p "Review staged changes for bugs and security issues" --silent

ai-docs:
	copilot -p "Update JSDoc for all modified files" --allow-all-tools --silent

ai-ci:
	COPILOT_ALLOW_ALL=true copilot \
		-p "Run the full CI pipeline locally and fix any issues" \
		--no-auto-update --silent
```

Usage:

```bash
make ai-fix      # Auto-fix lint and test failures
make ai-review   # Review staged changes before committing
make ai-docs     # Keep docs in sync with code changes
```

---

## 6. Docker Usage

### Basic Dockerfile

```dockerfile
FROM ubuntu:22.04

# Install dependencies
RUN apt-get update && apt-get install -y curl git && rm -rf /var/lib/apt/lists/*

# Install Copilot CLI
RUN curl -fsSL https://gh.io/copilot-install | bash

# Set CI-friendly defaults
ENV COPILOT_ALLOW_ALL=true
ENV NO_COLOR=1

ENTRYPOINT ["copilot"]
```

### Usage

```bash
# Build the image
docker build -t copilot-ci .

# Run a one-off prompt
docker run --rm \
  -e GH_TOKEN="${GH_TOKEN}" \
  -v "$(pwd):/workspace" \
  -w /workspace \
  copilot-ci -p "Fix all TypeScript errors" --silent --no-auto-update
```

### Docker Compose for Local CI

```yaml
services:
  copilot:
    build: .
    environment:
      - GH_TOKEN
      - COPILOT_ALLOW_ALL=true
      - NO_COLOR=1
    volumes:
      - .:/workspace
    working_dir: /workspace
```

---

## 7. Cost & Quota Considerations in CI

### Premium Request Model

Each `-p` invocation consumes premium requests. A complex multi-step task can consume dozens of requests per run. Plan accordingly before enabling CI automation.

### Choosing a Model for CI

Use `--model` (or `COPILOT_MODEL`) to select a cost-effective model for routine CI tasks:

| Task | Recommended Model | Why |
|------|-------------------|-----|
| Formatting, docs | `claude-haiku-4-5` | Fast, low cost |
| General coding, tests | `claude-sonnet-4-5` | Balanced cost/quality |
| Architecture, security | `claude-opus-4-5` | Best reasoning, highest cost |

```bash
# Use Haiku for routine doc updates
copilot -p "Add JSDoc to all exported functions" \
  --model claude-haiku-4-5 --allow-all-tools --silent

# Use Opus only for security-critical reviews
copilot -p "Audit auth.ts for security vulnerabilities" \
  --model claude-opus-4-5 --silent
```

### Capping Steps Per Run

Use `--max-autopilot-continues` to limit how many autonomous steps Copilot takes per run. This prevents runaway jobs from consuming excessive quota:

```bash
copilot -p "Fix all failing tests" \
  --allow-all-tools \
  --max-autopilot-continues 15 \
  --silent
```

### Benchmarking Before Automating

Before adding Copilot to a CI pipeline:

1. Run the task interactively a few times
2. Use `/usage` to check how many requests it consumed
3. Multiply by expected CI frequency
4. Confirm quota headroom before scheduling

### What Happens When Quota Runs Out

- Copilot CLI returns exit code `3`
- No partial work is committed unless Copilot already ran `git commit` steps
- The pipeline job fails — design your workflows to handle this gracefully

---

## 8. Troubleshooting CI Issues

| Issue | Cause | Fix |
|-------|-------|-----|
| "Not authenticated" | No `GH_TOKEN` in environment | Set `GH_TOKEN` secret in Actions |
| Interactive prompt shown | Missing `--allow-all-tools` | Add `COPILOT_ALLOW_ALL=true` |
| Auto-update prompt blocks CI | Missing `--no-auto-update` | Add `--no-auto-update` flag or `COPILOT_AUTO_UPDATE=false` |
| High quota consumption | No step cap | Add `--max-autopilot-continues 10` |
| Noisy output in logs | Missing `--silent` | Add `--silent` flag |
| ANSI codes in log output | Color enabled by default | Set `NO_COLOR=1` in environment |
| Exit code 1 with no error | Copilot hit a logic failure | Check output with `--output-format json` for details |
| JSON parse errors | Output contains non-JSON lines | Filter with `jq` using `--raw-input` |
| Task diverges from intent | Prompt too vague | Use more specific, scoped prompts in CI |

---

## Quick Reference

```bash
# Minimum viable CI invocation
GH_TOKEN=<token> copilot -p "your task" --allow-all-tools --silent --no-auto-update

# With model selection and step cap
copilot -p "your task" \
  --model claude-haiku-4-5 \
  --max-autopilot-continues 10 \
  --allow-all-tools \
  --silent \
  --no-auto-update

# JSON output for parsing
copilot -p "your task" --output-format json --silent --no-auto-update | jq .

# Environment variable approach
export GH_TOKEN="${GITHUB_TOKEN}"
export COPILOT_ALLOW_ALL=true
export COPILOT_MODEL=claude-haiku-4-5
export NO_COLOR=1
copilot -p "your task" --silent --no-auto-update
```

### Flag Summary

| Flag | Short | Effect |
|------|-------|--------|
| `--prompt` | `-p` | Run one prompt and exit |
| `--silent` | — | Suppress banners and usage stats |
| `--allow-all-tools` | — | Skip tool confirmation prompts |
| `--no-auto-update` | — | Suppress update checks |
| `--output-format json` | — | JSONL output |
| `--model` | — | Override active model |
| `--max-autopilot-continues` | — | Cap autonomous steps |

---

**Next:** [Team Setup](21-team-setup.md)
**Previous:** [Research Command](19-research-command.md)
**Related:** [Models and Costs](22-models-and-costs.md) | [Best Practices](10-best-practices.md)
