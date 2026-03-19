# The `/research` Command

The `/research` command turns Copilot into a **specialized research agent** that gathers and synthesises information from your codebase, GitHub repositories, and the web — producing a comprehensive, cited Markdown report rather than a quick chat reply.

It is not a mode (unlike autopilot or plan). It is a slash command you invoke for deep investigation work.

## Table of Contents

1. [How `/research` Differs from Normal Chat](#how-research-differs-from-normal-chat)
2. [Basic Usage](#basic-usage)
3. [How the Research Agent Works](#how-the-research-agent-works)
4. [Query Types and Report Formats](#query-types-and-report-formats)
5. [Viewing Your Report](#viewing-your-report)
6. [Sharing Reports](#sharing-reports)
7. [Finding Past Reports](#finding-past-reports)
8. [When to Use (and Avoid) `/research`](#when-to-use-and-avoid-research)
9. [Example Prompts with Explanations](#example-prompts-with-explanations)
10. [Quick Reference](#quick-reference)

---

## How `/research` Differs from Normal Chat

| | Normal chat | `/research` |
|--|-------------|-------------|
| **Goal** | Quick, focused reply | Exhaustive, cited report |
| **Length** | A few sentences to paragraphs | Hundreds of lines — architecture diagrams, code snippets, citations |
| **Output** | Ephemeral chat message | Persistent Markdown file saved to disk |
| **Clarifying questions** | May ask | Never — documents assumptions instead |
| **Interrupts your flow** | No | No — runs to completion, reports back |
| **Makes code changes** | Yes | No — report only |
| **Model** | Your currently selected model | Fixed research model (not configurable) |
| **Best for** | "Fix this bug", "explain this function" | Architecture overviews, technology comparisons, deep-dives |

---

## Basic Usage

```
> /research TOPIC
```

Where `TOPIC` is a natural language question or description:

```
> /research What is the architecture of this codebase?

> /research How does React implement concurrent rendering?

> /research What are best practices for rate limiting in Node.js APIs?

> /research How are feature flags implemented at our organization?
```

When research completes, Copilot shows a brief summary in the CLI and provides a link to the full Markdown report:

```
> /research How is session management implemented in this repo?

Research complete.

Key findings:
  • Sessions stored in Redis using connect-redis middleware (src/cache/session.js)
  • Session TTL: 24h (configurable via SESSION_TTL env var)
  • Refresh tokens handled separately in src/auth/tokens.js
  • 3 session-related security considerations documented

Full report: ~/.copilot/session-state/abc123.../research/session-management.md

Press Ctrl+Y to open the report, or use /share to export it.
```

---

## How the Research Agent Works

The `/research` agent is a **built-in custom agent** with a fixed model and specific tooling:

1. **Classifies the query** — determines whether it's a process question, conceptual question, or technical deep-dive, and adapts the report format accordingly
2. **Searches your codebase** — uses `grep`, `glob`, and `view` tools scoped to your working directory
3. **Searches GitHub** — can query public and accessible private repositories, including org-scoped searches (`org:yourorg`)
4. **Searches the web** — fetches and synthesises information from external sources
5. **Prioritises internal implementations** — explicitly looks for internal/private implementations before falling back to public documentation
6. **Never interrupts** — makes reasonable assumptions, documents them in a "Confidence Assessment" section of the report
7. **Writes the report** — saves a full Markdown file, then delivers a summary to the CLI

> **Note:** The research agent uses a fixed, hard-coded AI model regardless of what you have selected with `/model`. This model is optimised for research tasks and cannot be changed.

---

## Query Types and Report Formats

The agent classifies every prompt into one of three types and tailors the report:

| Query type | Example | Report style |
|------------|---------|--------------|
| **Process / how-to** | "How do I add an API endpoint?" | Step-by-step instructions, links to relevant docs and contacts |
| **Conceptual / explanatory** | "What's the difference between JWT and session auth?" | Narrative explanation, trade-off tables, design decision context |
| **Technical deep-dive** | "How is the auth system implemented?" | Architecture diagrams, component breakdowns, data-flow descriptions, file paths with line numbers |

### Guiding the report type

If you want a technical deep-dive but phrase the question as "What is X?", the agent may produce a conceptual answer. Be explicit to get the format you want:

```
# More likely to get a conceptual answer:
> /research What is the session management system?

# More likely to get a technical deep-dive:
> /research Give me a technical deep-dive into how the session management
  system is implemented, with architecture diagrams and code examples.
```

---

## Viewing Your Report

### Open in editor — `Ctrl+Y`

After research completes, press **Ctrl+Y** to open the most recent report in your terminal editor.

The editor used is determined by environment variables (checked in order):
1. `COPILOT_EDITOR`
2. `VISUAL`
3. `EDITOR`
4. Falls back to `vi` (Linux) or `vim` (macOS)

```bash
# Set your preferred editor before launching the CLI
export COPILOT_EDITOR=code   # VS Code
export COPILOT_EDITOR=nano
export EDITOR=nvim
```

### Open via the link

When research completes, Copilot provides a direct file path. Open it however you like:

```bash
# Open in VS Code
code ~/.copilot/session-state/abc123.../research/session-management.md

# Open in the browser (macOS)
open ~/.copilot/session-state/abc123.../research/session-management.md
```

---

## Sharing Reports

### Save to a file

```
> /share file research
```

Copilot lists the research reports from the current session — use `↑↓` to select one, press Enter. The file is saved to the current working directory with a name based on the research topic.

To specify a path:

```
> /share file research docs/architecture-research.md
```

### Publish to a GitHub Gist

```
> /share gist research
```

Copilot lists reports from the current session, you select one, and a **secret GitHub Gist** is created. The URL is shown in the CLI:

```
✅ Gist created: https://gist.github.com/user/a1b2c3d4e5f6...
```

---

## Finding Past Reports

Research reports are tied to the session in which they were created. The `Ctrl+Y` shortcut and `/share` command only surface reports from the **current session**.

Reports from previous sessions are still on disk:

```bash
# List your 10 most recent session directories (Linux/macOS)
ls -dtl ~/.copilot/session-state/*/ | head -10

# Reports are inside each session folder
ls ~/.copilot/session-state/<SESSION-ID>/research/
```

You can open them directly in any editor — they are plain Markdown files.

---

## When to Use (and Avoid) `/research`

### ✅ Great fits for `/research`

- **Understanding an unfamiliar codebase** — architecture overview before making large changes
- **Comparing technologies** — "Should we use Redis or Memcached for caching?"
- **How a library/framework works internally** — "How does Prisma handle migrations?"
- **Onboarding** — "How does the authentication system work in this project?"
- **Pre-refactor investigation** — understand the current shape before changing it
- **Cross-repo pattern research** — "How do other teams in our org handle error boundaries?"
- **Security audits** — "What authentication patterns are used across our services?"
- **Dependency evaluation** — "What are the trade-offs of adopting Turbopack?"

### ❌ Poor fits for `/research`

- **Quick questions** — "What does this function return?" — just ask in normal chat
- **Bug fixes or code changes** — `/research` produces reports, not edits; use a normal prompt or plan mode
- **Time-sensitive questions** — research makes many tool calls and takes longer than a normal response
- **Iterative conversations** — if you expect to go back and forth, normal chat is better

---

## Example Prompts with Explanations

### Codebase architecture

```
> /research What is the architecture of this codebase?
```

The agent explores the full project tree, reads key files, and synthesises an overview — producing architecture diagrams, component breakdowns, and data-flow descriptions. Much more thorough than asking in chat.

---

### Internal feature flag implementation

```
> /research How are feature flags implemented at our organization?
```

The agent searches with `org:YOURORG` queries, prioritises internal implementations over public docs, and looks for naming patterns like `-hub`, `-service`, `-client`. Perfect for understanding cross-repo conventions.

---

### Technology deep-dive

```
> /research How does React implement concurrent rendering?
```

The agent fetches information from the web and reads actual React source code on GitHub, prioritising source code over documentation.

---

### Comparing approaches

```
> /research What's the difference between JWT and session-based authentication?
  Include a trade-off table and design decision guidance.
```

The agent produces a narrative explanation with context, a comparison table, and design decision guidance. Explicitly asking for a table ensures you get one.

---

### How-to / process

```
> /research How do I add a new API endpoint to this service?
```

The agent detects this as a process question and produces step-by-step guidance with links to relevant files, patterns in use, and any team conventions it finds.

---

### Pre-refactor investigation

```
> /research Give me a technical deep-dive into how the payment module is
  structured, including all dependencies, data flows, and error handling
  patterns. Include architecture diagrams.
```

By explicitly requesting a deep-dive and diagrams, you ensure the agent produces the most detailed possible report — ideal before a major refactor.

---

### Share after researching

```
> /research How is rate limiting implemented across our API services?

[Research completes]

> /share gist research

✅ Gist: https://gist.github.com/user/abc123...
```

---

## Quick Reference

```
Run research:
  > /research TOPIC

Open latest report in editor:
  Ctrl+Y

Share research report:
  > /share gist research        Create a secret GitHub Gist
  > /share file research        Save to current directory
  > /share file research PATH   Save to specific path

Find past reports on disk:
  ls -dtl ~/.copilot/session-state/*/ | head -10
  ls ~/.copilot/session-state/<SESSION-ID>/research/

Set your preferred editor (add to shell profile):
  export COPILOT_EDITOR=code    VS Code
  export COPILOT_EDITOR=nano
  export EDITOR=nvim

Query type hints:
  Process question    → "How do I..."
  Conceptual          → "What is the difference between..."
  Technical deep-dive → "Give me a technical deep-dive into...
                         with architecture diagrams and code examples."
```

---

**Next:** [Latest Features](16-new-features.md)  
**Previous:** [Fleet Mode](18-fleet-mode.md)  
**Related:** [Plan Mode](09-plan-mode.md) | [Autopilot Mode](17-autopilot-mode.md) | [GitHub Integration](07-github-integration.md)
