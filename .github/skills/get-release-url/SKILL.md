---
name: get-release-url
description: Resolves the canonical GitHub release URL for a GitHub Copilot CLI version tag.
allowed-tools: bash
---

# Get Release URL Skill

## Metadata
- **ID**: get-release-url
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: Documentation
- **Tags**: releases, github, urls, copilot-cli, versioning

## Description

Resolves the canonical GitHub release URL for a specific `github/copilot-cli`
version so other skills can reference the exact upstream release notes.

## Capabilities

- Build the canonical release URL from a known tag
- Normalize version input to the expected `vX.Y.Z` format
- Verify the latest release tag when the target version is not provided
- Return a copy-paste-ready `Release:` line for commit messages

## When to Use

- Another skill needs the upstream release link for a release-driven change
- A commit message should link directly to GitHub Copilot CLI release notes
- You know the version number but want the exact GitHub release URL format

## Instructions

### Canonical URL format

Use this format for GitHub Copilot CLI releases:

```text
https://github.com/github/copilot-cli/releases/tag/vX.Y.Z
```

### Step 1 — Normalize the version

- If the version is provided as `1.0.18`, convert it to `v1.0.18`
- If it is already `v1.0.18`, keep it as-is

Example:

```bash
VERSION="1.0.18"
TAG="v${VERSION#v}"
echo "$TAG"
```

### Step 2 — Build the URL

Once you have the normalized tag, produce:

```text
Release: https://github.com/github/copilot-cli/releases/tag/v1.0.18
```

### Step 3 — Verify when needed

If the version is unknown and you need the latest release, use:

```bash
TAG=$(curl -fL https://api.github.com/repos/github/copilot-cli/releases/latest | jq -r '.tag_name') || {
  echo "Failed to resolve the latest github/copilot-cli release tag" >&2
  exit 1
}
```

Then build the URL using `$TAG`.

If the command fails or returns an empty tag, stop and report that the release
tag could not be determined instead of guessing.

### Best Practices

- ✅ Return the full URL, not just the tag name
- ✅ Keep the `Release:` prefix when the result is intended for a commit body
- ✅ Use the exact upstream tag so reviewers can jump straight to the release notes

## Related Skills

- `git-commit` — uses this skill when preparing release-driven commit messages
- `doc-maintenance` — can use this skill when updating docs for a new CLI release
