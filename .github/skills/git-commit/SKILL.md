---
name: git-commit
description: Expertise in staging, committing, and pushing git changes with clear commit messages and safe workflows.
allowed-tools: bash
---

# Git Commit Skill

## Metadata
- **ID**: git-commit
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: DevOps
- **Tags**: git, commit, push, staging, version-control

## Description

Expertise in staging git changes, writing clear commit messages, and pushing to a remote. Handles partial staging, commit message conventions, pre-push checks, and recovery from common mistakes.

## Capabilities

- Inspect working tree status and diffs before committing
- Stage all changes or selectively stage specific files/hunks
- Write clear, conventional commit messages
- Include upstream release links in release-driven commit messages
- Amend or reword the most recent commit
- Push to the correct remote branch
- Handle diverged branches and upstream conflicts
- Recover from common git mistakes (wrong files staged, bad commit message)
- Follow Co-authored-by trailer conventions for this repo

## When to Use

- User wants to commit and push changes
- User asks how to stage specific files or hunks
- User needs help writing a good commit message
- User's push was rejected and needs to resolve it
- User accidentally committed the wrong thing

## Instructions

### Step 1 — Inspect Before You Commit

Always start by reviewing the current state of the working tree:

```bash
git --no-pager status
```

Then review exactly what changed:

```bash
git --no-pager diff          # unstaged changes
git --no-pager diff --cached # already-staged changes
```

Use `--stat` for a concise summary when there are many files:

```bash
git --no-pager diff --stat
```

### Step 2 — Stage Changes

**Stage everything:**
```bash
git add -A
```

**Stage specific files:**
```bash
git add path/to/file.js
git add src/components/
```

**Stage interactively (choose hunks):**
```bash
git add -p
```

**Unstage a file you didn't mean to add:**
```bash
git restore --staged path/to/file.js
```

**Discard unstaged changes in a file:**
```bash
git restore path/to/file.js
```

### Step 3 — Write a Good Commit Message

Follow the **Conventional Commits** format:

```
<type>(<scope>): <short summary>

<optional body — explain the WHY, not the what>

<optional footer — breaking changes, issue refs, co-authors>
```

**Common types:**
| Type | When to use |
|------|------------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation changes only |
| `refactor` | Code restructuring, no behavior change |
| `chore` | Build, tooling, dependency updates |
| `test` | Adding or fixing tests |
| `style` | Formatting, whitespace — no logic change |
| `perf` | Performance improvement |

**Good examples:**
```
feat(auth): add JWT refresh token rotation

fix(api): handle null user in /profile endpoint

docs: update guide for v1.0.18

Release: https://github.com/github/copilot-cli/releases/tag/v1.0.18

chore: move skills to .github/skills/ directory
```

**Bad examples:**
```
❌ fix stuff
❌ WIP
❌ changes
❌ asdfasdf
```

**Subject line rules:**
- 50 characters or fewer
- Imperative mood ("add", "fix", "update" — not "added", "fixed")
- No full stop at the end
- Capitalize the first letter after the colon

**Body rules (when needed):**
- Wrap at 72 characters
- Explain *why* the change was made, not *what* (the diff shows the what)
- Separate from subject with a blank line

**Release-driven updates:**
- If the commit updates docs or automation for a specific GitHub Copilot CLI release, include the full GitHub release URL in the body
- Prefer a dedicated line such as `Release: https://github.com/github/copilot-cli/releases/tag/v1.0.18`
- Use the exact tag from the commit subject/body so reviewers can jump straight to the upstream release notes

### Step 4 — Commit

**Standard commit:**
```bash
git commit -m "feat(scope): short summary"
```

**Commit with body:**
```bash
git commit -m "feat(scope): short summary

Longer explanation of why this change was made and any
context that reviewers will find helpful.

Closes #42"
```

**Release update commit:**
```bash
git commit -m "docs: update guide for v1.0.18

Release: https://github.com/github/copilot-cli/releases/tag/v1.0.18"
```

