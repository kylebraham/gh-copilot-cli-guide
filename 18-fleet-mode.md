# Fleet Mode

Fleet mode lets Copilot CLI break a complex request into smaller, independent subtasks and run them **in parallel** using subagents. Where autopilot handles one task end-to-end, fleet handles many tasks at the same time — maximising speed and throughput on large, multi-part work.

## Table of Contents

1. [How Fleet Mode Works](#how-fleet-mode-works)
2. [Fleet vs Autopilot](#fleet-vs-autopilot)
3. [Using the `/fleet` Command](#using-the-fleet-command)
4. [Accepting a Plan with Fleet](#accepting-a-plan-with-fleet)
5. [Monitoring Subagents with `/tasks`](#monitoring-subagents-with-tasks)
6. [Specialisation: Custom Agents and Models](#specialisation-custom-agents-and-models)
7. [Premium Request Usage](#premium-request-usage)
8. [When to Use (and Avoid) Fleet](#when-to-use-and-avoid-fleet)
9. [Examples](#examples)
10. [Quick Reference](#quick-reference)

---

## How Fleet Mode Works

When you use `/fleet`, the **main Copilot agent acts as an orchestrator**:

1. It analyses your prompt and identifies subtasks
2. It assesses which subtasks are independent (can run in parallel) and which have dependencies (must run sequentially)
3. It spawns subagents and assigns each one a subtask
4. Subagents run concurrently where possible, reporting results back to the orchestrator
5. The orchestrator integrates the results and reports back to you

```
Your prompt
     │
     ▼
┌─────────────────────────────────────────────────┐
│              Main Agent (Orchestrator)           │
│  Decomposes task → manages deps → merges results │
└──────────┬───────────────┬───────────────┬───────┘
           ▼               ▼               ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │ Subagent │    │ Subagent │    │ Subagent │
    │  Task A  │    │  Task B  │    │  Task C  │
    └──────────┘    └──────────┘    └──────────┘
    (own context)   (own context)   (own context)
```

**Key properties of subagents:**
- Each has its own isolated context window — no cross-talk or context bleed between subtasks
- Each can use tools, read/write files, and run commands independently
- Each can be assigned a different AI model or custom agent profile
- By default, subagents use a low-cost model to keep costs reasonable

---

## Fleet vs Autopilot

These are complementary features, not alternatives. Understanding the distinction helps you choose the right tool:

| | Autopilot | Fleet |
|---|-----------|-------|
| **Core idea** | One agent, works end-to-end without pausing | Multiple agents, work in parallel |
| **Speed benefit** | Removes human wait time between steps | Reduces total wall-clock time by parallelising |
| **Best for** | Sequential tasks with a clear finish line | Tasks that can be decomposed into independent pieces |
| **Activated by** | `Shift+Tab` to cycle modes | `/fleet` slash command in your prompt |
| **Used together?** | ✅ Yes — plan → accept with "autopilot + /fleet" | ✅ Yes — same workflow |

You can combine them: start a plan, then choose **Accept plan and build on autopilot + /fleet** to get both full autonomy *and* parallel execution.

---

## Using the `/fleet` Command

Prefix any prompt with `/fleet` to instruct Copilot to use subagents:

```
> /fleet <your prompt here>
```

### Basic syntax

```
> /fleet Add unit tests for every service file in src/services/

> /fleet Refactor all API handlers to use async/await and update their tests

> /fleet implement the plan in plan.md
```

Copilot will decompose the request, determine which parts can run in parallel, and report back as subagents complete their work.

### Combining with other options

```bash
# Start a session with full permissions and a step cap, then use fleet
copilot --allow-all --max-autopilot-continues 30
```

```
> /fleet Refactor the payment module, add JSDoc to all helpers,
         and generate an updated API reference in docs/
```

---

## Accepting a Plan with Fleet

The most common and recommended workflow is to build a plan first, then execute it with fleet:

1. Press **Shift+Tab** to switch to **plan mode**
2. Describe the work; Copilot drafts `plan.md`
3. Review the plan and iterate until it looks right
4. When the plan is accepted, Copilot presents options:

```
Plan complete. How would you like to proceed?

  1. Accept plan and build on autopilot + /fleet   ← parallel + autonomous
  2. Accept plan and build on autopilot            ← autonomous, sequential
  3. Exit plan mode and I will prompt myself       ← manual control
```

Choosing option **1** launches subagents immediately, in autopilot mode — no further input required until everything is done.

If you select option **3** (manual control), you can still use fleet yourself:

```
> /fleet implement the plan
```

---

## Monitoring Subagents with `/tasks`

While fleet is running, use `/tasks` to see the status of all subagents:

```
> /tasks
```

Example output:

```
Background Tasks
────────────────────────────────────────────────────
  [1] ✅  Add tests: src/services/auth.service.js       (done)
  [2] ✅  Add tests: src/services/user.service.js       (done)
  [3] 🔄  Add tests: src/services/payment.service.js   (in progress)
  [4] 🔄  Add tests: src/services/email.service.js     (in progress)
  [5] ⏳  Update docs/api-reference.md                 (queued)
────────────────────────────────────────────────────
Press ↑↓ to navigate  |  Enter: view details  |  k: kill  |  r: remove  |  Esc: exit
```

### Task management keys

| Key | Action |
|-----|--------|
| `↑` / `↓` | Navigate the task list |
| `Enter` | View details / summary for a subtask |
| `k` | Kill a running subtask |
| `r` | Remove completed or killed subtasks from the list |
| `Esc` | Exit the task list, return to the main prompt |

---

## Specialisation: Custom Agents and Models

One of the most powerful aspects of fleet mode is the ability to direct specific subtasks to specific models or custom agents.

### Specifying models per subtask

Within a `/fleet` prompt, you can name the model for each piece of work:

```
> /fleet Use Claude Opus 4.6 to analyze the security posture of src/auth/,
         use GPT-5.3-Codex to refactor src/api/ to async/await,
         and use the default model to update the README.
```

Each subagent will use the model you specified for its portion of the work.

### Using custom agents

If you have custom agents defined (via `/agent`), reference them with `@agent-name`:

```
> /fleet Use @test-writer to add comprehensive unit tests for every
         file in src/services/,
         and use @doc-generator to create JSDoc comments for src/utils/.
```

For more on defining custom agents, see [Advanced Features](08-advanced-features.md).

### Default subagent model

When you don't specify a model, subagents default to a **low-cost model** to minimise premium request usage. If you need higher-quality results for a subtask, specify a model explicitly.

---

## Premium Request Usage

Each subagent interacts with the AI model independently, so fleet mode can consume **more premium requests** than a single sequential run.

- The main (orchestrator) agent uses requests for planning and coordination
- Each subagent uses requests for its own subtasks
- Model multipliers still apply — higher-cost models consume more per interaction
- Subagents default to a low-cost model to mitigate this

**Practical guidance:**
- Use `/model` to check the current model and its multiplier before launching fleet
- Specify low-cost models for routine subtasks (e.g., formatting, docs) and higher-cost models only where quality matters
- Monitor cumulative usage with `/usage`

---

## When to Use (and Avoid) Fleet

### ✅ Good fits for fleet

- **Test generation across many files** — each file's tests are independent
- **Multi-file refactoring** — updating unrelated modules simultaneously
- **Parallel documentation** — generating JSDoc/docstrings across services
- **Dependency updates** — updating and testing multiple packages at once
- **Executing a large implementation plan** — many plan steps with few dependencies
- **Concurrent code review and fixes** — analyse and patch multiple components

### ❌ Poor fits for fleet

- **Strictly sequential tasks** — where step B requires the output of step A
- **Single-file changes** — no parallelism to exploit
- **Tight budget constraints** — fleet may use more premium requests than a single agent
- **Ambiguous prompts** — the orchestrator needs clear task boundaries to decompose work effectively

> **Rule of thumb:** If you could give the same work to two different developers without them needing to talk to each other, it's a good candidate for `/fleet`.

---

## Examples

### Example 1: Test generation across a service layer

```
> /fleet Add comprehensive unit tests for every file in src/services/.
         Each test file should sit alongside the source file,
         use Vitest, and cover all exported functions including error paths.

[Orchestrator decomposes into one subtask per service file]

Background Tasks
  [1] 🔄  Write tests: auth.service.js       (in progress)
  [2] 🔄  Write tests: user.service.js       (in progress)
  [3] 🔄  Write tests: payment.service.js    (in progress)
  [4] 🔄  Write tests: email.service.js      (in progress)
  [5] 🔄  Write tests: order.service.js      (in progress)

[All 5 run concurrently]

  [1] ✅  Write tests: auth.service.js       (done — 12 tests)
  [2] ✅  Write tests: user.service.js       (done — 9 tests)
  [3] ✅  Write tests: payment.service.js    (done — 15 tests)
  [4] ✅  Write tests: email.service.js      (done — 7 tests)
  [5] ✅  Write tests: order.service.js      (done — 11 tests)

✅ All subtasks complete. 54 new tests added across 5 service files.
```

---

### Example 2: Plan + fleet for a new feature

```bash
copilot --allow-all --max-autopilot-continues 30
```

```
# Step 1: draft a plan
[plan] > Add a notifications system: a notifications model,
         a REST API for CRUD operations,
         an email delivery service,
         and end-to-end tests for the API.

AI: Drafting plan.md...

Plan:
  1. Create src/models/notification.js
  2. Create src/api/notifications.js (CRUD routes)
  3. Create src/services/notification-email.service.js
  4. Create tests/api/notifications.test.js

Plan complete. How would you like to proceed?
  1. Accept plan and build on autopilot + /fleet
  2. Accept plan and build on autopilot
  3. Exit plan mode and I will prompt myself

> 1

[Orchestrator launches subagents for steps 1–3 in parallel]
[Step 4 (tests) queued — depends on steps 1–2 completing first]

Background Tasks
  [1] ✅  Create notifications model          (done)
  [2] ✅  Create notifications API routes     (done)
  [3] ✅  Create email delivery service       (done)
  [4] 🔄  Write end-to-end API tests          (in progress)
  [4] ✅  Write end-to-end API tests          (done — 18 tests, all passing)

✅ Notifications system complete.
```

---

### Example 3: Mixed models for specialised subtasks

```
> /fleet Use Claude Opus 4.6 to audit src/auth/ for security vulnerabilities
         and produce a report in docs/security-audit.md,
         use GPT-5.3-Codex to refactor src/api/ to follow REST conventions,
         and use @doc-generator to add JSDoc comments to src/utils/.

[3 subagents launch simultaneously, each with its assigned model/agent]

Background Tasks
  [1] 🔄  Security audit (Claude Opus 4.6)    (in progress)
  [2] 🔄  API refactor (GPT-5.3-Codex)        (in progress)
  [3] 🔄  JSDoc generation (@doc-generator)   (in progress)

  [3] ✅  JSDoc generation                    (done — 24 functions documented)
  [2] ✅  API refactor                        (done — 8 files updated)
  [1] ✅  Security audit                      (done — report saved to docs/security-audit.md)

✅ All subtasks complete.
```

---

### Example 4: Manual fleet after plan mode

```
[plan] > Migrate the database layer from raw SQL to an ORM (Prisma).
         Update all models, queries, and integration tests.

[Plan drafted and reviewed]

Plan complete. How would you like to proceed?
  3. Exit plan mode and I will prompt myself

> 3

[interactive] > /fleet implement the plan

[Orchestrator identifies which plan steps are independent]
[Runs model migration, query updates, and schema generation in parallel]
[Queues integration test updates until models are complete]

Background Tasks
  [1] ✅  Generate Prisma schema from existing DB     (done)
  [2] ✅  Update src/models/ to use Prisma client     (done)
  [3] ✅  Refactor src/queries/ to Prisma syntax      (done)
  [4] 🔄  Update integration tests                   (in progress)
  [4] ✅  Update integration tests                   (done — all 42 passing)

✅ ORM migration complete.
```

---

## Quick Reference

```
Launch fleet:
  > /fleet <prompt>
  > /fleet implement the plan

Plan → fleet (recommended workflow):
  Shift+Tab                     Switch to plan mode
  [draft and approve plan]
  "Accept plan and build on autopilot + /fleet"

Monitor subagents:
  > /tasks
  ↑↓          Navigate tasks
  Enter       View subtask details
  k           Kill a subtask
  r           Remove completed/killed tasks
  Esc         Exit task list

Specify models per subtask:
  > /fleet Use Claude Opus 4.6 to ... use GPT-5.3-Codex to ...

Use custom agents:
  > /fleet Use @test-writer to ... and @doc-generator to ...

Check usage:
  > /usage
  > /model    (see current model multiplier)

Review all changes when done:
  > /diff
  > /review
```

---

**Next:** [Latest Features](16-new-features.md)  
**Previous:** [Autopilot Mode](17-autopilot-mode.md)  
**Related:** [Plan Mode](09-plan-mode.md) | [Advanced Features](08-advanced-features.md) | [Best Practices](10-best-practices.md)
