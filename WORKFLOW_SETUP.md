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

The `mvkaran/setup-copilot-cli@v1` action requires a token to install and
authenticate the Copilot CLI.

**Important:** `COPILOT_TOKEN` is used *only* to authenticate the Copilot CLI
binary. All repository operations — checkout, branch push, and PR creation —
are performed by the built-in `GITHUB_TOKEN`, which is already scoped to this
repository. The `COPILOT_TOKEN` does **not** need any repository permissions.

### Recommended: Fine-grained Personal Access Token

A fine-grained PAT is the least-privilege option. It grants only Copilot API
access and nothing else.

#### 2a — Generate a fine-grained PAT

1. Go to **GitHub** → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens**
2. Click **Generate new token**
3. Give it a descriptive name, e.g. `copilot-cli-doc-maintenance`
4. Set an expiration (recommended: 90 days — set a calendar reminder to rotate)
5. Under **Resource owner**, select your personal account (not a specific repo — Copilot is an account-level resource)
6. Under **Repository access**, select **No repository access** — none is needed
7. Under **Account permissions**, find **GitHub Copilot** and set it to **Read-only**
8. Click **Generate token** and **copy it immediately** — you won't see it again

> **Note:** The **GitHub Copilot** account permission only appears if your account has an active Copilot subscription (Individual, Business, or Enterprise).

> **Compatibility note:** Fine-grained PATs work with `gh auth login` (which `mvkaran/setup-copilot-cli@v1` uses internally). If you encounter an authentication error specifically mentioning fine-grained token restrictions, fall back to the classic PAT option below.

#### 2b — Add the token as a repository secret

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Set:
   - **Name**: `COPILOT_TOKEN`
   - **Secret**: paste your token
5. Click **Add secret**

---

### Fallback: Classic Personal Access Token (if fine-grained doesn't work)

If `mvkaran/setup-copilot-cli@v1` rejects the fine-grained token, generate a
classic PAT with the minimum scopes needed:

1. Go to **GitHub** → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. Click **Generate new token (classic)**
3. Name it `copilot-cli-doc-maintenance` and set an expiration
4. Select only these scopes:

   | Scope | Why |
   |---|---|
   | `read:user` | Required for Copilot CLI authentication |
   | `copilot` | Grants access to the GitHub Copilot API |

   > Do **not** add `repo` — all repository operations use `GITHUB_TOKEN`, not this token.

5. Click **Generate token**, copy it, and add it as `COPILOT_TOKEN` following step 2b above

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

1. Generate a new fine-grained PAT following Step 2a above (or classic if that's what you used)
2. Go to **Settings** → **Secrets and variables** → **Actions**
3. Click the **pencil icon** next to `COPILOT_TOKEN`
4. Paste the new token and click **Update secret**

> **Tip:** Set a calendar reminder a week before the expiry date so the workflow doesn't silently fail on a Saturday morning.

---

## Summary

| What | Where |
|---|---|
| `GITHUB_TOKEN` write permissions | Settings → Actions → General → Workflow permissions |
| Allow Actions to create PRs | Settings → Actions → General → Workflow permissions |
| `COPILOT_TOKEN` secret | Settings → Secrets and variables → Actions |
| Manual test trigger | Actions → Copilot CLI Release Doc Maintenance → Run workflow |
