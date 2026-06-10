# Latest Features in GitHub Copilot CLI — v1.0.61

This file covers recent additions to GitHub Copilot CLI. Features marked with "Full guide →" have their own dedicated documentation file — the entries here are summaries with links. Features without a dedicated file are covered in full below.

## Table of Contents

### Features with dedicated guides
1. [Autopilot Mode](#autopilot-mode-experimental) — [Full guide →](17-autopilot-mode.md)
2. [Fleet Mode (`/fleet`)](#fleet-mode-fleet) — [Full guide →](18-fleet-mode.md)
3. [Research Command (`/research`)](#research-command-research) — [Full guide →](19-research-command.md)

### Features covered in this file
4. [New in v1.0.61](#new-in-v1061)
5. [New in v1.0.60](#new-in-v1060)
5. [New in v1.0.59](#new-in-v1059)
5. [New in v1.0.58](#new-in-v1058)
5. [New in v1.0.57](#new-in-v1057)
5. [New in v1.0.56](#new-in-v1056)
5. [New in v1.0.55](#new-in-v1055)
5. [New in v1.0.54](#new-in-v1054)
5. [New in v1.0.53](#new-in-v1053)
5. [New in v1.0.52](#new-in-v1052)
5. [New in v1.0.51](#new-in-v1051)
5. [New in v1.0.49](#new-in-v1049)
5. [New in v1.0.48](#new-in-v1048)
5. [New in v1.0.47](#new-in-v1047)
5. [New in v1.0.46](#new-in-v1046)
5. [New in v1.0.45](#new-in-v1045)
5. [New in v1.0.44](#new-in-v1044)
5. [New in v1.0.43](#new-in-v1043)
5. [New in v1.0.42](#new-in-v1042)
6. [New in v1.0.41](#new-in-v1041)
5. [New in v1.0.40](#new-in-v1040)
5. [New in v1.0.39](#new-in-v1039)
5. [New in v1.0.37](#new-in-v1037)
5. [New in v1.0.36](#new-in-v1036)
5. [New in v1.0.35](#new-in-v1035)
5. [New in v1.0.34](#new-in-v1034)
6. [New in v1.0.33](#new-in-v1033)
6. [New in v1.0.32](#new-in-v1032)
5. [New in v1.0.31](#new-in-v1031)
6. [New in v1.0.30](#new-in-v1030)
7. [New in v1.0.29](#new-in-v1029)
8. [New in v1.0.28](#new-in-v1028)
5. [New in v1.0.27](#new-in-v1027)
5. [New in v1.0.26](#new-in-v1026)
6. [New in v1.0.25](#new-in-v1025)
5. [New in v1.0.24](#new-in-v1024)
6. [New in v1.0.23](#new-in-v1023)
7. [New in v1.0.22](#new-in-v1022)
8. [New in v1.0.21](#new-in-v1021)
9. [New in v1.0.20](#new-in-v1020)
10. [New in v1.0.19](#new-in-v1019)
11. [New in v1.0.18](#new-in-v1018)
12. [New in v1.0.17](#new-in-v1017)
13. [New in v1.0.16](#new-in-v1016)
14. [New in v1.0.15](#new-in-v1015)
15. [New in v1.0.13](#new-in-v1013)
16. [New in v1.0.11](#new-in-v1011)
16. [LSP Support](#lsp-language-server-protocol-support)
17. [Code Review Agent (`/review`)](#code-review-agent-review)
18. [Plugin System (`/plugin`)](#plugin-system-plugin)
19. [Keyboard Shortcuts Reference](#keyboard-shortcuts-reference)
20. [PAT Authentication](#pat-authentication)
21. [Extended Instructions Support](#extended-instructions-support)
22. [Project Initialization (`/init`)](#project-initialization-init)
23. [Enhanced Pull Request Creation (`/delegate`)](#enhanced-pull-request-creation-delegate)
24. [Staying Up to Date](#staying-up-to-date)

---

---

## New in v1.0.61

Released: 2026-06-09

### `/settings` Interactive Dialog

A new `/settings` command opens a full interactive dialog to browse and edit all user settings in one place — without manually editing `~/.copilot/settings.json`.

**How to use:**
```
> /settings
```

The dialog lists every available setting with its current value. Navigate with arrow keys, select a setting to edit it, and save on exit.

**Why it matters:** Discover and tweak settings interactively — no JSON editing required.

---

### `/worktree` Command (alias `/move`)

Create a new git worktree and switch into it, carrying any uncommitted changes along. This lets you context-switch between branches without stashing or committing work in progress.

**How to use:**
```
> /worktree new-branch-name
> /move my-experiment
```

**What it does:**
1. Creates a new git worktree for the specified branch (creating the branch if it doesn't exist)
2. Moves uncommitted changes into the new worktree
3. Switches the active working directory to the new worktree

**Why it matters:** Seamlessly branch your in-progress work without losing context or disrupting the current checkout.

---

### Claude Fable 5 Model

Claude Fable 5 is now available as a selectable model.

**How to use:**
```
> /model claude-fable-5
```

**Why it matters:** Expands the available model roster with a new Claude family option.

---

### Auto-load MCP Servers from `.github/mcp.json`

Copilot CLI now automatically loads MCP server definitions from `.github/mcp.json` in the workspace root, in addition to the existing `.mcp.json` file. This makes it easy to commit shared MCP server config alongside GitHub Actions workflows and other `.github/` configuration.

**Example `.github/mcp.json`:**
```json
{
  "mcpServers": {
    "my-server": {
      "command": "npx",
      "args": ["-y", "my-mcp-server"]
    }
  }
}
```

**Why it matters:** Teams can standardize MCP servers by committing `.github/mcp.json` to the repo, and every contributor gets the same server set automatically.

---

### Natural Language Scheduling for `/every` and `/after`

`/every` and `/after` now accept natural language expressions in addition to raw time units — use cron expressions, calendar times, or relative durations.

**How to use:**
```
> /experimental on
> /every "every weekday at 9am" summarize open PRs
> /after "in 2 hours" remind me to deploy the staging build
> /every "0 */4 * * *" run the integration tests
```

Both commands still accept the original `s`/`m`/`h` shorthand syntax.

**Why it matters:** Express schedules naturally without memorising cron syntax.

---

### `beepOnSchedule` Setting

A new `beepOnSchedule` setting (default `true`) controls whether the terminal beeps when a scheduled `/every` or `/after` run completes. Set it to `false` to suppress beeps.

**In `~/.copilot/settings.json`:**
```json
{
  "beepOnSchedule": false
}
```

---

### `tabs` Setting

A new `tabs` setting in `settings.json` lets you configure which tabs appear in the home tab bar, their order, and which are hidden.

**In `~/.copilot/settings.json`:**
```json
{
  "tabs": {
    "order": ["chat", "sessions", "mcp", "settings"],
    "hidden": ["changelog"]
  }
}
```

**Why it matters:** Streamline the UI by hiding tabs you never use and reordering the ones you do.

---

### Additional Improvements

- **`/sessions` navigates to the Sessions tab** instead of opening an overlay
- **Press `/` in the `/agent` picker** to filter agents by name
- **Number-key selection in pickers** works for items 10 and beyond
- **`/env` hides internal hooks** and shows full file paths for hook sources
- **`/help` lists `$HOME/.copilot/instructions/**/*.instructions.md`** alongside other user-level instruction locations
- **`/fork` shows a "Creating fork..." progress notification** while the fork is being created
- **Symlinked directories** now appear in `@`-file picker suggestions
- **Exit shell mode** by pressing `Esc` or `Ctrl+C` on an empty prompt (in addition to `Backspace`)
- **Grep searches in large monorepos** use an indexed search engine for significantly faster results
- **`/agents` picker** polished with consistent borders, headers, and styled inputs
- **Bug fixes:** blank screen on session resume, MCP OAuth re-authentication, pasted image leaks, bash UTF-8 multi-byte handling, nested autolink false positives, WSL/tmux color rendering

---

## New in v1.0.60

Released: 2026-06-05

### Max Reasoning Effort for Anthropic Models

All reasoning effort levels (`low`, `medium`, `high`, `max`) are now available for Anthropic models on every plan. Previously `max` effort was restricted. This lets you unlock the deepest reasoning for complex tasks without a plan upgrade.

**How to use:**
```
> /model claude-opus-4.8
> --reasoning-effort max
```

**Why it matters:** Maximum reasoning depth is now accessible to all subscribers for the most demanding tasks.

---

### `builtInAgents.rubberDuckAutoInvoke` Setting

A new `builtInAgents.rubberDuckAutoInvoke` setting controls whether the Rubber Duck agent automatically invokes itself to critique your plan. It is **disabled by default** — set it to `true` if you want the agent to chime in automatically.

**In `~/.copilot/settings.json`:**
```json
{
  "builtInAgents": {
    "rubberDuck": true,
    "rubberDuckAutoInvoke": true
  }
}
```

**Why it matters:** Opt-in control over automatic critique — useful for teams that want consistent pre-flight reviews without manually triggering `/rubber-duck`.

---

### `/context` — Custom Instructions Separated from System Prompt

`/context` now shows **Custom Instructions** as a distinct section, separate from the base system prompt. It also cross-references per-server MCP tool token costs with `/mcp`, making it easier to understand what is consuming your context budget.

**How to use:**
```
> /context
```

The output now includes a "Custom Instructions" section and links to `/mcp` for per-server tool token breakdowns.

---

### `billing` Help Topic

A new `billing` help topic provides an inline overview of AI credit usage features.

**How to use:**
```
> /help billing
```

Shows how premium requests are tracked, quota limits, and pointers to `/usage` and `/mcp` for detailed breakdowns.

---

### Vim-Style Navigation in `/diff`

The `/diff` view now supports vim-style navigation keys:

| Key | Action |
|-----|--------|
| `g` | Jump to the top of the diff |
| `G` | Jump to the bottom of the diff |
| `Ctrl+D` | Scroll down half a page |
| `Ctrl+U` | Scroll up half a page |

These complement the existing `j` / `k` (line-by-line) navigation.

---

### Create Git Worktree from Pull Requests Screen

From the pull requests list, you can now create a git worktree for a PR directly — no need to run `git worktree add` manually. Useful for reviewing or testing a PR in an isolated directory while keeping your main checkout clean.

When a PR branch name contains slashes (e.g., `cli/foo`), the worktree directory uses a flat name (`cli-foo`) to avoid nested directory issues.

---

### Auto-Link Bare `#number` References

Typing a bare issue or PR reference like `#42` anywhere in the prompt is now automatically linked to the current git repository. No need to spell out the full URL or use a special syntax.

**Example:**
```
> What's the status of #42?

AI: Issue #42 "Add dark mode support" is currently open with 3 comments…
```

---

### `-r` Shorthand for `--resume`

`-r` is now a shorthand flag for `--resume`, making it quicker to resume the last session from the command line:

```bash
copilot -r
# equivalent to: copilot --resume
```

---

### `/env` — Hook Counts and Source Provenance

`/env` now shows **hook counts** and the **source provenance** of each active hook (which file or plugin registered it), making it easier to audit what is running during tool calls.

---

## New in v1.0.59

Released: 2026-06-02

### `/voice` Command

A new `/voice` command lets you dictate prompts using local speech-to-text models. Speak your prompt and Copilot CLI transcribes it and submits it for you — no cloud speech service required.

**How to use:**
```
> /voice
```

Copilot CLI will start listening and transcribe your speech into the prompt field. Confirm or edit the transcription before submitting.

**Why it matters:** Hands-free prompt entry, useful for longer or more natural-language prompts.

---

## New in v1.0.58

Released: 2026-06-02

### Rubber Duck Enabled by Default

The `/rubber-duck` agent is now **enabled by default** for all users. You no longer need to enable experimental mode to use it.

```
> /rubber-duck
```

**Why it matters:** Instant access to independent AI critique without any setup.

---

### Remote JSON RPC Enabled by Default

Remote JSON RPC transport is now enabled by default, improving integration with external tooling and MCP setups.

---

### Scheduled Prompts: `/every` and `/after` (Experimental)

Two new experimental scheduling sub-commands allow you to run prompts on a schedule:

- **`/every <interval> <prompt>`** — Repeat a prompt at a fixed interval (e.g., every 5 minutes)
- **`/after <delay> <prompt>`** — Run a prompt once after a delay

**How to use:**
```
> /experimental on
> /every 10m check for new issues and summarize them
> /after 1h remind me to run the tests
```

> ⚠️ These are experimental features. Run `/experimental on` to enable them.

**Why it matters:** Automate recurring checks or deferred reminders without leaving the CLI.

---

### New GitHub `/theme` (Experimental)

A new GitHub-branded theme is available under experimental features.

**How to use:**
```
> /experimental on
> /theme set github
```

> ⚠️ Requires `/experimental on`.

---

### New Experimental UI

An enhanced experimental UI provides quick access to GitHub issues, pull requests, and gists directly from the CLI prompt area.

**How to enable:**
```
> /experimental on
```

> ⚠️ Experimental feature — behavior may change in future releases.

---

## New in v1.0.57

Released: 2026-06-01

### `showTipsOnStartup` Setting

A new `showTipsOnStartup` setting controls whether the startup tips panel is displayed each time you launch Copilot CLI.

**How to configure:**
```json
{
  "showTipsOnStartup": false
}
```

**Why it matters:** Experienced users can suppress the tips panel for a cleaner startup experience.

---

### `/diff` Defaults to Branch Diff

When there are no unstaged changes, `/diff` now automatically shows the branch diff instead of an empty diff view.

**How to use:**
```
> /diff
```

**Why it matters:** You get useful output immediately without having to manually specify a branch target.

---

### Plugin Commands Show Immediate Progress Feedback

`/plugin install`, `/plugin uninstall`, `/plugin update`, and `marketplace add/remove/browse` sub-commands now display immediate feedback while the operation is in progress, so you can see what's happening instead of waiting silently.

---

### Azure DevOps Repositories: MCP Server Now Provides `web_search`

Previously, the built-in GitHub MCP server was fully disabled in Azure DevOps-only repositories. As of v1.0.57, instead of being fully disabled, the server now exposes only the `web_search` tool, so you retain web search capability even when working in Azure DevOps repos.

**Why it matters:** You no longer lose access to web search when working in Azure DevOps repositories.

---

### Default Networking Transport: HTTP/1.1

The default networking transport is now HTTP/1.1, improving reliability on some network paths (particularly proxies and corporate firewalls that have issues with HTTP/2).

To opt back into HTTP/2:
```bash
export COPILOT_ENABLE_HTTP2=1
```

---

### `preToolUse` Hook Errors Deny Tool Calls

`preToolUse` hook errors now **deny** the tool call rather than silently allowing execution to continue. This ensures that hook-based guardrails are always enforced.

> ⚠️ **Behavior change in v1.0.57:** If your `preToolUse` hooks have errors, tool calls will be blocked. Fix hook errors to restore expected behavior.

---

### Ctrl+C Terminates Full Process Tree

Canceling a running shell command — via Ctrl+C on a `!command`, or aborting an agent command including in sandboxed and background-promoted shells — now terminates the entire process tree, preventing orphaned background processes.

---

### Other Improvements and Fixes

- **Actionable rate-limit error:** `copilot update` now shows a clear error message when it hits the GitHub API rate limit
- **`COPILOT_HOME` for server discovery:** `COPILOT_HOME` is now honored for the server discovery registry directory
- **`@`-mention case-insensitive search:** File search via `@`-mention matches files regardless of query letter casing
- **`/lsp` subdirectory fix:** `/lsp show`, `/lsp test`, and `/lsp reload` correctly discover project LSP config when the CLI is launched from a subdirectory
- **`/skills` quoted path fix:** `/skills add` and `/skills remove` correctly handle paths wrapped in quotes (e.g., from Windows Explorer "Copy as path")
- **`copilot plugin marketplace list` honors `extraKnownMarketplaces`:** Repo-level `extraKnownMarketplaces` settings from `.github/copilot/settings.json` are now respected
- **MCP `npx --registry` policy fix:** MCP servers configured with `npx --registry` are no longer incorrectly blocked by policy
- **MCP timeout preserved:** MCP server timeout configuration is preserved after tools list changes
- **Diff mode mouse click:** Click a diff line with the mouse to select it in diff mode
- **Tmux key input:** Ctrl+C and other modified keys work correctly inside tmux
- **High-contrast diff:** High-contrast diff backgrounds use darker colors to improve text readability
- **Unquoted multi-word prompt hint:** Running `copilot` with an unquoted multi-word prompt now shows a helpful "quote your prompt" hint
- **Quota footer:** Remaining quota is shown as a rounded percentage in the footer
- **Paste artifact fix:** Pasting text from a browser, editor, or terminal no longer leaves stray empty lines, broken box-drawing characters, or a misplaced cursor
- **Plugin isolation:** Plugins auto-installed from repository settings no longer leak into user global config; installed plugins no longer include the `.git` directory from the source repository
- **Canvas `file://` URLs:** Canvas providers can return `file://` URLs in open results for local file previews
- **Symlinked directories:** Symlinked directories now appear in `/cwd` completion suggestions
- **Session resilience:** Session resume works correctly after a crash that left partial data in the session log; sessions no longer hang indefinitely after an internal event processing error
- **Reasoning display:** New reasoning after tool calls appears at the bottom of the timeline instead of above earlier output
- **Auth error clarity:** The underlying reason (e.g., GitHub API rate limit) is now surfaced when SDK auth-token validation fails

---

## New in v1.0.56

Released: 2026-05-29

### Free and Student Users Can Now Select Any Model

Free and Student plan users can now choose models other than Auto in the model picker. The previous restriction (introduced in v1.0.55) that limited these users to Auto-only selection has been lifted.

**How to use:**
```
> /model
```

Select any available model from the picker regardless of your plan tier.

**Why it matters:** All plan tiers now have full access to the model picker, giving every user the flexibility to switch to the model best suited for their task.

### `builtInAgents.rubberDuck` Setting

The rubber-duck agent can now be explicitly enabled or disabled via `copilot config` or `~/.copilot/settings.json`:

```json
{
  "builtInAgents": {
    "rubberDuck": true
  }
}
```

Set to `false` to hide the rubber-duck agent entirely from the session.

**Why it matters:** Teams or CI environments that want a consistent, focused agent experience can now suppress the rubber-duck agent without relying on experimental feature flags.

### GitHub MCP Server Omits Redundant Tools When `gh` CLI Is on PATH

When the `gh` CLI is available on PATH, the built-in GitHub MCP server now automatically omits tools that duplicate `gh` CLI capabilities. This reduces token usage by keeping the tool list lean — only tools that aren't covered by the `gh` CLI are surfaced to the model.

**Why it matters:** Fewer redundant tools means less context consumed per request, especially in long sessions or repositories with many GitHub operations.

### MCP Tools Now Surface Both `content` and `structuredContent`

MCP tools that return both a human-readable `content` text payload and a `structuredContent` payload now deliver both to the agent. Previously, one side could be dropped. When the text is the literal JSON serialization of the structured payload (per MCP spec §5.2.6), it is deduplicated; otherwise the two are concatenated.

**Why it matters:** Tool responses are now complete and unambiguous, improving agent accuracy when working with MCP servers that use both response formats.

### Diff View Continuous Scroll Layout

The diff view now uses a continuous scroll layout with sticky file and hunk headers, full terminal width, and theme-aware colors. Long diffs are easier to navigate without losing track of which file or hunk you are reading.

### Code Review Agent Uses Current Session Model

The `/review` code review agent now uses the same model as the active session instead of a fixed default. This ensures the review quality matches whatever model you have selected.

### Reasoning Effort Picker Respects Model Capabilities

The reasoning effort picker no longer shows effort options that are not supported by the currently selected model. Only valid choices are displayed, preventing configuration errors.

### `web_fetch` Prefers Markdown Content

The `web_fetch` tool now uses HTTP content negotiation to prefer markdown content when available. Documentation sites that serve both HTML and markdown will return cleaner, more structured results.

### Other Fixes and Improvements

- ThemePicker side-by-side layout fits within a 120-column terminal without wrapping
- Model picker shows accurate total context window size per pricing tier
- Extended key reporting works correctly in tmux when Kitty keyboard protocol is unavailable
- Config and settings files are written atomically to prevent data loss when multiple CLI processes run concurrently
- BYOK provider configuration now applies correctly to ACP sessions
- Fix `/context` small-token legend formatting and free-space grid rounding
- File paths in `/env` output display with correct formatting
- Reasoning text always displays above the assistant response in the conversation timeline
- Assistant responses render without single-word orphan lines in the terminal timeline
- Cursor stays at correct position after pasting text that contains tab characters
- Context window tier selection now persists durably across SDK-only resume paths
- Remote session URL correctly uses the repository owner/name instead of literal `'copilot'`
- Trusted folder confirmation message clarifies that permissions may be remembered for the session

---

## New in v1.0.55

Released: 2026-05-28

### `/autopilot <objective>` — Focused Autopilot with `/goal` Alias

`/autopilot` now accepts an optional objective argument to keep the autonomous session focused on a specific goal. `/goal` is an alias for the same command.

**How to use:**
```
> /autopilot Refactor the authentication module to use JWT
> /goal Add comprehensive test coverage to the payments service
```

**Why it matters:** Without a stated objective, long autopilot sessions can drift scope. Providing an objective gives the model a concrete target and reduces off-task tool calls.

### Claude Opus 4.8

Claude Opus 4.8 is now available. Select it in the `/model` picker or via the flag:

```bash
copilot --model claude-opus-4.8
```

### Reasoning Tokens in Session Usage

Claude thinking (reasoning) tokens are now included in `/usage` summaries, giving a complete picture of token consumption for models that emit reasoning traces. The reasoning token count is also visible in session token summaries for all users.

### Recursive Skills and Agent Discovery

Custom agents and skills are now discovered **recursively** in subdirectories. You no longer need to place skill files directly in the root of a skills directory — nested folder structures are fully supported.

Additionally, `--plugin-dir` skills now take precedence over personal-home skills (`~/.copilot`, `~/.agents`) with the same name. The full priority order is:

```
project > --plugin-dir > personal (~/.copilot / ~/.agents) > custom
```

### `permissions.disableBypassPermissionsMode` Setting

A new `permissions.disableBypassPermissionsMode` setting in `~/.copilot/settings.json` prevents the session from entering allow-all or yolo mode. Useful for teams or CI environments where unrestricted tool access should be blocked:

```json
{
  "permissions": {
    "disableBypassPermissionsMode": true
  }
}
```

When enabled, `/allow-all` and `/yolo` commands are disabled for the session.

### Per-MCP-Server Token Usage

`/mcp` now shows per-server token consumption, and `/context` breaks out MCP tool tokens as a separate line item. This makes it easy to identify which MCP servers are contributing most to context size.

### Other Fixes and Improvements

- Free and Student plan users on token-based billing are restricted to Auto model selection, with an explanation shown in the model picker
- MCP server configuration form saves the latest typed value when pressing Ctrl+S
- MCP configuration now opens in its own dedicated screen, with scrollable server and tool lists
- `exit_plan_mode` tool is only offered to the model while the session is in plan mode
- `/statusline` and `/theme` commands can now run while the agent is executing
- Extension log files are captured per extension and surfaced in the `extensions_manage` tool
- Project extensions in `.github/extensions` are now discovered in non-git (folder-backed) workspaces
- Hook progress streaming shows real-time status messages from long-running hooks in the timeline
- Cell-based terminal renderer is now enabled for all users by default
- `/env` now shows loaded extensions with their status and source
- `copilot update` and `copilot version` authenticate release API requests to avoid rate limit errors in shared-NAT environments
- Native binary crash (e.g. SIGSEGV) now falls through to the JavaScript fallback instead of silently exiting
- Clipboard paste works correctly on Wayland compositors that do not support wlr-data-control (e.g. GNOME/Mutter)
- PowerShell 7 correctly detected when `pwsh.exe` is installed as a Microsoft Store App Execution Alias

---

## New in v1.0.54

Released: 2026-05-24

### Fixes and Changes

v1.0.54 is a stability release with internal fixes and changes. No user-facing features were added.

---

## New in v1.0.53

Released: 2026-05-24

### Multiline Prompt Display Fix

Multiline prompts now display fully in the input area without content being clipped or the selection offset being misaligned. Long, multi-line inputs are rendered correctly from the first character to the last.

**Why it matters:** Complex prompts that span several lines are now fully visible as you type, making it easier to review and edit them before submitting.

### `/skills` Picker Respects `--config-dir`

The `/skills` picker now correctly reads and saves skill preferences using the directory specified via `--config-dir`. Previously, preference changes made through the picker were written to the default config directory rather than the custom one.

**How to use:**
```
$ copilot --config-dir /path/to/custom-config
> /skills add my-skill
```

Skill preferences are now saved to the custom config directory as expected.

### Bash Sessions No Longer Hang with `PS0` or `PROMPT_COMMAND`

Bash shell sessions started by Copilot CLI no longer hang at startup when `PS0` or `PROMPT_COMMAND` is set in the environment. These environment variables are now handled safely, preventing the session from stalling.

**Why it matters:** Users who customise their shell prompt (e.g. via `starship`, `oh-my-bash`, or manual `PS0`/`PROMPT_COMMAND` exports) can now run bash tool sessions without encountering hangs.

---

## New in v1.0.52

Released: 2026-05-23

### `/compact` — Optional Focus Instructions

`/compact` now accepts an optional argument to shape what the compaction summary focuses on. This lets you preserve the details most relevant to your current task rather than relying on the default summary heuristic.

**How to use:**
```
> /compact
> /compact focus on the authentication refactor decisions
```

**Why it matters:** Guided compaction keeps the most important context in view even after a long conversation, reducing costly context re-establishment.

### `/usage` — Quota Progress Bars

`/usage` now displays graphical progress bars for session and weekly quota limits, making it easier to see at a glance how close you are to each limit.

**How to use:**
```
> /usage
```

**Why it matters:** Visual quota tracking helps you anticipate when to switch models or compact context before hitting a hard limit mid-task.

### Custom Agents — `deferred-tool-loading`

Custom agents can now opt into deferred tool loading by setting `deferred-tool-loading: true` in their YAML frontmatter. With this flag, tools are discovered on demand (tool-search) rather than loaded eagerly at startup, which significantly reduces initialization time for agents with large tool lists.

**Configuration:**
```yaml
---
name: large-toolset-agent
model: claude-sonnet-4.6
deferred-tool-loading: true
---
This agent loads tools on demand to reduce startup time.
```

**Why it matters:** Agents with many MCP servers or plugins start faster and avoid exceeding context limits from unused tool descriptions.

### Session Working Directory — `-C <dir>` Override

Sessions now resume in the working directory that was active when the session was last saved. Pass `-C <dir>` at startup to override the saved directory.

**How to use:**
```bash
# Resume in the saved directory (automatic)
copilot --continue

# Override the resume directory
copilot --continue -C /path/to/other/dir
```

**Why it matters:** You can pick up exactly where you left off without `cd`-ing first, while still having an explicit escape hatch.

### `--continue` Refreshes Branch and Git Context

`copilot --continue` now refreshes the saved branch name and git context when resuming from a session's saved directory, rather than carrying over stale git metadata from the previous session.

**Why it matters:** Prevents misleading status display when the branch has changed between sessions.

### `/restart` and `/update` Preserve Session ID

`/restart` and `/update` now keep the same session ID after restarting, so session continuity is maintained without needing to re-attach or re-identify the session.

**Why it matters:** Automation and scripts that track session IDs no longer break when the CLI restarts or self-updates.

### General-Purpose Subagents Use GPT-5.4 or GPT-5.5

`general-purpose` subagents (launched via `/delegate` and `/fleet`) now automatically select GPT-5.4 or GPT-5.5 when those models are available on your account, giving subagent tasks access to the strongest available reasoning capability.

**Why it matters:** Complex multi-step subagent tasks benefit from the highest-quality model without any manual model selection.

### Auto-Prune Process Log Files

Copilot CLI now automatically prunes old process log files from `~/.copilot/logs/` at startup, preventing unbounded disk growth from accumulated log rotations.

**Why it matters:** No manual log cleanup needed in long-running installations.

### Legacy MCP OAuth Key Migration

Legacy nested `oauth.clientId` and `oauth.callbackPort` keys in MCP server configs are now automatically migrated to the supported `oauthClientId` and `auth.redirectPort` keys instead of being silently dropped.

**Why it matters:** Existing MCP configs with the old key format now work correctly without manual edits.

### Bug Fixes and Polish (v1.0.52)

- **Non-interactive subcommands** (`plugin list`, `mcp list`, `help`, `version`) no longer consume stdin
- **Autopilot mode** switching no longer triggers unexpected permission prompts for tool, path, or URL access
- **Context window tier selection** (~200K vs 1M tokens) is now enforced end-to-end — picking a tier actually constrains compaction, truncation, and token display
- **Kill command safety filter** no longer rejects valid commands containing shell redirection like `kill -0 <PID> 2>/dev/null`
- **Status line command** supports plain shell commands in addition to executable script paths
- **Rendering fixes**: no more stuttering in tmux on Cygwin or mintty; gray background bar behind user messages removed on non-truecolor terminals
- **AI Credits** usage correctly displays after sessions using the Responses API; error messages updated with clearer language and a Manage budget link
- **Session file corruption fix**: sessions containing events with non-URL strings in URL/URI fields now resume without error
- **HTTP/2 upload stall retry**: requests that time out due to an HTTP/2 upload stall automatically retry over HTTP/1.1
- **Windows**: sessions no longer fail to load when a process exits with a high-bit exit code (e.g., .NET unhandled exceptions)
- **Timeline UI**: entry connector color matches surrounding elements when expanded
- **Picker checkboxes** now use a single-cell ▣/▢ glyph for tighter, more consistent rows
- **Reasoning tokens** display as a parenthetical on output token count in the token usage summary
- **Exit summary** displays `AI Credits` label with correct spacing before the value
- **MCP OAuth re-authentication** honors the configured `redirectPort`
- **PowerShell** division operator no longer triggers false 'Allow directory access' prompts on Windows
- **`/statusline` picker** polished with cleaner item descriptions and better spacing

---

## New in v1.0.51

Released: 2026-05-20

### `/security-review` — Security-Focused Code Review (Experimental)

The new `/security-review` command runs a dedicated security review agent on your current changes. It focuses specifically on security vulnerabilities — injection risks, authentication issues, secrets exposure, and similar concerns — rather than general code quality.

**How to use:**
```
> /security-review
```

**Why it matters:** Security concerns are often missed in standard code review. A dedicated security pass before opening a PR helps catch vulnerabilities early.

> ⚠️ Experimental feature. Annotated with `(experimental)` in the command picker.

### `--session-id=<id>` — Resume or Start Sessions by UUID

The new `--session-id=<id>` flag lets you resume a known session by its UUID, or start a brand-new session with a specific UUID. Useful in CI pipelines or scripts that need deterministic session identity.

**How to use:**
```bash
# Resume a known session
copilot --session-id=<uuid>

# Start a new session with a specific UUID
copilot --session-id=<new-uuid>
```

**Why it matters:** Gives scripts and automation full control over session continuity without relying on the auto-selection heuristic.

### `preMcpToolCall` Hook — Control Outgoing MCP Request Metadata

A new `preMcpToolCall` hook event fires before each outgoing MCP tool call. Hook providers can inspect and modify the request metadata (e.g., add tracing headers or enforce policies) before the request is sent to the MCP server.

**Configuration:**
```json
{
  "hooks": {
    "preMcpToolCall": [
      {
        "command": "~/.copilot/hooks/mcp-pre-call.sh"
      }
    ]
  }
}
```

**Why it matters:** Complements the existing `preToolUse` / `postToolUse` hooks with MCP-specific control over outgoing requests.

### `/chronicle cost-tips` — Personalized Token Cost Recommendations

The `/chronicle` command gains a new `cost-tips` subcommand that analyzes your session usage and provides personalized recommendations for reducing token consumption and cost.

**How to use:**
```
> /chronicle cost-tips
```

**Why it matters:** Helps token-based billing users identify expensive patterns and optimize their workflows.

### Secret Scanning for Commit Messages and PR Descriptions

The CLI now scans commit messages and PR descriptions for secrets before publishing them. Detected secrets are automatically redacted to prevent accidental exposure.

**Why it matters:** An extra safety net that catches secrets in prose text, not just in code files.

### `terminalProgress` Setting — OSC 9;4 Terminal Progress Indicators

A new `terminalProgress` setting controls whether the CLI emits OSC 9;4 progress codes. These codes allow terminals that support them (e.g., Windows Terminal) to display task progress in the taskbar or tab.

**How to configure:**
```json
{
  "terminalProgress": true
}
```

Set to `false` to disable if your terminal renders the escape sequences as visible noise.

### Other Improvements and Fixes

- **`/remote` respects org policy:** `/remote` commands now honour organisation remote-control and cloud-view policies; a clear error is displayed when the org has disabled remote access.
- **`/remote` usable mid-task:** The `/remote` command can now be invoked while the agent is actively working, not only between turns.
- **Faster MCP startup:** MCP tool loading at startup is significantly faster for users with many HTTP-based MCP servers.
- **Settings file stability:** The settings file no longer accumulates unrelated config keys when settings are updated by the CLI.
- **MCP OAuth persistence:** MCP servers that use OAuth stay connected when authentication was performed in a separate session.
- **Experimental mode indicator:** The experimental-mode indicator now appears persistently in the app header instead of as a one-time notification.
- **Loading indicator colour:** The loading spinner colour now matches the active mode (plan, autopilot, shell).
- **`postToolUse` in successful results:** `postToolUse` hooks can now inject `additionalContext` into successful tool results (previously only failed results were supported).
- **GitHub MCP web search immediately available:** The GitHub MCP web search tool is available at startup without requiring a `/mcp search` step first.
- **Input area responsive height:** The input area grows responsively with terminal height instead of capping at 3 lines.
- **Remote session failure visibility:** Startup failures for remote sessions are only shown when remote mode was explicitly requested (`--remote` or user config), reducing noise in local sessions.
- **Shell tool robustness:** Shell tool calls succeed even when the model omits the `description` parameter.
- **Token usage formatting:** Input token usage now correctly includes cached tokens, and formatting has been updated to clarify token counts.
- **Login prompt improvement:** The login prompt now more clearly warns when token storage falls back to insecure plain-text config.
- **`/memory show` documentation links:** `/memory show` now displays links to documentation for learning about and managing Copilot Memory.
- **GFM rendering fix:** GFM tables and blockquotes inside list items render correctly without a floating top border.
- **Subcommand completion Enter key:** Pressing Enter on a highlighted subcommand completion now inserts the selection instead of submitting the partial command.

---

## New in v1.0.49

Released: 2026-05-19

### `/memory` — Persistent Cross-Session Memory

The new `/memory` command lets you enable, disable, or inspect Copilot's persistent memory. When memory is on, the agent can store information (preferences, recurring patterns, useful context) that carries over across sessions.

**How to use:**
```
> /memory on
> /memory off
> /memory show
```

**Memory scopes:** Each stored memory item is scoped to either your user account (private) or a specific repository (shared with collaborators). Before saving anything, Copilot shows a permission prompt that names exactly who will be able to see the memory. Session timeline entries are annotated with `(for user)` or `(shared with repository collaborators)` accordingly.

**Why it matters:** Persistent memory allows Copilot to remember your preferences and project context without requiring you to repeat them at the start of every session.

### `/rubber-duck` — Independent Critique of Current Work (Experimental)

The `/rubber-duck` command invokes a separate agent to review and critique what the active agent has done so far. It provides a fresh perspective, surfaces blind spots, and may suggest alternative approaches.

**How to use:**
```
> /rubber-duck
```

**Why it matters:** Getting a second opinion mid-task can catch issues before they compound, especially during long or complex agent runs.

> ⚠️ This is an experimental feature. It may be annotated with `(experimental)` in the command picker.

### `/chronicle search` — Search Session Content by Keyword

The `/chronicle` command now supports a `search` subcommand that queries all session content (history, file changes, decisions) by keyword or topic.

**How to use:**
```
> /chronicle search "database migration"
> /chronicle search authentication
```

**Why it matters:** As sessions grow long, finding specific earlier context becomes difficult. Keyword search makes it fast to locate past decisions without scrolling through the full narrative.

### `/session id` — Display and Copy Current Session ID

`/session id` displays the current session ID and copies it to the clipboard, making it easy to reference the session in scripts, issue links, or hand-offs.

**How to use:**
```
> /session id
```

### `/exit print` — Print Session Before Exiting

`/exit print` prints the full session transcript to the terminal before the CLI closes.

**How to use:**
```
> /exit print
```

**Why it matters:** Useful for piping output, capturing logs in CI environments, or generating a human-readable record of a session without needing a separate `/share` step.

### `/mcp search` — Discover and Install MCP Servers (Experimental)

The new `/mcp search` subcommand lets you search the MCP server registry and install servers directly from within the CLI.

**How to use:**
```
> /mcp search <query>
> /mcp search database
```

> ⚠️ Experimental feature.

### `copilot plugin update --all` — Update All Plugins at Once

`/plugin update --all` updates every installed plugin in a single command instead of updating them one-by-one.

**How to use:**
```
> /plugin update --all
```

### `postToolUse` Hook `additionalContext` Now Injected as System Message

Previously, `additionalContext` returned by a `postToolUse` hook was silently discarded. It is now injected as a system message that the model can see and act on. This enables post-tool hooks to meaningfully influence subsequent model responses.

### Other Improvements and Fixes

- **Auto-link GitHub references:** The assistant now auto-links `owner/repo#number` references (issues and PRs) in responses.
- **Prompt mode loads workspace MCP sources:** Running with `-p` automatically loads workspace MCP server sources when the current folder is already trusted, matching interactive mode behaviour.
- **Alpine Linux (musl libc) support:** The CLI can now run on Alpine Linux.
- **"None" reasoning effort:** A new "None" option in the reasoning effort picker disables model reasoning entirely.
- **`auth.redirectPort` config option:** MCP servers can pin their OAuth callback to a fixed port with the new `auth.redirectPort` configuration key, useful in constrained network environments.
- **`COPILOT_PLUGIN_DIR_ONLY` env var:** Set this to disable automatic plugin discovery and use only the plugins in `--plugin-dir` — enabling deterministic plugin sets in CI/CD.
- **`--plugin-dir` and `--additional-mcp-config` in server/headless mode:** Both flags now work in `--server` / `--headless` mode.
- **Hooks fire for sub-agent tool calls:** `preToolUse`, `postToolUse`, `subagentStart`, and `subagentStop` hooks now fire correctly for tool calls made by sub-agents, not only top-level calls.
- **Experimental command annotation:** Slash commands that are experimental are now annotated with `(experimental)` in the help dialog and command picker.
- **Content-filtered responses show explanation:** When a model response is blocked by content filtering, the CLI now displays an explanation instead of a blank assistant turn.
- **Auto-update downloads platform-specific package:** Auto-update now fetches the smaller platform-specific build instead of the universal package.
- **Repo hooks load in prompt mode:** Hooks in `.github/hooks/` now load when running with `-p` if the folder is already trusted.
- **Input prompt collapses when empty:** The input prompt shrinks to a single line when empty and grows naturally as you type.
- **MCP stdio servers show type as `stdio`:** The display type for MCP stdio servers is now `stdio` instead of `local`.
- **Cursor positioning and wide-character rendering fixes:** Cursor positioning in input fields and text copying from the scroll view both work correctly with CJK characters and emoji.

---

## New in v1.0.48

Released: 2026-05-14

### Model Picker Shows Actual Token Prices

The `/model` picker now displays actual token prices for token-based billing users, replacing the previous dot indicator system. Users on token-based billing plans can see precise per-token pricing for each model directly in the picker, making cost estimation more transparent.

**Why it matters:** You can now compare real prices at a glance instead of interpreting opaque indicators before selecting a model.

### Instruction Files: Unquoted Glob Patterns in `applyTo` Now Work Correctly

Instruction files that use unquoted glob patterns in the `applyTo` frontmatter field (e.g., `applyTo: **/*.ts`) are now applied correctly. Previously, unquoted wildcard patterns were not matched, so the instruction file was silently skipped for all files.

**Example — now works as expected:**
```yaml
---
applyTo: **/*.ts
---
Always use strict TypeScript — no `any` types.
```

**Why it matters:** Many editors and tools generate `applyTo` lines without quotes. These patterns now work without needing to add quotes manually.

### `/context` Shows Correct Token Limits for All Models

`/context` now displays the correct maximum token limit for whichever model is active, instead of always showing 128k regardless of the model. Models with larger context windows (e.g., 200k) now report their actual limit.

**Why it matters:** Accurate context window information helps you plan how much content to include before hitting limits.

### GitHub MCP Server Auto-Disabled in Azure DevOps Workspaces (Prompt/Headless Mode)

The built-in `github-mcp-server` is now automatically disabled in Azure DevOps-only workspaces when running in prompt (`-p`) or headless mode, matching the existing behavior in interactive mode. This prevents authentication errors and irrelevant GitHub API calls in Azure DevOps pipelines.

### `/ask` Dialog No Longer Prompts for Follow-Up Replies

The `/ask` dialog no longer shows a follow-up input prompt it cannot receive. The dialog now closes cleanly after displaying its answer, removing a confusing empty input field.

### Skill Content: YAML Frontmatter Metadata No Longer Sent to Model

When skill content is injected into the model's context, the YAML frontmatter block (metadata such as `ID`, `Version`, `Author`, `Tags`) is now stripped before injection. Only the actual skill instructions reach the model. This reduces wasted tokens and keeps skill metadata out of the model context.

### CJK Characters and Emoji Render Without Blank Gaps

Input text containing CJK (Chinese, Japanese, Korean) characters or emoji no longer displays blank gaps between lines. The renderer now correctly accounts for double-width characters.

### Terminal Cursor Positioning Fixed

The terminal cursor now positions correctly on the input field rather than on decorative UI elements such as the selected tab indicator.

---

## New in v1.0.47

Released: 2026-05-13

### `/fork` Accepts an Optional Name

The `/fork` command now accepts an optional name argument. The new forked session is immediately identifiable by that name, and the sessions dialog shows the origin session so you always know where a fork came from.

**How to use:**
```
> /fork
> /fork my-experiment
```

**Why it matters:** Named forks are easier to find later with `/resume`; the origin label in the sessions dialog makes it clear which sessions are related.

### Copilot Max — Correct Models Shown for Subscription Tier

Copilot Max subscribers now see only the models available to their subscription tier in the `/model` picker. Previously, the picker could show models that were not accessible under the Max plan, leading to unexpected errors when selecting them.

**Why it matters:** No more confusion over which models you can actually use — the picker now reflects your subscription accurately.

### j/k Keys for Navigation in the `/diff` View

The `/diff` view now supports `j` and `k` for scrolling down and up respectively, matching the Vim-style navigation already available in other pickers.

**How to use:**
```
> /diff
# then press j to scroll down, k to scroll up
```

**Why it matters:** Faster keyboard-only navigation through large diffs without reaching for arrow keys.

### `--resume` Supports Cloud Agent Sessions with No Branch Changes

`--resume` can now resume a Copilot cloud agent session even when the agent has not yet pushed any commits to its branch. Previously, resuming such sessions failed with an error.

**Why it matters:** You can pick up cloud agent sessions from the very start of their work, before any code has been pushed.

---

## New in v1.0.46

Released: 2026-05-12

### Deprecation Warning for Outdated CLI Versions

The CLI now displays a warning when your installed version is deprecated and continued use may result in loss of premium model access. You will see an inline notice prompting you to run `/update`.

**Why it matters:** Ensures you know before premium model access is silently reduced, giving you time to upgrade without disruption.

### Read-Only `gh` CLI Commands Auto-Approved

Read-only `gh` CLI commands — including `list`, `view`, `status`, `diff`, and similar — are now automatically approved without requiring a confirmation prompt. Only write operations (creating issues, merging PRs, etc.) still require approval.

**Why it matters:** Fewer interruptions during autopilot sessions; information-gathering steps proceed without manual confirmation.

### Diff View Wraps Long Lines at Terminal Width

Long lines in diff output now wrap at the terminal width instead of being truncated. Previously, long lines were cut off and could not be read in full without additional tooling.

### PowerShell Starts Correctly as a .NET Global Tool Shim

PowerShell now starts correctly when `pwsh` is installed as a .NET global tool shim (a common installation pattern on Windows). Previously this setup caused launch failures.

### Sessions No Longer Crash with ERR_HTTP2_INVALID_SESSION

A bug that caused sessions to crash mid-turn with `ERR_HTTP2_INVALID_SESSION` errors has been fixed. Sessions now recover cleanly from transient HTTP/2 connection issues.

---

## New in v1.0.45

Released: 2026-05-11

### `/autopilot` Slash Command — Toggle Modes Directly

A new `/autopilot` slash command lets you toggle between interactive and autopilot modes without cycling through all modes with Shift+Tab.

**How to use:**
```
> /autopilot
```

**Why it matters:** Faster mode switching — jump directly into or out of autopilot without pressing Shift+Tab multiple times.

> **Full guide:** See [Autopilot Mode](17-autopilot-mode.md) for permissions, continuation limits, and examples.

### `/fork` Command — Fork Session into Independent Copy

The new `/fork` command duplicates the current session into a new, fully independent session. The forked session starts with the same conversation history and context as the original but diverges from that point forward.

**How to use:**
```
> /fork
```

**Why it matters:** Explore an alternative approach without losing your current session. Fork before a risky set of changes, then resume the original if things go wrong.

### OpenTelemetry Output Aligns with GenAI Semantic Conventions

OpenTelemetry output now follows the GenAI semantic conventions:

- **MCP tool calls** now emit standard `tool_call` spans instead of custom span types.
- **New `gen_ai.client.operation.duration` metric** tracks tool execution time.

This improves compatibility with observability platforms (Datadog, Honeycomb, Jaeger, etc.) that expect the standard GenAI conventions.

### Windows PowerShell Fallback

On Windows, if PowerShell 7+ (`pwsh`) is not available, Copilot CLI now automatically falls back to Windows PowerShell (`powershell.exe`). Previously, the CLI would fail if `pwsh` was not installed.

### `agentStop` Hook Fires Correctly on `task_complete`

The `agentStop` hook now fires reliably when the agent stops via `task_complete`. Previously, the hook was not triggered in that code path.

### Faster Startup on Terminals with Limited OSC Support

The CLI now starts up to **~1.5 seconds faster** on terminals with limited OSC color query support (e.g., some SSH sessions, Windows Terminal configurations). The OSC color probe now times out quickly instead of waiting for a response that never comes.

### Session Resume Fixed for Extension Permission Prompts

Sessions that ended while an extension permission prompt was displayed can now be resumed cleanly. Previously, resuming such a session produced a **"Session file is corrupted"** error.

### Other Fixes and Improvements

- **Windows PowerShell fallback** — `powershell.exe` is used automatically when `pwsh` is unavailable (v1.0.45+)

---

## New in v1.0.44

Released: 2026-05-08

### Slash Commands Mid-Input and Multiple Skills in One Message

Slash commands can now appear anywhere in your input — you no longer need to start a message with `/` to invoke one. This also unlocks invoking **multiple skills** in a single message by including more than one skill reference inline.

**How to use:**
```
> Let me ask /skills python-expert and /skills security-audit to review this code
> Summarize this and /clear after you're done
```

**Why it matters:** More natural, flexible prompting — combine context, questions, and commands in one send.

### `userPromptSubmitted` Hook — Bypass the LLM

A new `userPromptSubmitted` hook event fires when the user submits a prompt, before the LLM is called. Hook scripts can now **handle the request directly** and return a response, bypassing the model entirely. This enables fully programmatic responses for specific prompt patterns.

**Configuration:**
```json
{
  "hooks": {
    "userPromptSubmitted": [
      {
        "command": "~/.copilot/hooks/prompt-router.sh"
      }
    ]
  }
}
```

The hook receives the user prompt via stdin. If the script exits `0` and writes a non-empty response to stdout, that response is used as the AI reply — no model call is made. Exit non-zero to let the prompt continue to the LLM as normal.

**Why it matters:** Enables instant, deterministic responses for FAQs, template-based replies, or local tool integrations without consuming model quota.

### `prerelease` Argument for `/update` and `copilot update`

Both the slash command and the CLI command now accept an optional `prerelease` argument to fetch the latest prerelease build:

```
> /update prerelease
```

```bash
$ copilot update prerelease
```

**Why it matters:** Easy access to the latest features before the stable release, without manual download.

### Other Fixes and Improvements

- **`/add-dir` path completion** no longer flickers or gets intercepted by `@` and `#` pickers
- **Faster `/user list` and `/user switch`** for multi-account users
- **Shell aliases and rc file settings** now work correctly in `!` commands (e.g., `!ll`, `!myalias`)
- **Quota display** correctly shows remaining usage for Free users (previously always showed 100% used)
- **Tool permissions** granted in autopilot mode are now preserved after `/clear` — no need to re-approve
- **Effort level** applies correctly when switching models via the `/model` picker
- **Ctrl+C** while a permission prompt is pending no longer causes the CLI to hang
- **Project info** remains visible in the slash command picker when no results match the filter
- **Invalid URLs** in `settings.json` no longer crash CLI startup — they are skipped with a warning
- **Timeline** shows the resolved model for rubber-duck sub-agents (e.g. `Rubber-duck(claude-opus-4.7)`)

---

## New in v1.0.43

Released: 2026-05-06

### Security Fix — RCE Protection for Nested Bare Repositories

A critical security fix protects against remote code execution triggered by malicious bare repositories nested inside a project directory. Users should upgrade immediately.

> 🔒 **Advisory:** [GHSA-9ccr-r5hg-74gf](https://github.com/github/copilot-cli/security/advisories/GHSA-9ccr-r5hg-74gf)

**Why it matters:** Prevents a class of supply-chain attacks where a nested bare repo could execute arbitrary code on your machine.

### Username Toggle in `/statusline`

The `/statusline` picker now includes a **`username`** item that displays your active GitHub account in the footer. Useful when switching between personal and work accounts.

```
> /statusline username
```

**Why it matters:** Quickly confirm which account is active without leaving the session.

### Auto Mode — Server-Side Model Routing

`auto` model mode now uses **server-side routing** to pick the best model in real time based on your request. The model selection adapts dynamically rather than being resolved once at session start.

```bash
copilot --model auto
```

**Why it matters:** You get the most appropriate model for each turn without manually switching.

### MCP Child Processes Fully Terminated on Session End

MCP server child processes launched via `npx`, `uvx`, or similar spawners are now **fully terminated** when a session ends. Previously, orphaned processes could linger in the background.

**Why it matters:** Cleaner resource management — no ghost processes accumulating across sessions.

### Download Progress for `/update`

Running `/update` or `copilot update` now shows a **download progress indicator** while the new version is being fetched.

**Why it matters:** Better visibility into update status, especially on slow connections.

### Other Fixes and Improvements

- **Resume prompt** now shows the correct session name when multiple sessions are active simultaneously.

---

## New in v1.0.42

Released: 2026-05-06

### `-C <directory>` Flag — Change Working Directory on Start

A new `-C <directory>` flag lets you start Copilot CLI in a specific directory without `cd`-ing first, following the same convention as `git -C`.

```bash
# Start a session rooted in ~/projects/myapp
copilot -C ~/projects/myapp

# Combine with -p for a non-interactive one-shot prompt
copilot -C ~/projects/myapp -p "What tests are failing?"
```

**Why it matters:** Useful in scripts, aliases, and CI pipelines where the calling directory may differ from the project root.

### MCP Server Failure Warnings Now Include stderr Output

When an MCP server fails to connect, the warning message now includes the server's **stderr output**. This makes it much easier to diagnose why a server refused to start (missing env vars, wrong port, etc.).

**Why it matters:** Eliminates the need to run the server binary manually just to see its error output.

### Improved `/mcp show` Hint for Servers with Spaces in Their Name

When an MCP server whose name contains whitespace fails to start, the CLI now suggests a **directly runnable `/mcp show <server-name>`** command (with the name quoted correctly) rather than a generic hint.

**Why it matters:** One-click copy-paste command to inspect the failing server instead of having to figure out quoting manually.

### Rubber-Duck Agent for GPT Sessions (Experimental)

A new **rubber-duck agent** is available in `/experimental` for GPT-powered sessions. Powered by Claude, it provides a second-opinion sounding board within your session.

```
> /experimental enable
> /agent rubber-duck
```

**Why it matters:** Handy for talking through a problem and getting a fresh perspective without leaving the CLI.

### Remote Session Export Expanded

Remote session export (`/share` with `--remote`) now supports **non-GitHub repositories** and **repo-less directories**, not just GitHub-hosted repos.

**Why it matters:** Teams using other VCS hosts or working outside a git repo can now share sessions remotely.

### Other Fixes and Improvements

- **Exit message** resume command shows the **session ID** instead of an auto-generated name when the session has not been manually renamed.
- **False "session in use" warning** no longer appears after choosing "Go back" when resuming a session.
- **Enter key** no longer gets permanently stuck after cancelling a request.
- **Exit summary suppressed** when the session has no user messages and no saved session to resume.
- **Windows:** CLI updates no longer fail with `ENOENT` when a transient `EPERM` occurs during package extraction.

---

## New in v1.0.41

Released: 2026-05-05

### Faster Startup — Auth Resolves in Background

The CLI now renders the interactive UI **immediately** while authentication resolves in the background. You can start typing your first prompt before the auth handshake completes, reducing the time to first interaction.

**Why it matters:** Noticeably snappier start on slower networks or when token refresh is needed.

### Shell Completions Auto-Install

Shell completions for **bash, zsh, and fish** are now **automatically installed on first run** and refreshed automatically after `copilot update`. No manual `copilot completion <shell>` step is required.

**How to use:**

```bash
# Completions are set up for you on first launch.
# To manually regenerate at any time (v1.0.37+):
copilot completion bash >> ~/.bashrc
copilot completion zsh  >> ~/.zshrc
copilot completion fish > ~/.config/fish/completions/copilot.fish
```

**Why it matters:** New installs get working Tab completions out of the box.

### `--attachment` Flag in Non-Interactive Mode

The non-interactive (`-p` / `--prompt`) mode now accepts an `--attachment` flag to attach **images or native documents** to the initial prompt.

```bash
copilot -p "Describe the diagram" --attachment diagram.png
copilot -p "Summarize this document" --attachment report.pdf
```

**Why it matters:** Enables automated pipelines to pass files directly to the model without an interactive session.

### Experimental MCP Tasks

MCP tools that declare `taskSupport: "required"` now run as **non-blocking background agents** when experimental mode is enabled. Background tasks are trackable with the `list_agents` and `read_agent` tools.

**How to enable:**

```
> /experimental on
```

or via the CLI flag:

```bash
copilot --experimental
```

**Why it matters:** Long-running MCP tool calls no longer block the session; you can continue working while the task runs.

### Extensions in Prompt Mode

Extensions now load when using prompt mode (`-p`). **User-level extensions** load by default; **project extensions and management tools** require an opt-in env var:

| Scope | Behaviour |
|-------|-----------|
| User extensions | Load automatically in `-p` mode |
| Project extensions & management tools | Require `GITHUB_COPILOT_PROMPT_MODE_EXTENSIONS=true` |

```bash
GITHUB_COPILOT_PROMPT_MODE_EXTENSIONS=true copilot -p "Run project setup"
```

### Other Fixes and Improvements

- **Remote session errors** now display your logged-in account and tailored remediation steps.
- **Markdown formatting** renders inside `ask_user` prompt questions from the agent.
- **Slash command picker** now searches command descriptions and underlines matched characters.
- **Memory tool** confirmation prompt shows the scope (`repository` or `user`) when requesting permission to store a memory.
- **`--attachment` @-mention completion** works for `./` paths; no trailing space added for directories; project files shown before workspace roots.
- **Windows stability:** Works around a V8 crash in Node 24.x.
- **Windows packaging:** Extraction no longer crashes when antivirus or filesystem locks cause transient `EPERM` errors.
- **Unicode:** Session files containing Unicode line separator characters load correctly.
- **Reasoning effort picker** hint text now correctly reads "Esc to cancel".
- **File edit reliability** improved by better recovery from fuzzy or misaligned edit blocks.
- **Streaming animations** stay smooth on slow or busy hosts.
- **SQL todo timeline** entries display more accurately for `INSERT OR IGNORE`/`REPLACE` and blocked status updates.
- **Large output guidance** correctly references the configured grep tool name.
- **Plugin marketplace** installation via git SSH URL (e.g. `git@github.com:owner/repo`) now works correctly.
- **Assistant responses** no longer contain spurious system notification XML tags.

---

## New in v1.0.40

Released: 2026-05-01

### MCP Servers: Headless OAuth with `client_credentials`

MCP servers can now authenticate using the **`client_credentials` OAuth 2.0 grant type**, enabling fully headless authentication without opening a browser. This is ideal for CI/CD pipelines and server environments where interactive login is not possible.

**How to configure:**

```json
{
  "mcpServers": {
    "my-api-server": {
      "url": "https://my-mcp-server.example.com/mcp",
      "auth": {
        "type": "oauth2",
        "grant_type": "client_credentials",
        "client_id": "your-client-id",
        "client_secret": "your-client-secret",
        "token_url": "https://auth.example.com/oauth/token"
      }
    }
  }
}
```

**Why it matters:** Automation workflows and CI pipelines can now connect to authenticated MCP servers without any manual browser-based login step.

### Autopilot Default Continuation Limit

Autopilot mode now applies a **default limit of 5 autonomous continuations**. Previously, autopilot would run without a cap unless you explicitly set `--max-autopilot-continues`. This change prevents runaway execution out of the box.

Override the default:

```bash
copilot --max-autopilot-continues 20   # raise the limit
copilot --max-autopilot-continues 0    # remove the limit entirely
```

**Why it matters:** Safer defaults — expensive or long-running autopilot tasks now stop at a sensible boundary without any extra flags.

### `COPILOT_HOME` Replaces `--config-dir`

The `--config-dir` flag is **deprecated** in favour of the `COPILOT_HOME` environment variable. Both still work, but `COPILOT_HOME` is the recommended way to point the CLI at a non-default config directory.

```bash
# Deprecated (still works)
copilot --config-dir /custom/config/path

# Recommended (v1.0.40+)
export COPILOT_HOME=/custom/config/path
copilot
```

`COPILOT_HOME` also propagates correctly to plugin subcommands — a bug that affected `--config-dir` in earlier versions.

### `/chronicle` Available to All Users

The `/chronicle` command — along with **session history and file tracking** — is now available to all users, not just those in an early-access group.

```
> /chronicle         # View a narrative history of what this session has done
```

Use `/chronicle` to get a concise, human-readable account of all the changes and actions taken in the current session — useful for writing commit messages, reviewing work, or handing off to a colleague.

### Skills as Slash Commands in ACP Clients

Skills are now exposed as **slash commands inside ACP client sessions** (e.g. Zed), matching the experience already available in the interactive CLI. Any skill you have enabled appears as a `/skill-name` command in the ACP client's command picker.

### `/research` Uses Orchestrator/Subagent Architecture

The `/research` command now runs with an **orchestrator/subagent model** for more thorough and reliable deep research results. The orchestrator delegates parallel investigation threads to subagents and synthesises the findings into the final report.

**Why it matters:** More complex research tasks complete faster and produce higher-quality results through parallel investigation.

### Azure DevOps Repositories: GitHub MCP Auto-Disabled

When Copilot CLI detects an **Azure DevOps repository** as the working context, the built-in GitHub MCP server is automatically disabled. This avoids authentication errors and irrelevant GitHub API calls in environments that use Azure DevOps instead of GitHub.

### ACP Clients Display Live Plan

ACP clients (e.g. Zed) now display the **agent's live plan** as it progresses through multi-step tasks, matching the plan-mode view already available in the interactive CLI.

### Prompt Mode Opt-In for Repo Hooks and Workspace MCP

Prompt mode (`-p` / `--prompt`) now applies **secure-by-default behaviour** for two features that could cause unexpected side effects in automated pipelines:

| Feature | Opt-in env var |
|---------|---------------|
| Repository hooks (AGENTS.md, `.instructions.md`, etc.) | `GITHUB_COPILOT_PROMPT_MODE_REPO_HOOKS=1` |
| Workspace MCP servers (`.mcp.json`) | `GITHUB_COPILOT_PROMPT_MODE_WORKSPACE_MCP=1` |

Without the opt-in env var, prompt mode ignores repo hooks and workspace MCP. Set the relevant variable to restore the previous behaviour.

### Other Fixes and Improvements

- **`/clear` and `/new`** now reset the active custom agent selection, so you start fresh without a stale agent context.
- **`Ctrl+C` and double-`Esc`** remove pending queued messages **one at a time** instead of clearing all queued messages at once.
- **Slash command suggestions** now rank prefix matches above fuzzy matches for more predictable autocompletion.
- **Remote session statusline** shows the remote working directory and branch instead of local context.
- **Session resume picker** no longer shows duplicate entries for the same Mission Control-backed session.
- **Mouse selection** works while the `/ask` response dialog is open, so its content can be highlighted and copied.
- **`/update`** no longer re-submits the original `-i` prompt after restarting.
- **`copilot plugin list`** shows the correct version after running `copilot plugin update`.
- **MCP OAuth tokens** cache correctly when multiple servers share the same URL but use different static OAuth client IDs.
- **MCP tool names** with dots or other invalid characters are now sanitized correctly.

---

## New in v1.0.39

Released: 2026-04-28

### Background Task with `Ctrl+X → B`

While a task or shell command is running, press `Ctrl+X` then `B` to move it to the background. The task continues running but returns you to the input prompt so you can continue other work. Monitor background tasks with `/tasks`.

**How to use:**

```
Ctrl+X → B    # Move the current running task or shell command to the background
> /tasks       # Check status of background tasks
```

**Why it matters:** Long-running operations (builds, tests, analysis) no longer block your input prompt — send them to the background and keep working.

### `/remote` Status Shows Actionable Hints

The `/remote` status output now shows **actionable hints** tailored to each connection state. Instead of just reporting the current state, it tells you exactly what to do next.

**How to use:**

```
> /remote
```

The output will now include a suggested next step (e.g. how to enable remote control, how to connect from another terminal, or what is preventing a connection).

**Why it matters:** No more guessing what a remote-control state means — the CLI tells you the next action directly in the output.

### `--resume` Session Picker Improvements

The session picker (opened with `--resume` / `/resume`) has an improved **tab layout**, enhanced **status display**, and **progressive loading** so the picker opens immediately and populates as sessions are fetched.

**Why it matters:** Large session histories now load faster, and the improved layout makes it easier to spot the right session at a glance.

### Slash Command Argument Picker Opens Immediately

The slash command argument picker now opens as soon as you type an exact command name, without requiring a trailing space. Previously you had to type `/command ` (with a space) before options appeared.

**How to use:**

```
> /model       # argument picker opens immediately — no space needed
```

### ACP Session Enhancements

For integrations using the **Agent Communication Protocol (ACP)** to connect external clients to Copilot CLI:

- **allow-all toggle** — ACP clients can now enable or disable allow-all permission mode via session configuration, without requiring a user to type `/allow-all` in the CLI.
- **Additional slash commands** — `/compact`, `/context`, `/usage`, and `/env` are now available within ACP sessions (previously these were interactive-only commands).

### Other Fixes

- **Pipe errors** — transient pipe errors on child process stdio streams no longer cause crashes or trigger false crash reports.

---

## New in v1.0.37

Released: 2026-04-27

### Location-Based Permission Persistence (Now Default)

Approvals you grant for tool calls (e.g. allowing a shell command or file write) are now automatically remembered for the current directory and carried over into future sessions from the same location. You no longer need to re-approve the same operations each time you restart.

Previously this behaviour required opting in; it is now enabled for all users by default.

**Why it matters:** Repeat tasks in the same project no longer require re-approving each permission from scratch — your approval history follows the directory.

### Shell Completion Scripts (`copilot completion`)

A new `copilot completion` subcommand generates static shell completion scripts for subcommands, flags, and known choice values.

**How to use:**

```bash
# Generate and install completions for your shell
copilot completion bash >> ~/.bashrc
copilot completion zsh  >> ~/.zshrc
copilot completion fish > ~/.config/fish/completions/copilot.fish

# Then reload your shell
source ~/.bashrc   # or exec zsh / exec fish
```

**Why it matters:** Tab-completion for CLI subcommands and flags without needing a live process.

### Session Picker Sort Order (`s` key)

In the `/resume` session picker, press `s` to cycle through sort orders:

| Sort Order | Description |
|------------|-------------|
| Relevance  | Default; matches by name/branch similarity |
| Last used  | Most recently active session first |
| Created    | Newest session first |
| Name       | Alphabetical by session name |

### `/ask` Responses Render Markdown

Responses from `/ask` now render full markdown — including tables, code blocks, bold/italic text, and formatted links — rather than plain text.

**How to use:**

```
> /ask What are the differences between /compact and /clear?
```

The answer will now appear with proper formatting.

### Skill Picker Stays Visible with Errors

The skill picker list remains fully visible even when some skills have load errors or configuration warnings. Previously a skill error could truncate the visible list; now all entries are shown alongside any error indicators.

### Other Fixes and Improvements

- **Model change notification** — re-selecting the currently active model or effort level no longer shows a redundant change notification
- **Clipboard on Linux** — clipboard write operations no longer leak X11 display handles
- **Pending message indicator** — displays correctly when shown alongside prompt frames
- **Detached HEAD detection** — fixed a bug where detached HEAD state always returned false after switching to `git branch --show-current`
- **ACP model config** — model config options now include `description` and metadata fields for clients using the `configOptions` API

---

## New in v1.0.36

Released: 2026-04-24

### Subcommand Picker Selection Indicator

The subcommand completion picker now shows a **❯** indicator next to the highlighted item, making it easier to see which option is currently selected before pressing `Tab` or `Ctrl+Y` to accept it.

### Double `Esc` to Cancel In-Flight Work

Cancelling an in-flight AI operation now requires pressing `Esc` **twice** in quick succession. A single `Esc` still cancels typed input or closes pickers. This prevents accidental interruptions when you accidentally brush the Escape key mid-task.

**How to use:**

```
Esc         # Clears the current input line / closes a picker
Esc Esc     # Cancels the AI operation currently in progress
```

### `/keep-alive` Available Without Experimental Mode

`/keep-alive` is now a standard command that prevents your system from going to sleep while Copilot CLI is active — no need to enable `/experimental` first.

**How to use:**

```
> /keep-alive
```

**Why it matters:** Long autopilot or fleet runs on laptops were interrupted by system sleep. Now you can prevent that without enabling the experimental feature flag.

### `/remote on` / `/remote off` Subcommands

The `/remote` command has been extended:

- `/remote` now shows the **current remote control status**
- `/remote on` enables remote control
- `/remote off` disables remote control

```
> /remote       # show current status
> /remote on    # enable remote control
> /remote off   # disable remote control
```

### `changes` Statusline Toggle

A new `changes` item is available for the statusline. It shows the number of **lines added and removed** in the current session, similar to a git diff summary.

**How to use:**

```
> /statusline changes
```

Toggle it off again with the same command.

### `preToolUse` Matcher Fix

A bug was fixed where the `matcher` field on `preToolUse` hooks was ignored. After upgrading to v1.0.36, hooks with a `matcher` will run **only for tool names whose full string matches the regex**. If you have hooks without a `matcher`, they continue to run for all tools.

> ⚠️ **Behavior change in v1.0.36:** If your `preToolUse` hooks relied on `matcher` being ignored (i.e., they were always firing), verify your matcher patterns after upgrading.

### Custom Instruction Files in `.gitignored` Directories

Instruction files placed in `.gitignored` directories — such as `.github/instructions/` when `.github/` is gitignored — now load correctly. Previously, Copilot CLI skipped these files if their parent directory appeared in `.gitignore`.

### Clearer Multiple-License Error

When multiple Copilot licenses are detected for your account, the error message now includes a **direct link** to the GitHub settings page where you can resolve the conflict.

### Disabled Skills Hidden from Slash Command List

Skills that have been disabled no longer appear in the slash command picker. Previously, disabled skills still showed up as slash command suggestions even though they could not be invoked.

### Claude Opus 4.6 — Medium Reasoning Effort by Default

Claude Opus 4.6 now uses **medium reasoning effort** by default (previously high). This reduces latency and premium request consumption for typical Opus 4.6 tasks. Switch to a higher effort level with `/model` if you need maximum depth for a specific task.

### Other Notable Changes

- **Debug logs / feedback bundles:** Saving logs no longer overwrites existing archive files — a unique filename is generated each time.
- **Custom agents, skills, and commands from `~/.claude/`:** These are no longer loaded by Copilot CLI (extends the v1.0.35 isolation change to cover custom commands in addition to agents and skills).

---

## New in v1.0.35

Released: 2026-04-23

### Tab-Completion for Slash Command Arguments

Slash commands now support tab-completion for their arguments and subcommands. Start typing a command and press `Tab` to complete argument names, subcommand options, and values.

**How to use:**

```
> /session <Tab>          # shows: checkpoints, files, plan, rename, delete, delete-all
> /model <Tab>            # shows available model IDs
> /resume <Tab>           # shows recent session IDs and names
```

**Why it matters:** Faster command entry with fewer typos — no more guessing subcommand names.

### `Ctrl+Y` to Accept Completions

You can now press `Ctrl+Y` (in addition to `Tab`) to accept the highlighted option in completion popups — including `@`-mentions, path completions, and slash command completions.

**Why it matters:** Adds a second familiar keybinding for completion acceptance, useful when `Tab` is already bound in your terminal.

### `/session delete` Subcommands

New subcommands let you delete sessions without leaving the CLI:

```
# Delete a specific session by ID (or 7+ char prefix)
> /session delete <session-id>

# Delete all sessions at once
> /session delete-all
```

In the session picker (`/resume`), press `x` on any entry to delete it immediately.

**Why it matters:** Previously you had to manage sessions manually on disk. Now you can clean up old sessions without leaving the CLI.

### `--name` Flag and Resume by Name

Name sessions at startup with `--name`, then resume them later by name:

```bash
# Start a named session
copilot --name "auth-refactor"

# Resume by name (in addition to session ID)
copilot --resume auth-refactor
```

Within a session, `/rename` still works to name or rename the current session.

**Why it matters:** Easier to track long-running work — meaningful names are simpler to remember than session ID prefixes.

### Session Picker Improvements

The `/resume` session picker now shows:
- **Branch name** for each session
- **Idle / in-use status** so you can tell at a glance which sessions are active
- **Improved search** with cursor support for filtering sessions

### `COPILOT_GH_HOST` Environment Variable

Set `COPILOT_GH_HOST` to override the GitHub hostname used by the CLI. This takes precedence over `GH_HOST`, making it easy to point the CLI at a GitHub Enterprise Server instance independently of other tools.

```bash
export COPILOT_GH_HOST=github.example-enterprise.com
copilot
```

**Why it matters:** Teams on GHES can now configure Copilot CLI's GitHub endpoint separately from other `gh`-based tools.

### User Settings Separated from Internal State

User-editable settings are now stored in `~/.copilot/settings.json`, separate from `~/.copilot/config.json` which holds internal CLI state. If you have a `config.json` with user preferences, they will continue to work — the split is transparent.

> ⚠️ **Note:** `settings.json` is the new home for user-facing configuration options like `model`, `theme`, and `continueOnAutoMode`. Direct edits to `config.json` still work for now, but prefer `settings.json` for new entries.

### `continueOnAutoMode` Config Option

Add `continueOnAutoMode: true` to `~/.copilot/settings.json` to automatically switch to the `auto` model when you hit a rate limit, instead of pausing the session:

```json
{
  "continueOnAutoMode": true
}
```

**Why it matters:** Keeps long autopilot or fleet runs moving even when a specific model's quota is exhausted — Copilot switches to `auto` and continues without interruption.

### HTTP Hook Support

Hooks can now POST JSON payloads to a configured URL instead of running a local shell command. This enables server-side hook handling for team workflows, CI integration, and audit logging.

**How to configure:**

```json
{
  "hooks": {
    "preToolUse": [
      {
        "url": "https://hooks.example.com/copilot/pre-tool",
        "matcher": { "tool_name": "shell" }
      }
    ],
    "postToolUse": [
      {
        "url": "https://audit.example.com/copilot/tool-events"
      }
    ]
  }
}
```

The hook endpoint receives the same JSON payload that shell-based hooks receive via stdin. The endpoint should respond with a JSON body in the same format as shell hooks return via stdout.

**Why it matters:** Teams can centralise hook logic on a server rather than distributing scripts to every developer machine.

### Session Selector: Branch and Status Visibility

The session picker (opened with `/resume` or `--resume` without an argument) now shows:
- The **git branch** active in each session
- Whether each session is **idle** or **in-use**

This makes it much easier to identify the right session when you have many open.

### Shell Escape Uses `$SHELL`

The `!` shell escape command now uses your `$SHELL` environment variable when it is set, instead of always invoking `/bin/sh`. This means your preferred shell (zsh, fish, etc.) is used for shell-escape commands.

### `/usage` Contribution Graph

The `/usage` command now includes a **GitHub-style contribution graph** showing your usage history. The graph adapts to your terminal's color mode and falls back to distinct glyphs in no-color terminals.

### Other Notable Changes

- **`--continue` prefers CWD:** `--continue` now resumes the most recent session from the current working directory first, rather than the globally most recently touched session.
- **Plugins take effect immediately:** Installed plugins are now active without requiring a CLI restart.
- **MCP server names:** Server names with spaces and special characters are now supported in MCP config.
- **LSP timeouts:** LSP server entries in `lsp.json` support `spawnTimeout`, `initializationTimeout`, and `warmupTimeout` fields for fine-grained control.
- **Sync task blocking:** Sync task calls (`mode: "sync"` in the task tool) now block until completion under `MULTI_TURN_AGENTS` mode; sync tasks no longer return a reusable `agent_id` — use `mode: "background"` for follow-ups.
- **Custom agent name in statusline:** The active custom agent's name is shown in the statusline footer and can be toggled via `/statusline`.
- **Clipboard error on Linux:** A helpful error message with install instructions is shown when `wl-clipboard` or `xclip` is missing.
- **`~/.claude/` isolation:** Custom agents and skills stored in `~/.claude/` are no longer incorrectly loaded as Copilot project config.

---

## New in v1.0.34

Released: 2026-04-20

### "Session Rate Limit" Error Message

Rate limit errors now say **"session rate limit"** instead of "global rate limit", clarifying that the limit applies to the current session rather than your entire account.

---

## New in v1.0.33

Released: 2026-04-20

### `--resume` / `--continue` Auto-Inherits `--remote`

When you resume a remote session using `--resume` or `--continue`, the `--remote` flag is now automatically inherited — you no longer need to re-specify it.

**Before v1.0.33:**
```bash
copilot --resume <session-id> --remote
```

**Now:**
```bash
copilot --resume <session-id>   # --remote inherited automatically
```

**Why it matters:** Fewer flags to remember when picking up a remote session.

### New Slash Command Aliases

Several new short aliases have been added for frequently used commands:

| Alias | Maps to | Purpose |
|-------|---------|---------|
| `/upgrade` | `/update` | Update the CLI to the latest version |
| `/bug` | `/feedback` | Report a bug or submit feedback |
| `/continue` | `/resume` | Continue/resume a previous session |
| `/release-notes` | `/changelog` | View recent release notes |
| `/export` | `/share` | Export or share the current session |
| `/reset` | `/clear` | Reset conversation history |

### Slash Command Picker Suggests Similar Commands

If you type an unrecognized or misspelled slash command, the picker now suggests the closest matching commands instead of just showing a generic error.

**How to use:**

```
> /changelg
  Did you mean: /changelog?
```

**Why it matters:** Reduces frustration from typos and makes command discovery easier.

### `ctrl+t` Reasoning Toggle Listed in Help

The `ctrl+t` keyboard shortcut for toggling model reasoning display is now listed in the `/help` overlay and `?` shortcut panel, making it more discoverable.

### Sub-Agents in Auto Mode Inherit Session Model

When using `auto` model selection, sub-agents launched during a session now inherit the same model automatically rather than defaulting to a different one.

### Usage Limit Warnings Updated to 50% and 95%

Usage warnings now fire at **50%** and **95%** of your weekly premium request limit (previously 75% and 90%), giving you earlier notice before hitting the cap.

### Vim-Style Navigation in Tasks Dialog

The `/tasks` dialog now supports keyboard shortcuts for power users:

| Key | Action |
|-----|--------|
| `j` | Move selection down |
| `k` | Move selection up |
| `x` | Cancel / kill the selected task |

### Fixes and Improvements

- **Grep**: No longer times out on large repositories when content exclusion policies are enabled
- **Non-interactive mode**: Waits for all background agents to finish before exiting, ensuring CI pipelines capture complete output
- **Skill picker**: Correctly truncates CJK/Japanese descriptions and long skill names without wrapping
- **Slash command picker**: Selects the highlighted command when pressing Enter (previously required clicking)

---

## New in v1.0.32

Released: 2026-04-17

### `auto` Model — Let Copilot Pick for You

Select `auto` as your model to let Copilot automatically choose the best available model for each session based on the task at hand.

**How to use:**

```
> /model auto
```

Or start a session with it:

```bash
copilot --model auto
```

**Why it matters:** Removes the cognitive overhead of model selection — Copilot evaluates the task type and picks accordingly, balancing quality and cost automatically.

### Document File Attachments

You can now attach supported document files (PDF, DOCX, and similar) directly to prompts. The agent reads and reasons about the document content inline.

**How to use:**

```
> @report.pdf Summarize the key findings from this document
> @spec.docx Implement the API described in this specification
```

**Why it matters:** Previously limited to code and text files, Copilot can now process richer document formats without requiring you to copy-paste content manually.

### `--connect` Flag — Join a Remote Session Directly

Connect to an existing remote session by its ID without going through the `/resume` picker.

**How to use:**

```bash
copilot --connect <session-id>
```

**Why it matters:** Useful in scripted or CI environments where you need to attach to a specific named session programmatically.

### Short Session ID Prefixes for `--resume` / `/resume`

You no longer need to supply the full session ID when resuming. Any unambiguous prefix of 7 or more hex characters is accepted.

**How to use:**

```
> /resume a3f9b12
```

```bash
copilot --resume a3f9b12
```

### Usage Limit Warnings

The CLI shows inline warnings when you approach your weekly premium request limit. As of v1.0.33, warnings fire at **50%** and **95%** (originally introduced in v1.0.32 at 75% and 90%). This helps you pace usage before hitting the cap mid-session.

### `--print-debug-info` Flag

Print version information, terminal capabilities, and environment variables to stdout and exit. Useful for diagnosing unexpected behavior.

```bash
copilot --print-debug-info
```

### `--session-idle-timeout` Flag

Configure how long an idle session waits before automatically closing. Disabled by default.

```bash
# Close after 30 minutes of inactivity
copilot --session-idle-timeout 30m
```

### Fixes and Improvements

- **Rate limiting**: Paused sessions now queue messages and automatically retry instead of dropping them; error messages include the specific limit type
- **Tables**: Correct column widths, emoji support, and stable borders during terminal resize
- **`/feedback`**: Bundle saves to `TEMP` when the working directory is not writable
- **Rewind**: Works correctly after using `/cd` to change directories
- **Plan mode**: Multiline input is preserved when entering `/plan` or plan mode
- **Status line**: No longer shows stray Unicode glyphs after `/clear` in terminals like Neovim
- **`/ask` dialog**: Mouse wheel scrolling works correctly
- **Shell mode**: Backspace correctly exits shell mode only when input is empty
- **`copilot login --host`**: Now correctly authenticates with GitHub Enterprise Cloud (GHE) instances
- **Agent context**: Current date and time now includes the local timezone offset
- **Skills**: Skills that exceed the context token limit are still discoverable and invocable by name
- **Terminal**: Progress indicator stays visible while the agent is thinking

---

## New in v1.0.31

Released: 2026-04-16

### Rendering Fix for Prompt Frame

The prompt frame no longer causes rendering issues on Windows and Ubuntu terminals. No action required — update to v1.0.31 to get the fix.

---

## New in v1.0.30

Released: 2026-04-16

### `/statusline` — Customize the Status Bar

The new `/statusline` command (also available as `/footer`) lets you choose which items appear in the status bar at the bottom of the CLI:

```
> /statusline
```

**Available items:** `directory`, `branch`, `effort`, `context` (context window usage), `quota` (premium request quota)

**How to use:**

```
# Show current status bar configuration
> /statusline

# Toggle individual items on or off
> /statusline directory
> /statusline branch
> /statusline quota
```

**Why it matters:** If the status bar feels cluttered or you want to focus on specific metrics, you can tailor it to show only what's relevant for your current workflow.

### `/undo` Rewind Unavailable Message

`/undo` now shows a clear explanatory message when rewind is unavailable (e.g., not in a git repository or no commits yet), instead of silently doing nothing.

### Image Paste Restored

Image paste from clipboard works again after a regression in bracketed paste handling. Both `Ctrl+V` and `Meta+V` trigger image paste on all platforms.

---

## New in v1.0.29

Released: 2026-04-16

### Claude Opus 4.7 Model Support

Claude Opus 4.7 is now available in Copilot CLI:

```
> /model claude-opus-4.7
```

See [Models and Costs](22-models-and-costs.md) for the full model comparison table.

### `COPILOT_AGENT_SESSION_ID` Environment Variable

Shell commands and MCP servers now receive `COPILOT_AGENT_SESSION_ID` as an environment variable when invoked by the CLI. This lets external tools and scripts identify which Copilot session called them:

```bash
# Available automatically inside shell commands and MCP server processes
echo $COPILOT_AGENT_SESSION_ID
# → e.g., "abc123-def456-789"
```

**Why it matters:** Useful for logging, telemetry, or coordinating side-effects across tools that need to trace back to a specific Copilot session.

### Remote MCP Server `type` Field Now Optional

When configuring a remote (HTTP) MCP server, the `type` field can now be omitted — it defaults to `http`:

```json
{
  "mcpServers": {
    "my-remote-server": {
      "url": "https://my-mcp-server.example.com/mcp"
    }
  }
}
```

Previously, you had to specify `"type": "http"` explicitly. Existing configs with `"type": "http"` continue to work unchanged.

---

## New in v1.0.28

Released: 2026-04-16

### Remote Control Sessions in `--resume` Picker

The `--resume` session picker now lists active CLI remote control sessions alongside local sessions, so you can connect to a running remote session directly from the picker:

```bash
copilot --resume
```

**Why it matters:** Previously, connecting to a remote session required knowing its ID in advance. Now all resumable sessions — local and remote — appear in one place.

### `COPILOT_DISABLE_TERMINAL_TITLE` Environment Variable

Set this environment variable to prevent Copilot CLI from updating the terminal window/tab title:

```bash
export COPILOT_DISABLE_TERMINAL_TITLE=1
copilot
```

**Why it matters:** Useful in environments where dynamic terminal titles are distracting or interfere with tooling (e.g., multiplexers, CI dashboards).

### Rewind Picker: Simplified Navigation

The rewind picker (opened via `/rewind` or double-Esc) now uses **arrow keys and Enter only**. The previous 1–9 quick-select shortcut has been removed.

```
> /rewind
# Use ↑ / ↓ to select a checkpoint, Enter to confirm
```

**Why it matters:** Removes a confusing shortcut that conflicted with other keybindings and makes the picker behaviour consistent with other pickers in the CLI.

### MCP Migration Hint Links to Docs

When the CLI detects a `.vscode/mcp.json` without a `.mcp.json`, the migration hint at startup now links to platform-specific documentation instead of embedding shell commands directly in the message.

### Bug Fixes

- Permission prompts now show the correct repository path when working inside git submodules
- Background agent completion notifications are no longer sent redundantly when `read_agent` is already waiting for the result
- Azure resource IDs no longer trigger false path security warnings when running `az` CLI commands
- Clear error message is shown when the configured editor cannot be launched
- Mascot plays a short blink sequence on startup instead of blinking continuously

---

## New in v1.0.27

Released: 2026-04-15

### `/ask` Command

A new `/ask` command lets you ask a quick question without adding it to your conversation history.

```
> /ask What does the --allow-all flag do?
```

**Why it matters:** Use `/ask` for one-off lookups, quick clarifications, or exploratory questions when you don't want to pollute the conversation history that the model carries forward. Your next regular prompt will pick up exactly where the conversation left off before the question.

### `copilot plugin marketplace update`

Refresh all configured plugin marketplace catalogs from the shell without opening an interactive session:

```bash
copilot plugin marketplace update
```

**Why it matters:** Useful in CI or onboarding scripts to ensure the local plugin catalog is current before installing plugins non-interactively.

### Status Bar Context Hints

The status bar now shows contextual hints while you type:
- **`@files`** and **`#issues`** hints appear in the status bar to remind you about file and issue references as you compose a prompt
- **`/help`** hint appears in the status bar when the slash command picker is open

### Improved Trial Account Error Messages

When a Copilot Pro trial is paused, the CLI now shows a clear, actionable message instead of a generic policy error — including a direct link to upgrade or revert to Copilot Free.

### WSL Clipboard Fix

Clipboard copy on WSL no longer leaks an invisible BOM (byte-order mark) character into pasted text.

---

## New in v1.0.26

Released: 2026-04-14

### Remote Tab: Coding Agent Tasks

The Remote tab now shows Copilot coding agent tasks and supports steering them without requiring an open pull request.

### Duplicate Instruction File Deduplication

When two instruction files (e.g., `copilot-instructions.md` and `CLAUDE.md`) contain identical content, only one copy is sent to the model — reducing wasted tokens per turn.

### Instruction Files with `applyTo` Patterns: Table Format

Instruction files that use specific `applyTo` patterns are now consolidated into a summary table in the context window rather than inlining their full content. This significantly reduces context window usage for repos with many scoped instruction files.

### Plugin Hook Environment Variables

Plugin hooks now receive three additional environment variables with the plugin's installation directory:

| Variable | Value |
|----------|-------|
| `PLUGIN_ROOT` | Plugin installation directory |
| `COPILOT_PLUGIN_ROOT` | Same as `PLUGIN_ROOT` |
| `CLAUDE_PLUGIN_ROOT` | Same as `PLUGIN_ROOT` |

### ACP Server Localhost Binding

The ACP server now binds to `localhost` only, preventing unintended exposure on other network interfaces.

### Enterprise Login: Hostname Without Scheme

Enterprise login now accepts hostnames without a URL scheme — you can enter `github.example.com` instead of `https://github.example.com`:

```bash
copilot --enterprise-url github.example.com
```

### Session Scope Selector Improvements

The session scope selector in the sync prompt is now more prominent and keyboard-navigable using left/right arrow keys.

### `Ctrl+O`: Expand All Timeline Entries

`Ctrl+O` now expands **all** timeline entries (same behaviour as `Ctrl+E`). Previously, `Ctrl+O` only expanded recent entries.

### Bug Fixes

- Escape key reliably dismisses `ask_user` and elicitation prompts without getting stuck
- Spurious directory access prompts no longer appear for arguments inside `find -exec` blocks
- Agent sessions no longer fail with unrecoverable errors when context compaction splits a tool call across a checkpoint boundary
- Single-segment slash-prefixed tokens (e.g. `/help`, `/start`) no longer treated as file paths in bash commands
- Anthropic BYOM correctly includes image data when viewing image files
- Permission prompt notification hook only fires when a prompt is actually shown to the user
- Relative paths in file edit operations resolve against the session working directory
- LSP language servers correctly initialise on Windows using proper file URI paths
- Installing a plugin named `git` from a marketplace no longer fails due to incorrect URL parsing

---

## New in v1.0.25

Released: 2026-04-13

### `/env` Command

A new `/env` command shows all loaded environment details for the current session at a glance:

```
> /env
```

**Displays:**
- Active instruction files
- Connected MCP servers and their status
- Loaded skills
- Available agents
- Installed plugins

**Why it matters:** Quickly audit the full set of context and tooling loaded into your session — essential for debugging why a tool isn't available or verifying that instructions are in effect.

### Install MCP Servers from the Registry

`/mcp install` opens an interactive browser of the MCP server registry with guided configuration — no manual JSON editing required:

```
> /mcp install
```

Select a server, answer the prompted questions, and the CLI adds the entry to `~/.copilot/mcp-config.json` automatically.

**Why it matters:** Discovering and adding MCP servers is now a guided, in-CLI experience rather than a manual config file task.

### Remote Session Control (`/remote` and `--remote`)

You can now remote-control CLI sessions using the `/remote` slash command or the `--remote` startup flag:

```
> /remote
```

```bash
copilot --remote
```

**Use cases:**
- Monitor a long-running autopilot session from a second terminal
- Steer an active session from a different machine
- View session state without interrupting the active conversation

### `Alt+D` Keyboard Shortcut

`Alt+D` now deletes the word **in front of** the cursor in text input — the forward-delete complement to `Ctrl+W` (which deletes the word behind the cursor).

| Shortcut | Action |
|----------|--------|
| `Ctrl+W` | Delete word behind cursor |
| `Alt+D` | Delete word in front of cursor |

### ACP Clients Can Provide MCP Servers

ACP (Agent Communication Protocol) clients can now supply their own MCP servers (stdio, HTTP, or SSE) when starting or loading sessions. This allows external integrations to inject MCP tooling without requiring changes to the user's config files.

### `/add-dir` Accepts Relative Paths

`/add-dir` now resolves relative paths correctly:

```
> /add-dir ./src
> /add-dir ../sibling-project
```

Paths like `./src` and `../sibling` are resolved to their absolute equivalents before being added to the allowed directory list.

### `/share` File Extension Auto-Appended

When a custom output path is provided without a file extension, `/share` automatically appends the correct extension:
- `/share file ~/my-session` → saves as `~/my-session.md`
- `/share html ~/my-session` → saves as `~/my-session.html`

`/share html` also now displays a `file://` URL in the output so you can open the file directly, and supports `Ctrl+X O` to open it immediately.

### `/logout` Warning for Non-OAuth Auth

`/logout` now shows a clear warning when you are signed in via `gh` CLI, a Personal Access Token, an API key, or the `GH_TOKEN` environment variable — since `/logout` only clears OAuth sessions and cannot remove those credentials.

### Other Improvements

- **Model persistence:** The resolved model is now persisted in session history; model changes are deferred during active turns to avoid mid-turn switches.
- **`--config-dir` respected for model selection:** The active model is now correctly read from the config directory specified by `--config-dir`.
- **MCP remote retry:** Remote MCP server connections automatically retry on transient network failures.
- **Skill picker scroll:** The skill picker list now scrolls correctly when the list exceeds terminal height.
- **Skill instructions persist:** Skill instructions are now correctly maintained across all conversation turns.
- **MCP client version:** The MCP client now reports the correct CLI version during the server handshake.
- **`Esc` after failed `/resume`:** The `Esc` key now works correctly after a failed session lookup.

---

## New in v1.0.24

Released: 2026-04-10

### `preToolUse` Hooks: `modifiedArgs`, `updatedInput`, and `additionalContext`

`preToolUse` hooks can now return three new fields to give the CLI richer control over tool execution:

- **`modifiedArgs`** / **`updatedInput`** — rewrite the tool's input arguments before the tool runs (e.g., enforce path restrictions or inject extra flags).
- **`additionalContext`** — inject a string of context that is appended to the model's tool result, letting hooks supply extra information the model should see after a tool call.

**Example hook response:**
```json
{
  "permissionDecision": "allow",
  "modifiedArgs": { "command": "ls -la /safe/path" },
  "additionalContext": "Note: this directory is read-only in production."
}
```

**Why it matters:** Hooks can now act as middleware — sanitizing inputs before execution and enriching outputs without writing a full custom tool.

> See [Advanced Features](08-advanced-features.md) for the full hook reference.

### Custom Agent `model` Field Accepts VS Code Display Names

The `model` field in a custom agent's frontmatter now accepts the friendly display names and vendor suffixes shown in VS Code, such as `"Claude Sonnet 4.5"` or `"GPT-5.4 (copilot)"`, in addition to canonical model IDs.

```yaml
---
name: my-agent
model: Claude Sonnet 4.5
---
```

**Why it matters:** You can copy a model name directly from the VS Code model picker without needing to look up the exact API identifier.

### Terminal State Restored After Crashes

If the CLI crashes due to an OOM error or segfault, the terminal's alt-screen mode, cursor visibility, and raw-mode settings are now correctly restored. Previously a crash could leave the terminal in a broken state requiring a `reset` command.

### `--remote` Flag Honoured at First-Run Sync Prompt

The `--remote` flag is now respected when the session-sync prompt appears on the first run inside a GitHub repository. Previously the flag was ignored at that specific prompt, causing unexpected local-only behaviour.

### Redesigned Exit Screen

The exit screen has been redesigned with the Copilot mascot and a cleaner usage summary layout showing token consumption, model used, and session duration at a glance.

---

## New in v1.0.23

Released: 2026-04-10

### Launch Flags: `--mode`, `--autopilot`, `--plan`

You can now start the CLI directly in a specific agent mode without going through interactive mode first:

| Flag | Effect |
|------|--------|
| `--mode interactive` | Start in interactive mode (default) |
| `--mode plan` | Start in plan mode — Copilot proposes a plan before acting |
| `--mode autopilot` | Start in autopilot mode — Copilot acts autonomously |
| `--autopilot` | Shorthand for `--mode autopilot` |
| `--plan` | Shorthand for `--mode plan` |

```bash
# Start directly in autopilot mode for a one-shot task
copilot --autopilot "Add input validation to all API handlers"

# Start in plan mode to review the approach before executing
copilot --plan "Refactor the auth module to use JWT"
```

**Why it matters:** CI pipelines and shell scripts can now invoke the right mode directly without needing `/experimental` toggles or `Shift+Tab` keypresses.

> See [Autopilot Mode](17-autopilot-mode.md) and [Plan Mode](09-plan-mode.md) for full usage guides.

### `Ctrl+L` Clears Screen Without Losing Conversation

`Ctrl+L` now clears the visible terminal output while keeping the conversation session fully intact. Previously, clearing the screen could disrupt session state in some terminal emulators.

### Slash Command Picker: Full Skill Descriptions and Refined Scrollbar

The slash command picker (opened by typing `/`) now displays the **full description** of each skill alongside commands, and the scrollbar has been visually refined for better readability in long lists.

### Slash Commands Available While Agent Is Running

`/diff`, `/agent`, `/feedback`, `/ide`, and `/tuikit` can now be invoked while the agent is actively executing a task. Previously these commands were blocked until the agent finished.

### Reasoning Token Usage in Per-Model Breakdown

When a model uses reasoning tokens, the per-model token breakdown (accessible via `/usage`) now shows the reasoning token count separately when it is nonzero.

### Remote Tab: Copilot Coding Agent Tasks and Steering via Tasks API

The Remote tab now correctly lists Copilot coding agent tasks and supports sending steering messages to a running task via the Tasks API — without leaving the CLI.

### MCP Migration Notice Includes `jq` Command

The migration notice shown when a `.vscode/mcp.json` is detected now includes a ready-to-run `jq` command to copy the config to `.mcp.json`:

```bash
jq '.' .vscode/mcp.json > .mcp.json
```

---

## New in v1.0.22

Released: 2026-04-09

### MCP Config Source Consolidated to `.mcp.json`

Copilot CLI now reads MCP server configuration **only from `.mcp.json`** in the project root. Support for `.vscode/mcp.json` and `.devcontainer/devcontainer.json` as MCP config sources has been removed.

If you have a `.vscode/mcp.json` without a corresponding `.mcp.json`, the CLI will display a migration hint at startup.

**Why it matters:** A single canonical config file eliminates ambiguity when multiple config sources existed in the same repository and makes MCP setup easier to audit and version-control.

**Migration:** Copy or rename `.vscode/mcp.json` to `.mcp.json` in your project root:

```bash
cp .vscode/mcp.json .mcp.json
```

> See [MCP Management Commands](08-advanced-features.md#mcp-management-commands) for the full config reference.

### Custom Agents: `skills` Field for Eager Skill Loading

Custom agents can now declare a `skills` field in their frontmatter to **eagerly load skill content into the agent's context at startup**, rather than waiting for `/skills add` during a session.

```yaml
---
name: backend-agent
model: claude-sonnet-4.6
skills:
  - python-expert
  - django-expert
  - security-audit
---
This agent specialises in Django backend development.
```

**Why it matters:** Skills listed in `skills` are injected into the agent's context before the first prompt, so the agent starts with full domain expertise without manual activation steps.

> See [Skills System Guide](14-skills-system.md) for skill file format and locations.

### Sub-Agent Depth and Concurrency Limits

The CLI now enforces configurable **depth and concurrency limits** on sub-agent spawning to prevent runaway agent trees:

- **Depth limit** — caps how many levels of nested agents can be created (e.g., agent → sub-agent → sub-sub-agent).
- **Concurrency limit** — caps how many agents can run in parallel at any given time.

When either limit is hit, the CLI surfaces a clear error instead of silently spawning more agents.

**Why it matters:** Long-running autopilot or fleet tasks that recursively delegated work could previously exhaust memory and API quota. These limits keep resource usage predictable.

### Plugin Improvements

- **Post-install messages**: Plugins can now display setup instructions immediately after installation, so you know exactly what to configure before using the plugin.
- **Session persistence**: Plugins stay enabled across sessions and auto-install on startup based on your saved config — no need to re-enable plugins after restarting.
- **Model respect**: Plugin agents now honour the `model` field in their frontmatter, so a plugin that declares `model: claude-haiku-4.5` will use that model rather than the session default.

### `sessionStart` and `sessionEnd` Hooks: Once Per Session

In interactive mode, `sessionStart` and `sessionEnd` hooks now fire **once per session** instead of once per prompt turn. This matches the expected lifecycle semantics and prevents double-firing when Copilot processes multiple prompts in the same session.

**Why it matters:** Hook scripts that perform setup or teardown work (e.g., loading secrets, writing logs) no longer run redundantly on every prompt.

### Other Improvements and Fixes

- **MCP schema sanitization**: MCP tools with non-standard JSON schemas are now sanitized for compatibility across all model providers — these tools no longer silently fail when used with certain models.
- **Large image handling**: Better handling of large images returned by MCP and extension tools, preventing crashes on high-resolution screenshots or diagrams.
- **Rendering performance**: A new simplified inline renderer improves display performance in long sessions.
- **Policy message**: A clear, actionable message now appears when remote sessions are blocked by an organization policy, directing users to contact their administrator.
- **Sub-agent tool names**: Sub-agent activity no longer shows duplicated tool names (e.g., "view view the file…").
- **BYOM/BYOK hooks**: Permission checks and hook scripts now work correctly when using Anthropic models via BYOM/BYOK configuration.
- **Slash command picker**: Now appears above the text input for a more stable layout.
- **Session conflict warning**: A warning is shown when resuming a session that is already open in another CLI instance or application.
- **V8 crash fix**: The CLI no longer crashes on systems affected by a V8 engine bug in grapheme segmentation.

---

## New in v1.0.21

Released: 2026-04-07

### `copilot mcp` — CLI Command for MCP Server Management

A new top-level CLI subcommand lets you manage MCP servers directly from your shell, without needing to start an interactive session:

```bash
$ copilot mcp
```

**Why it matters:** Previously MCP servers could only be added, disabled, or removed via `/mcp` slash commands inside an active Copilot session. The `copilot mcp` command gives you full MCP management from the shell — useful for scripts, CI setup, and one-time configuration changes.

> See [MCP Management Commands](08-advanced-features.md#mcp-management-commands) in the Advanced Features guide for the full list of subcommands.

### Hook Payloads Normalized to `snake_case`

Hook scripts that use **PascalCase event names** (e.g., `PreToolUse`, `PostToolUse`) now receive VS Code-compatible `snake_case` payloads. Each payload includes:

- `hook_event_name` — the snake_case name of the event
- `session_id` — the current session identifier
- Timestamps formatted as **ISO 8601** strings

Hooks already using `snake_case` event names are unaffected.

**Why it matters:** Hook scripts can now be shared between VS Code and Copilot CLI without conditional payload-handling logic, since both environments produce the same payload shape.

### Other Improvements

- **Spinner**: No longer appears stuck when a long-running async shell command is active.
- **Login flow**: Enterprise GitHub URL input now accepts keyboard input and submits on Enter.
- **Slash command picker**: No longer flickers or shifts the input while filtering.
- **Timeline**: No longer goes blank when content shrinks (e.g., after cancelling or tool completion).
- **Plan mode**: Timeline display shows user text without a redundant "Plan" prefix.
- **Memory**: Idle shell sessions are automatically shut down to reduce memory usage.

---

## New in v1.0.20

Released: 2026-04-07

### `copilot help monitoring` — OpenTelemetry Configuration Guide

A new built-in help topic covers OpenTelemetry monitoring in detail, including configuration options, environment variables, and examples for wiring up tracing backends.

```bash
$ copilot help monitoring
```

**Why it matters:** All OpenTelemetry configuration details — span kinds, attribute names, exporter setup — are now a single command away without leaving the terminal.

### `/yolo` Slash Command — Persists Across `/restart`

`/yolo` and `--yolo` now behave identically. In addition, `/yolo` state **persists across `/restart`** — you no longer need to re-enable it after restarting the session.

```
> /yolo
> /restart   # /yolo stays active
```

**Why it matters:** Avoids repeatedly re-enabling unrestricted mode during iterative autopilot sessions that use `/restart` to reset context.

### Azure OpenAI BYOK: Versionless v1 Route Default

When no API version is configured, Azure OpenAI BYOK connections now default to the GA **versionless v1 route** (`/openai/deployments/{deployment}/chat/completions?api-version=`). This eliminates errors caused by stale or missing API version strings.

**Why it matters:** BYOK Azure setups that previously required explicit API version pinning now work correctly out of the box.

### Spinner Active Until Background Work Completes

The activity spinner now stays visible until all **background agents and shell commands** finish — not just until the model stops streaming. User input remains available throughout.

**Why it matters:** Gives a clear visual signal that Copilot is still working, preventing premature follow-up messages that interrupt in-flight operations.

---

## New in v1.0.19

Released: 2026-04-06

### `/mcp enable` and `/mcp disable` Persist Across Sessions

MCP server enable/disable state is now saved between sessions. Previously, disabling an MCP server with `/mcp disable` was session-local and the server would re-enable on next launch.

```
> /mcp disable heavy-server   # now saved permanently until re-enabled
> /mcp enable heavy-server    # restore and save
```

**Why it matters:** No more re-running `/mcp disable` commands on startup for servers you rarely use.

### OpenTelemetry Monitoring Improvements

- Subagent spans now use **INTERNAL** span kind (previously unset), making agent hierarchy clearer in tracing backends.
- Chat spans now include a `github.copilot.time_to_first_chunk` attribute for streaming sessions, enabling first-token latency tracking.

### Slash Command Timeline Entries Now Include Command Name

The session timeline now labels slash command entries with the command name (e.g., "Review", "Plan") instead of a generic entry.

**Why it matters:** Makes session history and replays easier to navigate when a session contains multiple slash commands.

### Other Fixes

- Plugin hook scripts with missing execute permissions now run correctly on macOS.
- Custom agent is properly restored when resuming a session where the agent display name differs from its filename.
- IDE auto-connect is skipped when the session is already in use by another client.

---

## New in v1.0.18

Released: 2026-04-04

### Critic Agent (Experimental)

A new **Critic agent** automatically reviews plans and complex implementations using a complementary model to catch errors early. The Critic runs alongside the primary agent and provides an independent second opinion before changes are finalized.

The Critic agent is available in **experimental mode for Claude models only**.

**How to enable:**

```
> /experimental
```

Once experimental mode is active, the Critic agent runs automatically during plan reviews and complex multi-step implementations — no additional configuration required.

**Why it matters:** An independent model reviewing the work catches logical errors, missed edge cases, and architectural mistakes before they become code — without requiring you to manually review every step.

### Notification Hook Event

A new `notification` hook event fires **asynchronously** when significant events occur during a session:

- Shell command completes
- Permission prompt appears
- Elicitation dialog opens
- Agent completes a task

```json
{
  "hooks": {
    "notification": [
      {
        "command": "scripts/notify.sh"
      }
    ]
  }
}
```

Because the hook fires asynchronously, it does not block the agent or add latency to the session.

**Why it matters:** Lets you wire up desktop notifications, Slack alerts, or custom logging for any significant event during a long-running Copilot session.

### `preToolUse` Hook: `allow` Now Suppresses Approval Prompt

When a `preToolUse` hook returns `permissionDecision: 'allow'`, the tool approval prompt is now fully suppressed. Previously the hook could grant approval programmatically, but the UI still displayed the confirmation prompt.

```json
{
  "hooks": {
    "preToolUse": [
      {
        "command": "scripts/auto-approve-safe-tools.sh"
      }
    ]
  }
}
```

**Why it matters:** Enables seamless, prompt-free tool approvals in automated workflows without requiring `--allow-all`.

### Other Fixes

- Session resume picker now correctly groups sessions by branch and repository on first use.

---

## New in v1.0.17

Released: 2026-04-03

### Built-in Skills Now Included with the CLI

Starting in v1.0.17, the CLI ships with a set of built-in skills out of the box. The first included skill is a guide for customizing Copilot cloud agent's environment — available without any manual installation or configuration.

```
> /skills list
```

Look for skills marked `(built-in)` in the output. They are ready to use immediately on any install.

**Why it matters:** Previously all skills had to be created or installed manually. Built-in skills provide useful capabilities with zero setup.

### MCP OAuth: HTTPS Redirect URI Fallback

MCP OAuth flows now support HTTPS redirect URIs via a self-signed certificate fallback. This improves compatibility with OAuth providers that require HTTPS (e.g., Slack).

No configuration change is required — the fallback activates automatically when an OAuth provider rejects an HTTP redirect.

**Why it matters:** Enables MCP OAuth with providers that mandate HTTPS, such as Slack, without needing a full TLS setup.

### Faster `/resume` Session Picker

The `/resume` session picker now loads significantly faster, especially for users with large session histories.

```
> /resume
```

**Why it matters:** Large session histories no longer cause a noticeable delay when picking up previous work.

---

## New in v1.0.16

Released: 2026-04-02

### PermissionRequest Hook

A new `PermissionRequest` hook lets scripts programmatically approve or deny tool permission requests. This is particularly useful in CI/CD pipelines and automated workflows where interactive prompts aren't possible.

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "command": "scripts/approve-tool.sh"
      }
    ]
  }
}
```

The hook script receives the permission request details and exits `0` to approve or non-zero to deny.

**Why it matters:** Enables fully automated, non-interactive Copilot runs without using `--allow-all`.

### MCP Tool Calls Shown in Timeline

MCP tool calls now display the tool name and a parameter summary directly in the session timeline, making it easier to audit exactly what each MCP server did during a session.

**Why it matters:** Improved observability for sessions that use MCP servers heavily.

### `postToolUseFailure` Hook

A new `postToolUseFailure` hook fires when a tool call fails, enabling custom error-handling scripts. The existing `postToolUse` hook now only fires on successful tool calls (previously it fired on both success and failure).

**Why it matters:** Lets you react specifically to tool errors — for example, logging failures or sending alerts — without duplicating logic in `postToolUse`.

### Deprecated: `marketplaces` Config Key

> ⚠️ **Removed in v1.0.16:** The `marketplaces` repository config setting has been removed. Use `extraKnownMarketplaces` instead.

```json
// ❌ Old (removed)
{ "marketplaces": ["https://plugins.example.com/registry.json"] }

// ✅ New
{ "extraKnownMarketplaces": ["https://plugins.example.com/registry.json"] }
```

### Other Fixes and Improvements

- SQL prompt tags no longer appear when the `sql` tool is excluded via `excludedTools` or `availableTools`
- MCP servers reconnect correctly with valid authentication when the working directory changes
- MCP servers load correctly after login, user switch, and `/mcp reload`
- BYOK Anthropic provider now respects the configured `maxOutputTokens` limit

---

## New in v1.0.15

Released: 2026-04-01

### `/share html` — Export Session as Interactive HTML

Export your session or research report as a self-contained interactive HTML file — no GitHub account required to view it.

```
> /share html
> /share html ~/reports/my-session.html
```

**Why it matters:** Easily share Copilot sessions with teammates or stakeholders who don't have CLI access.

### `/mcp auth` — MCP OAuth Re-authentication

Authenticate or re-authenticate an MCP OAuth server, with account switching support.

```
> /mcp auth my-server
```

**Why it matters:** Fixes auth expiry without needing to remove and re-add a server.

### Config Keys Now Use camelCase

Config settings now prefer camelCase names. snake_case still works for backwards compatibility.

| Old key | New key |
|---------|---------|
| `ask_user` | `askUser` |
| `auto_update` | `autoUpdate` |
| `store_token_plaintext` | `storeTokenPlaintext` |
| `log_level` | `logLevel` |
| `skill_directories` | `skillDirectories` |
| `disabled_skills` | `disabledSkills` |

### Autopilot No Longer Resumes After Cancel

Pressing **Escape** or **Ctrl+C** now fully stops Autopilot. Previously, autopilot could continue after a cancel under some conditions.

### Ctrl+D No Longer Queues Messages

`Ctrl+D` is now shutdown-only. Use `Ctrl+Q` or `Ctrl+Enter` to queue a message while the agent is running.

### Model Removals

The following models were removed in v1.0.15:
- `gpt-5.1-codex`
- `gpt-5.1-codex-mini`
- `gpt-5.1-codex-max`

> ⚠️ **Removed in v1.0.15:** If you were using `gpt-5.1-codex*` models, switch to `gpt-5.3-codex` or `gpt-5.2-codex`. Run `/model` to see current options.

---

## New in v1.0.13

Released: 2026-03-30

### `/rewind` Timeline Picker

`/rewind` and double-Esc now open a **timeline picker** to roll back to any point in conversation history — not just the previous snapshot.

```
> /rewind
# or press Esc Esc to open the picker
```

**Why it matters:** Recover from mistakes several turns back without losing all context.

### MCP Servers Can Request LLM Inference

MCP servers can now request LLM inference (sampling) with user approval via a review prompt. This allows MCP tools to leverage AI reasoning as part of their workflows.

### Model Removals

`gemini-3-pro-preview` was removed in v1.0.13.

> ⚠️ **Removed in v1.0.13:** If you were using `gemini-3-pro-preview`, switch to another available model via `/model`.

---

## New in v1.0.11

### `/rewind` / `/undo` — Undo Last Turn

Rewind the last turn and revert all file changes made during it. Useful when an AI action went wrong.

```
> /rewind
```

Both `/rewind` and `/undo` do the same thing.

### `/context` — Token Usage Visualization

Show a breakdown of the context window token usage, including a visualization of what's taking up space.

```
> /context
```

### `/compact` — Summarize Conversation History

Compress the current conversation to reduce context window usage while retaining key information.

```
> /compact
```

### New Model Lineup

v1.0.11 adds several new models to the picker:

| Model | ID |
|-------|----|
| Claude Sonnet 4.6 | `claude-sonnet-4.6` |
| Claude Opus 4.6 | `claude-opus-4.6` |
| Claude Opus 4.6 (fast) | `claude-opus-4.6-fast` |
| GPT-5.4 mini | `gpt-5.4-mini` |
| GPT-4.1 | `gpt-4.1` |

Run `/model` to see all available models with current multipliers.

### `/streamer-mode` — Safe Streaming

Toggle streamer mode to hide preview model names and quota details — useful when screen-sharing or live streaming.

```
> /streamer-mode
```

### Keyboard Shortcut Updates

Several keyboard shortcuts changed meaning in v1.0.11. See the full table in [Keyboard Shortcuts Reference](#keyboard-shortcuts-reference) below.

---

## Autopilot Mode (Experimental)

Autopilot mode lets Copilot work end-to-end on a task without pausing for your input at every step. Enable experimental features first, then press **Shift+Tab** to cycle to autopilot mode.

```
interactive  →  plan  →  autopilot  →  (back to interactive)
```

> **Note:** Without experimental mode active, `Shift+Tab` only cycles between **interactive** and **plan**. Enabling `/experimental` adds autopilot to the cycle.

```bash
# Enable experimental features first
copilot --experimental

# Or from inside the CLI
> /experimental
```

**✅ Good for:** Large refactors, end-to-end feature work, CI fixes, batch operations with clear success criteria.  
**❌ Avoid for:** Exploratory work, open-ended tasks, or sensitive production configs where you want step-by-step approval.

> **See the full guide:** [Autopilot Mode →](17-autopilot-mode.md)
> Covers permissions, `--max-autopilot-continues`, the plan→autopilot workflow, and four detailed examples.

---

## Fleet Mode (`/fleet`)

Fleet mode enables **parallel subagent execution** — the main Copilot agent acts as an orchestrator, breaking your request into independent subtasks and running them concurrently via subagents. Useful for large refactors, test generation, and any multi-part work with independent pieces.

```
> /fleet Add unit tests for every service in src/services/

[Orchestrator spawns one subagent per file — all run concurrently]
```

Monitor progress with `/tasks`. Navigate with `↑↓`, press `Enter` for details, `k` to kill, `r` to remove.

**Best combined with plan mode:** draft a plan, then choose **Accept plan and build on autopilot + /fleet** for fully autonomous parallel execution.

> **See the full guide:** [Fleet Mode →](18-fleet-mode.md)
> Covers the orchestrator model, custom agents, per-subtask model selection, cost considerations, and four detailed examples.

---

## Research Command (`/research`)

`/research` activates a **specialized research agent** that gathers information from your codebase, GitHub repos, and the web, then produces a comprehensive cited Markdown report. It is not a mode — it's a slash command for deep investigation work.

```
> /research How does authentication work in this codebase?

[Agent searches codebase, GitHub, and web — compiles full report]

Summary: Auth uses JWT tokens (src/auth/), sessions stored in Redis,
         refresh tokens handled separately in src/auth/tokens.js.

Full report: ~/.copilot/session-state/.../research/authentication.md
Press Ctrl+Y to open.
```

The agent classifies your query (process / conceptual / technical deep-dive) and adapts the report format. It uses a **fixed built-in model** regardless of your `/model` setting.

**Share the report:**
```
> /share gist research      # Publish as a GitHub Gist
> /share file research      # Save as a local Markdown file
```

> **See the full guide:** [Research Command →](19-research-command.md)
> Covers query-type classification, `Ctrl+Y`, sharing, finding past reports, and six example prompts with explanations.

---

## LSP (Language Server Protocol) Support

### Overview

Copilot CLI now supports **Language Server Protocol (LSP)** integration, enabling rich code intelligence features: go-to-definition, hover documentation, and inline diagnostics. LSP servers run alongside the CLI and feed the AI more precise context about your code.

### Installing an LSP Server

LSP servers are installed separately via your package manager. Example for TypeScript:

```bash
npm install -g typescript-language-server typescript
```

Other common LSP servers:

```bash
# Python
pip install python-lsp-server

# Rust
rustup component add rust-analyzer

# Go
go install golang.org/x/tools/gopls@latest
```

### Configuration

#### User-Level Config

Create or edit `~/.copilot/lsp-config.json` to define LSP servers globally:

```json
{
  "lspServers": {
    "typescript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".ts": "typescript",
        ".tsx": "typescript"
      }
    },
    "javascript": {
      "command": "typescript-language-server",
      "args": ["--stdio"],
      "fileExtensions": {
        ".js": "javascript",
        ".jsx": "javascript"
      }
    }
  }
}
```

#### Repository-Level Config

For project-specific LSP settings, create `.github/lsp.json` in the repository root:

```json
{
  "lspServers": {
    "python": {
      "command": "pylsp",
      "args": [],
      "fileExtensions": {
        ".py": "python"
      }
    }
  }
}
```

Repository-level config takes precedence over user-level config for matching file extensions.

### Checking LSP Status

```
> /lsp

LSP Servers:
  typescript  ✅ Running  (pid 12345)
  python      ❌ Not configured
```

### What LSP Enables

With LSP active, the AI can:
- **Go-to-definition** — trace where a symbol is declared
- **Hover information** — get type signatures and documentation
- **Diagnostics** — see compiler errors and warnings inline
- **Rename symbols** — safely refactor identifiers across files
- **Find references** — understand where a function or variable is used

---

## Code Review Agent (`/review`)

### Overview

The `/review` command runs a dedicated, high-signal code review agent on your current changes. It focuses exclusively on what matters: bugs, security vulnerabilities, and logic errors — not style or formatting.

### Basic Usage

```
> /review

Code Review Agent
━━━━━━━━━━━━━━━━
Analyzing staged changes...

🔴 CRITICAL: src/auth/login.js (line 42)
   SQL query built with string concatenation — potential SQL injection.
   Fix: Use parameterized queries.

🟡 WARNING: src/api/upload.js (line 18)
   No file size validation before writing to disk.
   Fix: Check Content-Length header and enforce a maximum.

✅ src/utils/format.js — No issues found.
✅ src/models/user.js — No issues found.
```

### Reviewing Before Committing

Use `/review` alongside `/diff` for a pre-commit workflow:

```
> /diff        # See what changed
> /review      # Get the AI review
> /delegate    # Create the PR when ready
```

### Reviewing Specific Files

```
> /review @src/payments/processor.js

[Focuses the review agent on just that file]
```

### Integration with PR Workflow

After review, act on findings without leaving the CLI:

```
> /review
[AI finds a potential null-pointer dereference]

> Fix the issue the review agent found in src/api/orders.js

[AI applies the fix]

> /delegate Fix null-pointer issue in order processing
```

---

## Plugin System (`/plugin`)

### Overview

The plugin system lets you extend Copilot CLI with additional capabilities beyond the built-in tool set. Plugins can add new slash commands, custom skills, and integrations with external services.

### Browsing Available Plugins

```
> /plugin list

Available Plugins:
  copilot-jira       — Jira issue integration
  copilot-datadog    — Datadog metrics and logs
  copilot-terraform  — Terraform plan and apply assistance
  copilot-k8s        — Kubernetes cluster management
```

### Installing a Plugin

```
> /plugin install copilot-jira

Installing copilot-jira...
✅ Plugin installed. New command available: /jira
```

### Managing Marketplaces

```
> /plugin marketplace add https://plugins.example.com/registry.json

Marketplace added. Run /plugin list to see new plugins.
```

### Removing a Plugin

```
> /plugin remove copilot-jira

✅ copilot-jira removed.
```

---

## Keyboard Shortcuts Reference

The full list of keyboard shortcuts in v1.0.11:

### Navigation & Control

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Cancel current input / interrupt / copy selection |
| `Ctrl+C` × 2 | Exit Copilot CLI |
| `Ctrl+D` | Shutdown |
| `Ctrl+L` | Clear screen |
| `Esc` | Cancel the current operation |
| `↑ / ↓` | Navigate command history |
| `Shift+Tab` | Cycle modes: interactive → plan (autopilot requires `/experimental`) |
| `Ctrl+S` | Stash/pop current prompt (v1.0.60) |
| `Ctrl+T` | Toggle model reasoning display |
| `!` | Execute command in local shell (bypass Copilot) |

### Timeline

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Expand all timeline entries (when no input) |
| `Ctrl+E` | Expand all timeline entries (when no input) |
| `Ctrl+X → O` | Open link from most recent timeline event |

### Text Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line (when typing) |
| `Ctrl+W` | Delete previous word |
| `Ctrl+U` | Delete from cursor to beginning of line |
| `Ctrl+K` | Delete from cursor to end of line |
| `Ctrl+H` | Delete previous character (backspace) |
| `Meta+← / →` | Move cursor by word |
| `Ctrl+G` | Edit prompt in external editor |

### Using `Ctrl+G` (External Editor)

This is especially useful for long, multi-line prompts:

```
# Start typing a prompt
> Implement a new feature that...

# Press Ctrl+G — your $EDITOR opens with the prompt text
# Edit in full-screen, save, and close
# The edited prompt is sent automatically
```

### Using `Ctrl+X → O` (Open Link)

After any operation that produces a URL (a PR, a research result, a share link):

```
> /delegate Fix authentication bug

🔗 PR #147 created: https://github.com/user/repo/pull/147

# Press Ctrl+X then O to open that URL in your browser
```

---

## PAT Authentication

### Overview

In addition to `gh auth login`, Copilot CLI supports authentication via a **fine-grained Personal Access Token (PAT)**. This is useful for CI/CD environments, shared machines, or scenarios where interactive OAuth isn't feasible.

### Creating a PAT

1. Visit: https://github.com/settings/personal-access-tokens/new
2. Set a token name and expiration
3. Under **Permissions**, grant **"Copilot Requests"** (read/write)
4. Click **Generate token** and copy the value

### Setting the Token

```bash
# Set for the current shell session
export GH_TOKEN=github_pat_your_token_here

# Or use GITHUB_TOKEN (both are supported)
export GITHUB_TOKEN=github_pat_your_token_here
```

To persist across sessions, add the export to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.).

### Verifying Authentication

```bash
gh auth status
# Should show: ✓ Logged in to github.com as <username>
```

### When to Use PAT Auth

- **CI/CD pipelines** — non-interactive environments
- **Docker containers** — where browser auth isn't available
- **Service accounts** — automated workflows with minimal permissions
- **Multiple accounts** — switch between tokens for different organizations

---

## Extended Instructions Support

### Overview

Copilot CLI now reads custom instructions from a wider set of locations, giving you flexible control over AI behavior at the user, project, and team level.

### Instruction File Locations (in priority order)

| Location | Scope |
|----------|-------|
| `$HOME/.copilot/copilot-instructions.md` | User-level, applies everywhere |
| `.github/copilot-instructions.md` | Repository-level |
| `.github/instructions/**/*.instructions.md` | Repository-level, per-topic files |
| `AGENTS.md` (git root and cwd) | Project-level agent instructions |
| `CLAUDE.md` | Project-level (Claude-compatible format) |
| `GEMINI.md` | Project-level (Gemini-compatible format) |
| `COPILOT_CUSTOM_INSTRUCTIONS_DIRS` env var | Additional directories (colon-separated) |

### Example: Repository-Level Instructions

```markdown
<!-- .github/copilot-instructions.md -->

## Project: Payments Service

- Always use parameterized queries — never string interpolation in SQL
- All monetary values must use the `Money` type from `src/types/money.ts`
- PRs must include a test for every changed code path
- Follow the error handling patterns in `src/utils/errors.ts`
```

### Example: Per-Topic Instructions

```markdown
<!-- .github/instructions/testing.instructions.md -->

## Testing Standards

- Use Vitest for all unit and integration tests
- Test file naming: `*.test.ts` in the same directory as the source
- Minimum 80% branch coverage for new files
- Always test error paths, not just the happy path
```

### Example: Additional Directories via Env Var

```bash
# Include instructions from multiple directories
export COPILOT_CUSTOM_INSTRUCTIONS_DIRS="/team/shared-instructions:/project/instructions"
```

### How Instructions Are Combined

All applicable instruction files are merged and provided to the model as context. More specific scopes (repository-level) take precedence when there are conflicts with user-level instructions.

---

## Project Initialization (`/init`)

### Overview

The `/init` command scaffolds complete projects from scratch. It asks targeted questions, makes smart configuration choices, and sets up everything you need to start coding immediately.

### Basic Usage

#### Interactive Mode (Recommended)

```
> /init

AI: What type of project would you like to create?

1. Frontend (React, Vue, Angular, Next.js)
2. Backend API (Express, FastAPI, Flask)
3. Full-Stack (Next.js, MERN, Django)
4. Mobile (React Native, Flutter)
5. Desktop (Electron, Tauri)
6. CLI Tool (Node, Python, Rust)
7. Other

Select option (1-7):
> 1
```

#### Direct Mode

```
> /init react-app
> /init nextjs
> /init express-api
> /init python-flask
> /init rust
```

### Supported Project Types

- **Frontend:** React (Vite), Next.js (App Router), Vue 3, Angular, Svelte
- **Backend:** Express, FastAPI, Flask, NestJS, Django
- **Full-Stack:** Next.js full-stack, MERN, T3 Stack
- **Mobile:** React Native, Flutter
- **CLI Tools:** Node, Python, Rust, Go

### Example: React App

```
> /init react-app

AI Questions:
- Project name? my-app
- TypeScript? yes
- Build tool? Vite
- State management? Zustand
- UI Library? Tailwind CSS

AI: Generating project...

✅ Project created: my-app/
   - React 18 + TypeScript
   - Vite build tool
   - Zustand for state
   - Tailwind CSS
   - ESLint + Prettier
   - Vitest for testing

Next steps:
  cd my-app
  npm install
  npm run dev
```

### Tips for Using `/init`

**✅ Do:**
- Use interactive mode when exploring options
- Review the generated structure before committing
- Use for new projects and learning setups

**❌ Don't:**
- Use in an existing project directory (will conflict)
- Skip the generated `README.md` — it contains important setup steps
- Forget to configure required environment variables

---

## Enhanced Pull Request Creation (`/delegate`)

### Overview

The `/delegate` command automates end-to-end PR creation: it implements changes, runs tests, creates a well-described pull request, and handles reviewer assignment — all from a single instruction.

### Core Features

#### Intelligent Issue Linking

```
> /delegate Implement the feature described in issue #123

AI: Analyzing issue #123...
    Title: "Add user profile editing"
    I'll implement this with:
    - Profile edit form
    - API endpoint for updates
    - Input validation + tests
    
    Linked issue #123 will be auto-closed when PR merges.
    Proceed? (y/n)
```

#### Pre-Push Test Validation

```
> /delegate Fix authentication bug
  Run tests before pushing

AI: Making changes...
   ✓ src/auth/login.js
   ✓ tests/auth.test.js

🧪 Running test suite...
   ✓ auth.test.js (12 tests)
   ✓ integration.test.js (5 tests)
   All tests passed ✓ — safe to push? (y/n)
```

#### Draft PR Support

```
> /delegate --draft Experimental: try Redis caching

🔗 Created DRAFT PR #145
   This will not trigger CI/CD or request reviews.
   Mark as "Ready for review" when done.
```

#### Custom Branch Strategy

```
> /delegate --branch feature/user-profile-v2 Add profile editing
> /delegate --base develop Add new feature
```

#### Multi-File Change Intelligence

```
> /delegate Update all API endpoints to use async/await

AI: Found 23 files with API endpoints.
    Updating all to async/await with consistent error handling...

🔧 Updating 23 files...
🧪 Running tests... ✓ All passed
🔗 PR #146 created with comprehensive changeset
```

### Advanced Patterns

```
# Fix + regression tests
> /delegate Fix bug #234
  Add regression tests
  Run full test suite before pushing

# Feature + documentation
> /delegate Implement OAuth2 authentication
  Include implementation, API docs, and migration guide

# Refactor + verify
> /delegate Refactor database layer to repository pattern
  Ensure no breaking changes and all tests pass
```

---

## Staying Up to Date

### Checking Your Version

```bash
copilot --version
# Example: GitHub Copilot CLI version 1.0.11
```

### Updating

```bash
# Homebrew (macOS/Linux)
brew update && brew upgrade copilot-cli

# npm global install
npm update -g @github/copilot

# WinGet (Windows)
winget upgrade GitHub.Copilot

# Or use the built-in update command
> /update
```

### Checking for Experimental Features

New experimental features are gated behind the `--experimental` flag or `/experimental` command. Check the release notes after each update to discover what's newly available:

```
> /experimental
> /help
```

### Getting Help

```
> /help           # Full command reference
> /init --help    # Init-specific options
> /delegate --help
> /lsp            # LSP server status
> /plugin list    # Available plugins
```

---

**Previous:** [Copilot Directory](15-copilot-directory.md)  
**Related:** [Slash Commands](04-slash-commands.md) | [Examples](12-examples.md) | [Troubleshooting](11-troubleshooting.md)
