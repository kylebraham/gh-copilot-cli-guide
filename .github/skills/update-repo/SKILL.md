---
name: update-repo
description: Refreshes all remote refs and pulls the latest changes for the branch you want to work on.
---

# Update Repo Skill

## Metadata
- **ID**: update-repo
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: Git
- **Tags**: git, pull, remote, sync, update

## Description

Refreshes the local repository with the latest remote state across all tracked
branches and tags. Start with `git remote update` so every remote-tracking ref
is current, including `origin/main`, `origin/release-updates`, and any other
active branches. Then pull or fast-forward the specific branch you plan to work
on.

## Capabilities

- Fetch all remote refs without merging
- Refresh remote-tracking refs for `main`, `release-updates`, and other branches
- Pull or fast-forward the branch you want to work on
- Report the result of the refresh and branch sync steps

## When to Use

- Before starting work to ensure the local repo is up to date
- After a PR is merged and you want to pull those changes locally
- When another contributor has pushed to any shared branch and you need to sync
- As the first step before running any doc-maintenance or other update workflow

## Instructions

### Step 1 — Fetch all remote refs

```bash
git remote update
```

This fetches all branches and tags from every configured remote without
modifying the working tree. It updates the remote-tracking refs (e.g.
`origin/main`, `origin/release-updates`) so you can see what has changed
upstream everywhere in the repository.

### Step 2 — Pull the branch you plan to work on

```bash
git pull --ff-only
```

Run this from the branch you want to update locally. It fast-forwards your
current branch to match its upstream when possible.

If you specifically need the long-running `release-updates` branch for the
automation workflow, switch to it after the remote refresh and then fast-forward
it:

```bash
git checkout release-updates
git pull --ff-only origin release-updates
```

### Running both together

```bash
git remote update && git pull --ff-only
```

### Common Pitfalls

- ❌ Skipping the refresh step may leave remote-tracking refs stale and hide
  updates on `main`, `release-updates`, or other active branches
- ❌ Running this while you have uncommitted local changes — stash first:
  ```bash
  git stash
  git remote update && git pull --ff-only
  git stash pop
  ```
- ❌ Assuming `git remote update` changes your working tree — it only refreshes
  remote-tracking refs; you still need to pull the branch you want locally

### Best Practices

- ✅ Run this at the start of every working session
- ✅ Run this before triggering any workflow that depends on up-to-date docs
- ✅ Perform this refresh before deciding whether any documentation changes are needed
- ✅ For doc automation, refresh all refs first and then fast-forward `release-updates`
- ✅ Use `--ff-only` to avoid accidental merge commits during routine syncs