**Co-authored-by trailer** (required for commits made with Copilot in this repo):
```bash
git commit -m "feat: my change

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>"
```

**Amend the last commit** (before pushing):
```bash
# Amend message only
git commit --amend -m "corrected: better message"

# Amend and add forgotten files
git add forgotten-file.js
git commit --amend --no-edit
```

### Step 5 — Push

**Push to the current branch's upstream:**
```bash
git push
```

**Push and set upstream (first push of a new branch):**
```bash
git push -u origin HEAD
```

**Push to a specific remote and branch:**
```bash
git push origin main
git push origin feature/my-branch
```

**Force push after amend** (only on non-shared branches):
```bash
git push --force-with-lease
```

> ⚠️ Never force-push to `main` or any shared branch. Always prefer `--force-with-lease` over `--force` — it refuses if someone else has pushed in the meantime.

### Handling Push Rejections

**Rejection: remote has new commits you don't have**

```
! [rejected] main -> main (fetch first)
```

Pull and rebase:
```bash
git pull --rebase
git push
```

Or merge (creates a merge commit):
```bash
git pull
git push
```

**Rejection: diverged branches**

```
! [rejected] main -> main (non-fast-forward)
```

1. Fetch and inspect what's different:
   ```bash
   git fetch origin
   git --no-pager log --oneline HEAD..origin/main
   ```
2. Rebase onto the remote:
   ```bash
   git rebase origin/main
   ```
3. Resolve any conflicts, then:
   ```bash
   git rebase --continue
   git push
   ```

### Full Workflow — Quick Reference

```bash
# 1. Check what changed
git --no-pager status
git --no-pager diff --stat

# 2. Stage
git add -A                   # all changes
# or
git add path/to/file.js      # specific file

# 3. Commit
git commit -m "type(scope): summary"

# 4. Push
git push                     # existing upstream
git push -u origin HEAD      # new branch
```

### Common Pitfalls

- ❌ Committing without reviewing the diff first — always run `git diff --cached` before `git commit`
- ❌ Vague commit messages like "fix" or "changes" — future-you will thank present-you for detail
- ❌ Staging everything with `git add -A` when some files (logs, secrets, build artifacts) shouldn't be committed — check `.gitignore` first
- ❌ Force-pushing to shared branches — use `--force-with-lease` on personal branches only
- ❌ Committing secrets or credentials — use `git diff --cached` to scan before committing

### Best Practices

- ✅ Commit small, focused changes — one logical change per commit
- ✅ Always run `git status` and `git diff --stat` before staging
- ✅ Use `git add -p` for surgical staging when a file has multiple unrelated changes
- ✅ Write the commit message for the person who will review it six months from now
- ✅ Verify the push succeeded by checking the remote or running `git --no-pager log --oneline -5`
- ✅ Use `git pull --rebase` instead of `git pull` to keep history linear

## Checklists

### Before Every Commit
- [ ] `git status` — no unintended files staged
- [ ] `git diff --cached` — changes look correct
- [ ] No secrets, credentials, or debug code included
- [ ] `.gitignore` up to date if new file types were added

### Commit Message
- [ ] Type prefix used (`feat`, `fix`, `docs`, etc.)
- [ ] Subject ≤ 50 characters
- [ ] Imperative mood ("add", not "added")
- [ ] Body explains *why* (if needed)
- [ ] Release link included for release-driven updates
- [ ] Co-authored-by trailer present (if applicable)

### After Pushing
- [ ] `git --no-pager log --oneline -5` confirms commit is on remote
- [ ] CI/CD checks passing (if applicable)
- [ ] PR created if working on a feature branch

## Resources

- [Conventional Commits specification](https://www.conventionalcommits.org/)
- [Git documentation](https://git-scm.com/docs)
- [GitHub flow guide](https://docs.github.com/en/get-started/using-github/github-flow)

## Related Skills

- `cli-expertise` — for using Copilot CLI's `/diff` and `/pr` commands alongside git
- `doc-maintenance` — uses this skill's commit conventions when updating documentation
