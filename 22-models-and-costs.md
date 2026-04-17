# 22. Model Selection Strategy and Cost Management

Understanding how to choose the right model — and when to switch — is one of the highest-leverage skills for getting the most out of Copilot CLI. This guide covers available models, cost mechanics, and practical strategies for individuals and teams.

## Table of Contents

1. [Available Models Overview](#1-available-models-overview)
2. [What Are Premium Requests?](#2-what-are-premium-requests)
3. [Model Selection Decision Guide](#3-model-selection-decision-guide)
4. [Cost Optimization Strategies](#4-cost-optimization-strategies)
5. [Switching Models](#5-switching-models)
6. [Team Budget Patterns](#6-team-budget-patterns)
7. [Model-Specific Tips](#7-model-specific-tips)
8. [Understanding the /model Output](#8-understanding-the-model-output)
9. [Quick Reference](#9-quick-reference)

---

## 1. Available Models Overview

| Model | ID | Speed | Best for |
|-------|-----|-------|---------|
| Claude Sonnet 4.5 | `claude-sonnet-4.5` | Fast | Default — general coding, balanced quality/cost |
| Claude Sonnet 4.6 | `claude-sonnet-4.6` | Fast | Latest Sonnet — improved reasoning over 4.5 |
| Claude Opus 4.6 | `claude-opus-4.6` | Slower | Most capable — complex reasoning, architecture, security |
| Claude Opus 4.6 (fast) | `claude-opus-4.6-fast` | Fast | Opus quality with faster response times |
| Claude Opus 4.7 | `claude-opus-4.7` | Slower | Latest Opus — most capable model (v1.0.29+) |
| Claude Haiku 4.5 | `claude-haiku-4.5` | Fastest | Quick tasks, fleet subagents, docs, formatting |
| GPT-5.4 | `gpt-5.4` | Fast | Strong alternative for general code generation |
| GPT-5.3-Codex | `gpt-5.3-codex` | Fast | Code-specialized tasks |
| GPT-5.2-Codex | `gpt-5.2-codex` | Fast | Code-specialized, stable alternative |
| GPT-5.4 mini | `gpt-5.4-mini` | Fastest | Ultra-cheap for trivial or bulk tasks |
| GPT-4.1 | `gpt-4.1` | Fast | Fast, cost-effective general tasks |

> **Note:** Multipliers can change as GitHub updates pricing. Always run `/model` to see current multipliers and available models before committing to a long session.

---

## 2. What Are Premium Requests?

Copilot CLI consumes **premium requests** each time the model is invoked. Understanding what drives consumption helps you predict and control costs.

### How Consumption Works

- **One interaction = one premium request** (at baseline)
- **Multiplier effect**: actual cost = multiplier × base request
  - A 1x model consumes 1 premium request per interaction
  - A 0.33x model (Haiku) consumes ~0.33 premium requests per interaction
- **Multiplier of 0x** (e.g., GPT-5 Mini) means the model is free at the time it was configured that way — verify current status with `/model`

### High-Consumption Modes

| Mode / Feature | How it multiplies cost |
|----------------|------------------------|
| **Autopilot** | Each autonomous continuation step = additional premium requests |
| **Fleet mode** | Each subagent interaction = premium requests; large fleets add up quickly |
| **`/research`** | Many tool calls per research run = many requests |
| **`/delegate`** | Runs an autonomous subagent — multiple interactions chained |

### Monitoring Usage

```
> /usage
```

This shows how many premium requests you've consumed in the current session. Check it before kicking off a long autopilot or fleet run.

---

## 3. Model Selection Decision Guide

### By Task Type

| Task | Recommended Model | Why |
|------|-------------------|-----|
| Architecture planning | Claude Opus 4.7 | Latest Opus — deepest reasoning, considers trade-offs and edge cases |
| Security audit | Claude Opus 4.7 | Catches subtle vulnerabilities and attack vectors |
| General feature development | Claude Sonnet 4.5 | Good quality, standard cost — the safe default |
| Latest capabilities | Claude Sonnet 4.6 | Improved reasoning over Sonnet 4.5 |
| Writing tests | Claude Haiku 4.5 | Pattern-matching task, doesn't require deep reasoning |
| Adding JSDoc/comments | Claude Haiku 4.5 | Templated output, no deep reasoning needed |
| Code formatting / linting fixes | Claude Haiku 4.5 | Simple, repetitive, cheap to run |
| Debugging complex issues | Claude Sonnet 4.5 or Opus 4.7 | Depends on how deep the issue goes |
| Refactoring a large codebase | Claude Sonnet 4.5 | Balance of quality and cost across many files |
| CI/CD automation | Claude Haiku 4.5 or GPT-5.4 mini | Cost-effective for automated / unattended runs |
| Code-specialized tasks | GPT-5.3-Codex or GPT-5.2-Codex | Optimized for code generation |
| `/research` deep-dives | Fixed by research agent | The research command uses its own model — not configurable |

### Decision Tree

```
Is this task complex reasoning, security, or architecture?
  YES → Claude Opus 4.7
  NO → Is this a simple, repetitive task (docs, formatting, tests)?
    YES → Claude Haiku 4.5 or GPT-5.4 mini  (save costs)
    NO → Claude Sonnet 4.5  (default, balanced)
```

When in doubt, start with **Sonnet 4.5**. Upgrade to Opus 4.7 if the quality isn't sufficient; downgrade to Haiku 4.5 if you just need bulk work done cheaply.

---

## 4. Cost Optimization Strategies

### Fleet Mode — Use Cheap Subagents

In `/fleet`, each task spins up a subagent. You can explicitly direct fleet to use a cheaper model for bulk work and reserve a better model for the parts that matter:

```
> /fleet Use Claude Haiku 4.5 to add JSDoc comments to all files in src/utils/,
         and use Claude Sonnet 4.5 to refactor the auth module.
```

### Autopilot — Set a Step Cap

Autopilot can run many continuation steps. Cap the number of autonomous steps to avoid surprise costs:

```bash
copilot --max-autopilot-continues 10 --model claude-haiku-4.5
```

This is especially useful when running Copilot in scripts or on large tasks where you're not monitoring in real time.

### CI/CD — Always Use a Cheap Model

Automated pipelines should default to the cheapest viable model. Use an environment variable to enforce this across all pipeline runs:

```bash
# In your CI environment (e.g., GitHub Actions)
COPILOT_MODEL=claude-haiku-4.5 copilot -p "Fix lint errors" --silent
```

See [CI/CD and Automation](20-cicd-automation.md) for more pipeline patterns.

### Check Cost Before Committing to a Long Task

Before starting something that will consume many requests (autopilot, fleet, research), take 30 seconds to check your current model and session usage:

```
> /model    # See current model and its multiplier
> /usage    # Check what you've already spent this session
```

If you're already deep into a session with a high-multiplier model, consider switching to Haiku for the remaining work.

---

## 5. Switching Models

You can change models at any point — interactively, per session, or globally.

### Interactive Selection

```
> /model
```

This opens a picker showing all available models with their current multipliers. Use arrow keys to select and press Enter to switch.

### Set for a Single Session (Flag)

```bash
copilot --model claude-opus-4.6
```

This sets the model for the duration of that CLI invocation. It does not persist between sessions.

### Set Globally (Environment Variable)

```bash
export COPILOT_MODEL=claude-haiku-4.5
```

Add this to your shell profile (`.bashrc`, `.zshrc`) to make it the default across all sessions. Override per-session with the `--model` flag.

### Switch Mid-Session

```
> /model claude-sonnet-4.5
```

Pass the model ID directly to `/model` to switch without going through the picker. Useful when you want to upgrade for one complex question then switch back.

---

## 6. Team Budget Patterns

When multiple people are using Copilot CLI, a consistent team policy prevents unexpected cost spikes and ensures the right models are used for the right situations.

### Recommended Team Policy

| Scenario | Model | How enforced |
|----------|-------|--------------|
| Default daily coding | Claude Sonnet 4.5 | Team `.env.shared` or org default |
| Latest capabilities | Claude Sonnet 4.6 | Opt-in per session |
| Architecture / security reviews | Claude Opus 4.7 | Opt-in per task (manual override) |
| CI/CD pipelines | Claude Haiku 4.5 or GPT-5.4 mini | Env var in pipeline config |
| Fleet subagents | Claude Haiku 4.5 | Specify in `/fleet` prompt |

### Enforcing a Team Default

Add to a shared environment file (e.g., `.env.shared`, sourced in team onboarding):

```bash
# .env.shared
export COPILOT_MODEL=claude-sonnet-4.5
```

For CI/CD specifically, set the env var in your pipeline definition (GitHub Actions, Jenkins, etc.) rather than relying on the shared config.

### Monitoring

- **Individual usage**: visible via `/usage` in-session
- **Org-level usage**: GitHub org admins can monitor consumption in the GitHub organization settings under Copilot billing

### Educating the Team

A few key messages that prevent the most wasted spend:
- Opus is for deliberate, high-value reasoning tasks — not everyday Q&A
- Haiku handles anything repetitive: tests, comments, formatting, CI tasks
- Always check `/usage` before kicking off a fleet or autopilot run

---

## 7. Model-Specific Tips

### Claude Opus 4.7 — Getting the Most Out of It

Opus 4.7 is the latest and most capable Opus model (available from v1.0.29). Use it when quality or depth of reasoning genuinely matters:

- **"Explain why this is wrong"** — Opus surfaces non-obvious issues
- **"What are the security implications of this design?"** — Opus thinks through attack surfaces more thoroughly
- **`/research` on complex architecture questions** — pairs well with Opus for synthesized answers
- **Initial architecture decisions** — pays for itself by preventing rework later
- Consider switching *back* to Sonnet once you have a clear plan and just need implementation

### Claude Haiku 4.5 — Ideal For

- Fleet subagents doing repetitive work across many files
- CI/CD pipelines where you want consistent, cheap automation
- Adding JSDoc, docstrings, or inline comments
- Formatting and linting fix passes
- Quick "what does this function do?" questions that don't need deep reasoning
- Any task where the pattern is clear and correctness is easy to verify

### Codex Models — Best For

- Pure code generation tasks (more code, less explanation)
- When you want a code-first response without lengthy commentary
- Tasks where you're iterating on code output and don't need reasoning traces

---

## 8. Understanding the /model Output

Running `/model` without arguments displays something like this:

```
Available models:

  claude-haiku-4.5         0.33x  ⚡ Fastest
  claude-sonnet-4.5        1x     ✓ [current]
  claude-sonnet-4.6        1x     ✨ Latest Sonnet
  claude-opus-4.6          1x     🔍 Most capable
  claude-opus-4.6-fast     1x     🔍 Opus (fast)
  gpt-5.4-mini             0.33x  ⚡ Fast/cheap
  gpt-5.4                  1x
  gpt-5.3-codex            1x     💻 Code-specialized
  gpt-5.2-codex            1x     💻 Code-specialized
  gpt-4.1                  0.33x  ⚡ Fast/cheap

Select a model or press Esc to cancel.
```

**What each field means:**

| Field | Description |
|-------|-------------|
| Model name | The identifier used with `--model` or `COPILOT_MODEL` |
| Multiplier (e.g., `1x`, `0.33x`, `0x`) | How many premium requests each interaction costs relative to the base rate |
| `[current]` indicator | The model currently active in this session |
| Speed/capability indicators | Optional icons showing relative speed or capability tier |

> The multiplier values shown here are illustrative. Always check the live `/model` output for current values, as GitHub adjusts pricing over time.

---

## 9. Quick Reference

```
Check current model:       > /model
Check session usage:       > /usage
Set model (this session):  copilot --model MODEL-ID
Set model (global):        export COPILOT_MODEL=MODEL-ID

Cheapest model:            Claude Haiku 4.5 or GPT-5.4 mini (~0.33x)
Best reasoning:            Claude Opus 4.7
Latest Opus:               Claude Opus 4.7
Latest Sonnet:             Claude Sonnet 4.6
Safe default:              Claude Sonnet 4.5 (1x)
Code-specialized:          GPT-5.3-Codex or GPT-5.2-Codex
Research agent:            Fixed model (not configurable via /model)

Cap autopilot steps:       copilot --max-autopilot-continues N
CI/CD default:             COPILOT_MODEL=claude-haiku-4.5
Switch mid-session:        > /model claude-sonnet-4.5
```

---

**Next:** [Best Practices](10-best-practices.md)
**Previous:** [Team Setup](21-team-setup.md)
**Related:** [CI/CD and Automation](20-cicd-automation.md) | [Autopilot Mode](17-autopilot-mode.md) | [Fleet Mode](18-fleet-mode.md)
