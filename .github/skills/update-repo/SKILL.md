---
name: update-repo
description: Syncs the long-running release-updates branch with the latest remote state before automated changes.
---

# Update Repo Skill

## Metadata
- **ID**: update-repo
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: Git
- **Tags**: git, pull, remote, sync, update

## Description

Syncs the local repository with the latest remote state for this repo's
long-running `release-updates` branch. Fetches remote refs, checks out
`release-updates`, and fast-forwards it to `origin/release-updates` before any
change detection or automated documentation updates run.

## Capabilities

- Fetch all remote refs without merging
- Check out the local `release-updates` branch
- Fast-forward `release-updates` to `origin/release-updates`
- Report the result of both operations

## When to Use

- Before starting work to ensure the local repo is up to date
- After a PR is merged and you want to pull those changes locally
- When another contributor has pushed to `release-updates` and you need to sync
- As the first step before running any doc-maintenance or other update workflow

## Instructions

### Step 1 — Fetch all remote refs

```bash
git remote update
```

This fetches all branches and tags from every configured remote without
modifying the working tree. It updates the remote-tracking refs (e.g.
`origin/main`) so you can see what has changed upstream.

### Step 2 — Reset the local release-updates branch to the remote

```bash
git checkout -B release-updates origin/release-updates
```

This ensures the local long-running `release-updates` branch exactly matches the
remote branch before any automated change detection begins.

### Running both together

```bash
git remote update && git checkout -B release-updates origin/release-updates
```

### Common Pitfalls

- ❌ Skipping the refresh step may leave `release-updates` behind the remote and
  cause automation to compute changes from stale files
- ❌ Running this while you have uncommitted local changes — stash first:
  ```bash
  git stash
  git remote update && git checkout -B release-updates origin/release-updates
  git stash pop
  ```
- ❌ Treating `release-updates` like a disposable feature branch — it is a
  shared long-running branch and should stay aligned with the remote copy

### Best Practices

- ✅ Run this at the start of every working session
- ✅ Run this before triggering any workflow that depends on up-to-date docs
- ✅ Perform this sync before deciding whether any documentation changes are needed
- ✅ Create or reuse a pull request from `release-updates` into `main` after pushing new commits
