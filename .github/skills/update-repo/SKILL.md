---
name: update-repo
description: Pulls the latest changes from the git remote by running git remote update and git pull main.
---

# Update Repo Skill

## Metadata
- **ID**: update-repo
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: Git
- **Tags**: git, pull, remote, sync, update

## Description

Pulls the latest changes from the git remote into the local repository. Runs
`git remote update` to fetch all remote refs, then `git pull main` to merge
the latest `main` into the current branch.

## Capabilities

- Fetch all remote refs without merging
- Pull the latest `main` branch changes into the working tree
- Report the result of both operations

## When to Use

- Before starting work to ensure the local repo is up to date
- After a PR is merged and you want to pull those changes locally
- When another contributor has pushed to `main` and you need to sync
- As a first step before running any doc-maintenance or other update workflow

## Instructions

### Step 1 — Fetch all remote refs

```bash
git remote update
```

This fetches all branches and tags from every configured remote without
modifying the working tree. It updates the remote-tracking refs (e.g.
`origin/main`) so you can see what has changed upstream.

### Step 2 — Pull latest main

```bash
git pull main
```

This merges the latest `main` from the remote into the current branch.

> If you are already on `main`, this fast-forwards your local branch to match
> the remote. If you are on a feature branch, this merges `main` into it.

### Running both together

```bash
git remote update && git pull main
```

### Common Pitfalls

- ❌ Running `git pull main` without `git remote update` first may work but
  skips refreshing all remote-tracking refs — use both for a clean sync
- ❌ Running this while you have uncommitted local changes — stash first:
  ```bash
  git stash
  git remote update && git pull main
  git stash pop
  ```
- ❌ Pulling into a branch that has diverged from `main` without first
  reviewing the diff — check `git log HEAD..origin/main` first if unsure

### Best Practices

- ✅ Run this at the start of every working session
- ✅ Run this before triggering any workflow that depends on up-to-date docs
- ✅ If `git pull main` reports merge conflicts, resolve them before continuing
