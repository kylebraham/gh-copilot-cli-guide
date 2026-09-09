# Autopilot Mode

Autopilot mode lets Copilot CLI work **autonomously** on a task — carrying out every step from start to finish without pausing for your input after each action. Give the initial instruction, then let Copilot do the work.

## Table of Contents

1. [How Autopilot Differs from Interactive Mode](#how-autopilot-differs-from-interactive-mode)
2. [Enabling Experimental Features](#enabling-experimental-features)
3. [Activating Autopilot Mode](#activating-autopilot-mode)
4. [Permissions Prompt](#permissions-prompt)
5. [Stopping Autopilot](#stopping-autopilot)
6. [Limiting Autonomous Steps](#limiting-autonomous-steps)
7. [Autopilot vs `--allow-all` vs `--no-ask-user`](#autopilot-vs---allow-all-vs---no-ask-user)
8. [Premium Request Usage](#premium-request-usage)
9. [Typical Workflow](#typical-workflow)
10. [Examples](#examples)
11. [When to Use (and Avoid) Autopilot](#when-to-use-and-avoid-autopilot)
12. [Quick Reference](#quick-reference)

---

## How Autopilot Differs from Interactive Mode

In the standard **interactive** mode, Copilot responds to each message, then waits. You review each response and give the next instruction. It is like pair programming — the AI does most of the work, but checks back with you at every step.

In **autopilot** mode, you hand the whole task over. Copilot plans, edits, runs commands, fixes errors, and iterates until it determines the task is complete — all without asking for your input between steps.

Think of it as the difference between working *with* a colleague versus delegating a task *to* them and asking them to report back when finished.

| Aspect | Interactive | Autopilot |
|--------|-------------|-----------|
| Pauses for input | After every response | Only at the very start |
| Confirmation prompts | Yes | No (if full permissions granted) |
| Best for | Exploratory or creative work | Well-defined, multi-step tasks |
| Premium request usage | You initiate each step | Steps happen automatically |

---

## Enabling Experimental Features

Autopilot mode is gated behind **experimental mode**. Enable it once and the setting persists in your config:

```bash
# Start the CLI with experimental features enabled
copilot --experimental
```

Or from inside a running session:

```
> /experimental
```

You can check which experimental features are currently active, or turn experimental mode back off, at any time:

```
> /experimental        # Toggle on/off, or show current status
```

---

## Activating Autopilot Mode

Use **Shift+Tab** to cycle through the three interaction modes:

```
interactive  →  plan  →  autopilot  →  (back to interactive)
```

The active mode is shown in the input prompt footer. Once you see `autopilot`, type your task and press Enter.

```
# The footer indicator changes as you press Shift+Tab
[interactive] > _
[plan]        > _
[autopilot]   > _
```

> **v1.0.45+:** Use the `/autopilot` slash command to toggle directly into (or out of) autopilot mode without cycling through all modes with Shift+Tab.

> **v1.0.55+:** Pass an objective to `/autopilot` (or use the `/goal` alias) to keep the session anchored to a specific task:
> ```
> > /autopilot Migrate all REST endpoints to use the new authentication middleware
> > /goal Write unit tests for the UserService class
> ```

You can also start the CLI directly in autopilot mode from the command line:

```bash
copilot --autopilot "Your task description here"
```

> **v1.0.79+:** Combine `--plan` with `--mode autopilot` to plan the task first, then implement it automatically without waiting for approval to enter autopilot — useful for non-interactive runs that should both plan and execute unattended. See [Plan Mode — Method 4](09-plan-mode.md#method-4-combine---plan-with---mode-autopilot-v1079).

> **v1.0.80+:** Setting an explicit objective with `/autopilot <objective>` or `/goal <objective>` no longer requires experimental mode. You still need experimental mode enabled to enter autopilot mode itself — this change only lifts the requirement for anchoring an objective once you're in it.

---

## Permissions Prompt

When you first enter autopilot mode in a session (and have not already granted full permissions), the CLI displays a prompt:

```
Autopilot mode requires permissions to act autonomously.

  1. Enable all permissions (recommended)
  2. Continue with limited permissions
  3. Cancel (Esc)
```

**Option 1 — Enable all permissions:** Copilot can use any tool, read/write any path, and access any URL. This is equivalent to running `copilot --allow-all`. Recommended for best results.

**Option 2 — Continue with limited permissions:** Copilot will automatically deny any tool call that requires approval, which may prevent it from completing certain tasks.

You can grant full permissions at any point during an autopilot session with:

```
> /allow-all
```

> **v1.0.44+:** Tool permissions granted during an autopilot session are now **preserved after `/clear`**. Previously, running `/clear` reset the permission state and you had to re-approve on the next turn.

> **v1.0.46+:** Read-only `gh` CLI commands (`gh issue list`, `gh pr view`, `gh repo status`, `gh pr diff`, etc.) are **auto-approved** in autopilot mode — no confirmation prompt is shown. Only write operations still require approval.

> **v1.0.77+:** Choosing "Enable all permissions" (or running with `--allow-all`) now also **disables the sandbox** for the rest of the current session, when your sandbox policy allows bypass. Previously, granting full tool approval didn't automatically lift sandbox restrictions, which could still silently constrain what autopilot could do.

---

## Stopping Autopilot

Autopilot runs until one of these conditions is met:

| Condition | Description |
|-----------|-------------|
| Task complete | The agent determines it has finished |
| Unresolvable error | A blocker it cannot work around |
| `Ctrl+C` or `Esc` | You press Ctrl+C or Escape to cancel |
| Continuation limit | `--max-autopilot-continues` count reached |

> **v1.0.64:** When the agent calls `task_complete`, autopilot mode **automatically returns to interactive mode**. Previously the session stayed in autopilot and your next prompt would trigger another autonomous run. Now you are returned to interactive mode to review the results before proceeding.

> **v1.0.69+:** Set `"stayInAutopilot": true` in `~/.copilot/settings.json` to keep the CLI in autopilot mode after a task completes instead of automatically returning to interactive mode. Defaults to `false` (the v1.0.64 behavior described above).

> **v1.0.76:** The default flipped — autopilot now **stays selected** after `task_complete` by default. Set `"stayInAutopilot": false` in `~/.copilot/settings.json` to restore the v1.0.64 behavior of returning to interactive mode after each task.

> **v1.0.76:** Resuming a session (`/resume`) now restores whichever mode it was in — autopilot or Plan Mode — instead of reverting to interactive mode. This keeps the autopilot-only `task_complete` tool available and ensures the mode matches the session you left.

> **v1.0.83+:** A follow-up prompt typed while autopilot is running no longer disappears from the timeline. The collapsed autopilot goal panel now reads as a single-line pinned prompt, keeping the frame it shares with a pinned prompt instead of compressing into a bare band wedged against the chrome above it.

> **v1.0.64:** Autopilot now auto-handles elicitation, `ask_user`, sampling, and permission prompts — including prompts shown at launch with `--autopilot` and during continuation turns — so they no longer surface dialogs during an autonomous run.

Pressing **Ctrl+C** or **Escape** cleanly stops the current autonomous step and autopilot will not resume (v1.0.15+). You can then review what was done, switch back to interactive mode with Shift+Tab, and continue manually.

---

## Limiting Autonomous Steps

To prevent runaway execution, set a maximum number of continuation steps:

```bash
copilot --max-autopilot-continues 10
```

> **v1.0.40+:** Autopilot mode applies a **default limit of 5 autonomous continuations** when no `--max-autopilot-continues` value is specified. To raise or remove the limit:
>
> ```bash
> copilot --max-autopilot-continues 20   # raise the limit
> copilot --max-autopilot-continues 0    # remove the limit entirely
> ```

Each time Copilot continues autonomously, the CLI displays a message like:

```
Continuing autonomously (3 premium requests)
```

This helps you track how much work the agent is doing and how many requests are being consumed.

Combine with `--allow-all` for a fully autonomous, bounded session:

```bash
copilot --allow-all --max-autopilot-continues 15
```

### Disabling Allow-All / Yolo Mode

> **v1.0.55+:** To prevent users from entering allow-all or yolo mode (e.g. in CI or team environments), set `permissions.disableBypassPermissionsMode` in `~/.copilot/settings.json`:
>
> ```json
> {
>   "permissions": {
>     "disableBypassPermissionsMode": true
>   }
> }
> ```
>
> When this setting is enabled, `/allow-all` and `/yolo` commands are disabled and the permissions prompt in autopilot mode will not offer the "Enable all permissions" option.

---

## Autopilot vs `--allow-all` vs `--no-ask-user`

These three options are related but distinct:

| Option | Permissions | Autonomy | Continues without input? |
|--------|-------------|----------|--------------------------|
| Default interactive | Asks per tool | None | No |
| `--allow-all` | All granted | None | No — still pauses for your replies |
| `--no-ask-user` | Asks per tool | Partial | Skips clarifying questions |
| **Autopilot mode** | All granted (recommended) | Full | **Yes — works end-to-end** |

- **`--allow-all`** removes *permission* gates but does not remove *interaction* gates. Copilot still stops and waits for your next message.
- **`--no-ask-user`** suppresses clarifying questions so the agent makes its own decisions. However, it does not allow the agent to continue through multiple AI-model interactions — additional premium requests are not used without your direct involvement.
- **Autopilot mode** enables both: no permission gates and no interaction gates. The agent drives the session from start to finish.

---

## Premium Request Usage

Autopilot uses premium requests the same way the interactive mode does — one request per model interaction. The difference is that in autopilot mode, those interactions happen automatically without you submitting each one.

- Each autonomous continuation displays the request count: `Continuing autonomously (3 premium requests)`
- The count reflects the model multiplier for the currently selected model
- Use `/model` to check your current model and its multiplier
- Use `--max-autopilot-continues` to cap total autonomous steps
- Check cumulative session usage at any time with `/usage`

---

## Typical Workflow

Autopilot mode is most effective when paired with **plan mode**. A recommended pattern:

1. **Start a session** with appropriate flags:
   ```bash
   copilot --allow-all --max-autopilot-continues 20
   ```

2. **Use plan mode** (Shift+Tab) to let Copilot draft an implementation plan before touching any code:
   ```
   [plan] > Refactor the authentication module to use refresh tokens,
            update all related tests, and fix any lint errors.
   ```
   Review `plan.md`, edit it if needed, then confirm.

3. **Switch to autopilot mode** (Shift+Tab again) and submit the same (or a refined) prompt:
   ```
   [autopilot] > Implement the plan in plan.md
   ```
   Copilot works through every step autonomously.

4. **Review the result** when Copilot reports completion:
   ```
   > /diff          # See all changes made
   > /review        # Run the code review agent
   > !git diff      # Raw diff if preferred
   ```

5. **Open a PR** when satisfied:
   ```
   > /delegate Refactor auth module to use refresh tokens
   ```

---

## Examples

### Example 1: End-to-End Feature Implementation

```
[autopilot] > Add a password reset flow to the auth module.
              It should: send an email with a time-limited token,
              validate the token on the reset page,
              update the password, and invalidate the token.
              Include unit tests for all new functions.

Continuing autonomously (2 premium requests)

🔧 Creating src/auth/password-reset.js...
🔧 Creating src/email/password-reset-template.js...
🔧 Updating src/routes/auth.js to add /forgot-password and /reset-password...
🧪 Running: npm test...
  ✅ 14/14 tests passing

Continuing autonomously (3 premium requests)

🔧 Fixing lint warning in src/auth/password-reset.js (line 42)...
🧪 Re-running tests... ✅ All passing

✅ Task complete. Password reset flow implemented with tests.
```

---

### Example 2: Large-Scale Refactoring

```
[autopilot] > Migrate the entire codebase from CommonJS (require/module.exports)
              to ES modules (import/export). Update package.json accordingly
              and fix any broken imports.

Continuing autonomously (2 premium requests)

📂 Scanning for CommonJS patterns across 47 files...
🔧 Updating package.json ("type": "module")...
🔧 Converting src/app.js...
🔧 Converting src/routes/users.js...
🔧 Converting src/utils/logger.js...
... (44 more files)

Continuing autonomously (5 premium requests)

🧪 Running: npm test...
  ❌ 2 tests failing — import path issue in tests/auth.test.js

🔧 Fixing import paths in tests/auth.test.js...
🧪 Re-running: npm test...
  ✅ All 38 tests passing

✅ Task complete. All 47 files migrated to ES modules.
```

---

### Example 3: CI Fix Workflow

```
[autopilot] > Our CI pipeline is failing on the main branch.
              Look at the recent test failures, diagnose the root cause,
              and fix the code so all tests pass.

Continuing autonomously (2 premium requests)

🔍 Reading recent CI logs...
🔍 Running: npm test locally...
  ❌ 3 failures in tests/api/orders.test.js

Continuing autonomously (3 premium requests)

🔍 Analyzing failures — root cause: null-check missing in src/api/orders.js (line 87)
🔧 Adding null-check...
🧪 Re-running: npm test...
  ✅ All 52 tests passing

✅ Task complete. Null-check added; all CI tests now pass.
```

---

### Example 4: Combined Plan + Autopilot

```bash
# Start with permissions and a step cap
copilot --allow-all --max-autopilot-continues 25
```

```
# Step 1: plan mode — get the plan approved
[plan] > Add rate limiting to all public API routes using Redis.
         Use the existing Redis client in src/cache/client.js.

AI: Drafting plan.md...

[Review and approve plan.md]

# Step 2: autopilot mode — execute the plan
[autopilot] > Implement the plan in plan.md

Continuing autonomously (2 premium requests)
... [agent works through all plan steps]

✅ Task complete. Rate limiting added to 12 routes with Redis.
```

---

## When to Use (and Avoid) Autopilot

### ✅ Good fits for autopilot

- **Large, well-specified refactors** — migrating patterns across many files
- **End-to-end feature implementation** — a feature with a clear specification
- **Test generation** — write tests for an entire module or service
- **CI/build fixing** — diagnose and fix a failing test suite
- **Batch operations** — apply a consistent change to many files
- **Executing a drafted plan** — when you've approved a plan in plan mode

### ❌ Poor fits for autopilot

- **Exploratory or open-ended work** — tasks without a clear success condition
- **Iterative design decisions** — when you want to guide direction along the way
- **Sensitive production configs** — where you want to approve each change
- **Ambiguous instructions** — vague prompts lead to unexpected changes
- **New codebases** — you don't know yet what Copilot might change

> **Tip:** If you're unsure, run in plan mode first. Review and refine the plan before switching to autopilot to execute it.

---

## Critic Agent (Experimental)

Available in v1.0.18+, the **Critic agent** adds an automatic second-opinion review for complex implementations running in autopilot. A complementary model reviews the work in progress and flags potential issues before they are committed — without pausing the agent for your input.

### Requirements

- Experimental mode enabled (`/experimental` or `--experimental`)
- A Claude model active for the session

### How It Works

When autopilot is executing a complex multi-step task, the Critic agent runs asynchronously alongside it. At key decision points — completing a major step, before a destructive operation, after a test failure — the Critic reviews the current state and surfaces concerns in the session timeline.

```
[autopilot] > Migrate the payment module to use Stripe's new SDK

Continuing autonomously (2 premium requests)

🔧 Updating src/payments/stripe-client.js...
🔍 Critic: Warning — stripe.charges.create is deprecated in the new SDK; use stripe.paymentIntents.create instead
🔧 Using stripe.paymentIntents.create as suggested...
🧪 Running: npm test... ✅ All passing

✅ Task complete.
```

### Tips

- ✅ The Critic is most valuable for autopilot runs on unfamiliar codebases
- ✅ Critic notes appear inline in the timeline — review them when the task completes
- ❌ Does not replace running tests — always verify the output with your own test suite

---

## Quick Reference

```
Enable experimental features:
  copilot --experimental
  > /experimental

Cycle modes:
  Shift+Tab     interactive → plan → autopilot → interactive

Start directly in autopilot from the command line:
  copilot --autopilot "task description"

Grant all permissions:
  copilot --allow-all
  > /allow-all

Cap autonomous steps:
  copilot --max-autopilot-continues 10

Stop autopilot mid-task:
  Ctrl+C

Check usage:
  > /usage

Review changes made:
  > /diff
  > /review

Suppress clarifying questions only (not full autopilot):
  copilot --no-ask-user
```

---

**Next:** [Latest Features](16-new-features.md)  
**Previous:** [.copilot Directory Guide](15-copilot-directory.md)  
**Related:** [Plan Mode](09-plan-mode.md) | [Interactive Features](03-interactive-features.md) | [Best Practices](10-best-practices.md)
