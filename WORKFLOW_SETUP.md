# Workflow Setup Guide

This guide covers everything you need to configure in GitHub to make the
[`copilot-cli-release-doc-maintenance`](.github/workflows/copilot-cli-release-doc-maintenance.yml)
workflow run successfully.

---

## Overview of secrets used

| Secret | Source | Purpose |
|---|---|---|
| `GITHUB_TOKEN` | Auto-provided by GitHub Actions | Polls the `github/copilot-cli` releases API; creates/updates the pull request |
| `COPILOT_TOKEN` | You must create this | Authenticates the `mvkaran/setup-copilot-cli@v1` action and the Copilot CLI itself |

---

## Step 1 — Enable workflow write permissions

The workflow needs to write to branches and create pull requests using the
built-in `GITHUB_TOKEN`. This requires write permissions to be enabled.

1. Go to your repository on GitHub
2. Click **Settings** → **Actions** → **General**
3. Scroll to **Workflow permissions**
4. Select **Read and write permissions**
5. Check **Allow GitHub Actions to create and approve pull requests**
6. Click **Save**

> Without this, `GITHUB_TOKEN` will be read-only and the PR creation step will fail.

---

## Step 2 — Create a `COPILOT_TOKEN` secret

The `mvkaran/setup-copilot-cli@v1` action requires a GitHub token that has
access to GitHub Copilot to install and authenticate the Copilot CLI.

### 2a — Generate a Personal Access Token (Classic)

1. Go to **GitHub** → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. Click **Generate new token (classic)**
3. Give it a descriptive name, e.g. `copilot-cli-doc-maintenance`
4. Set an expiration (recommended: 90 days or 1 year — remember to rotate it)
5. Select these scopes:

   | Scope | Why |
   |---|---|
   | `repo` | Full repository access (read/write) — needed for checkout and push |
   | `read:user` | Required for Copilot CLI authentication |
   | `copilot` | Grants access to GitHub Copilot API |

6. Click **Generate token** and **copy it immediately** — you won't see it again

> **Note:** The `copilot` scope may appear as **"GitHub Copilot"** or **"Access GitHub Copilot"** depending on your GitHub plan. If you don't see it, ensure your account or organization has an active Copilot subscription.

### 2b — Add the token as a repository secret

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Set:
   - **Name**: `COPILOT_TOKEN`
   - **Secret**: paste your token
5. Click **Add secret**

---

## Step 3 — Verify the branch protection rules (if applicable)

If your `main` branch has branch protection rules enabled, you need to ensure
the Actions bot is allowed to push to the automation branches and open PRs.

- The workflow creates branches named `docs/copilot-cli-release-<tag>` (e.g. `docs/copilot-cli-release-v1.0.16`)
- These are **not** `main` — protection rules on `main` do **not** block the workflow from pushing them
- The workflow opens a PR from that branch → `main`, which goes through your normal review process

**No special bypass is needed** unless you have rules that restrict who can push *any* branch to the repository.

---

## Step 4 — Test the workflow manually

Before waiting for the next scheduled 09:00 UTC run, trigger it manually to
confirm the setup is correct.

1. Go to your repository on GitHub
2. Click **Actions** → **Copilot CLI Release Doc Maintenance**
3. Click **Run workflow** → **Run workflow**

### Expected outcomes

| Condition | Outcome |
|---|---|
| Auth/token issue | Workflow fails at **Setup GitHub Copilot CLI** step |
| `github/copilot-cli` has no new release (tag matches `.github/last-processed-release`) | Workflow exits at **Check whether release is new** — no PR created ✅ |
| New release detected, but doc-maintenance produces no file changes | Exits at **Exit cleanly** step — no PR created ✅ |
| New release detected and changes are produced | PR created at `docs/copilot-cli-release-<tag>` ✅ |

---

## Step 5 — Simulating a new release (optional, for testing)

To test the full end-to-end flow without waiting for an actual new release:

1. Edit `.github/last-processed-release` in the repository and set it to a fake old tag, e.g. `v0.0.1`
2. Commit and push that change to `main`
3. Trigger the workflow manually via **Run workflow**
4. The workflow will detect `v1.0.15` (current latest) as "new" and run the full flow
5. After testing, reset `.github/last-processed-release` back to `v1.0.15`

---

## Rotating the `COPILOT_TOKEN`

When your token expires:

1. Generate a new PAT following Step 2a above
2. Go to **Settings** → **Secrets and variables** → **Actions**
3. Click the **pencil icon** next to `COPILOT_TOKEN`
4. Paste the new token and click **Update secret**

> **Tip:** Set a calendar reminder a week before the expiry date.

---

## Summary

| What | Where |
|---|---|
| `GITHUB_TOKEN` write permissions | Settings → Actions → General → Workflow permissions |
| Allow Actions to create PRs | Settings → Actions → General → Workflow permissions |
| `COPILOT_TOKEN` secret | Settings → Secrets and variables → Actions |
| Manual test trigger | Actions → Copilot CLI Release Doc Maintenance → Run workflow |
