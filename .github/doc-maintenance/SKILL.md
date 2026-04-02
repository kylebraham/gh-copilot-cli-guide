# Doc Maintenance Skill

## Metadata
- **ID**: doc-maintenance
- **Version**: 1.0.0
- **Author**: GitHub Copilot CLI Guide
- **Category**: Documentation
- **Tags**: docs, maintenance, versioning, changelog

## Description

Expertise in keeping this GitHub Copilot CLI guide up to date when new CLI versions release. Knows exactly which doc files map to which CLI features and provides a systematic workflow for updating the repo after any release.

## Capabilities

- Detect when a new CLI version has been released
- Identify which documentation files need updating for a given release type
- Apply version bump conventions consistently
- Write deprecation callouts for removed or changed features
- Update CHANGELOG-history.md with meaningful entries
- Cross-reference check: find all mentions of a changed feature across the repo
- Draft new feature sections following the repo's documentation style

## When to Use

- A new GitHub Copilot CLI version has released
- A specific feature was changed, added, or removed
- The docs feel stale or a user reports inaccurate information
- Preparing a documentation PR for a release
- Auditing the guide for consistency after a batch of changes

## Instructions

### Detecting a New Release

Monitor for new releases via:

1. **GitHub Releases page:** https://github.com/github/copilot-cli/releases
2. **CLI version check:**
   ```bash
   copilot --version
   ```
3. **npm latest version:**
   ```bash
   npm view @github/copilot version
   ```
4. **CHANGELOG-history.md** — check the top entry to see what version the repo currently documents.

### Doc File Map — What to Update Per Release Type

Use this map to determine scope. Always start with ALL RELEASES, then add the feature-specific files.

#### ALL RELEASES (every version bump)
| File | What to update |
|------|----------------|
| `README.md` | Version note in footer, "Latest features" section |
| `00-cheat-sheet.md` | New commands or shortcuts |
| `16-new-features.md` | Add a new section for this version's features |

#### New COMMANDS added
| File | What to update |
|------|----------------|
| `04-slash-commands.md` | Add full command entry with description and examples |
| `00-cheat-sheet.md` | Add to the quick reference table |

#### New or changed MODELS
| File | What to update |
|------|----------------|
| `22-models-and-costs.md` | Model comparison table, pricing if known |
| `README.md` | Latest features list |

#### AUTH changes
| File | What to update |
|------|----------------|
| `01-getting-started.md` | Auth section (login steps, token scopes) |
| `11-troubleshooting.md` | Auth issues section |

#### CONFIG / directory structure changes
| File | What to update |
|------|----------------|
| `15-copilot-directory.md` | Directory structure diagram and file descriptions |
| `13-agents-file.md` | If AGENTS.md format changed |

#### SKILLS system changes
| File | What to update |
|------|----------------|
| `14-skills-system.md` | Skill format, commands, directory locations |

#### PLAN MODE changes
| File | What to update |
|------|----------------|
| `09-plan-mode.md` | Behavior changes, new options |

#### AUTOPILOT changes
| File | What to update |
|------|----------------|
| `17-autopilot-mode.md` | New capabilities, changed behavior |

#### FLEET / parallel agents changes
| File | What to update |
|------|----------------|
| `18-fleet-mode.md` | Updated workflows, new options |

#### MCP server changes
| File | What to update |
|------|----------------|
| `08-advanced-features.md` | MCP config format, new built-in servers |

#### CI/CD integration changes
| File | What to update |
|------|----------------|
| `20-cicd-automation.md` | Updated pipeline examples |

#### Team / org changes
| File | What to update |
|------|----------------|
| `21-team-setup.md` | New org features, policy settings |

### Version Bump Convention

The README.md footer contains a version note in this format:

```markdown
> **Note:** This guide covers GitHub Copilot CLI vX.X.X
```

Update the version number from old to new. Search for the pattern:

```bash
grep -n "This guide covers" README.md
```

Then update that line.

### Writing New Feature Sections

When adding to `16-new-features.md`, follow this structure:

```markdown
## What's New in vX.X.X

Released: YYYY-MM-DD

### New Feature Name

Brief description of the feature.

**How to use:**
```
> example usage
```

**Why it matters:** One sentence on the value.
```

### Deprecation Callouts

When a feature is removed or significantly changed, add a callout to the relevant section rather than deleting the content immediately:

```markdown
> ⚠️ **Deprecated in vX.X.X:** This feature was removed. Use [new feature](link) instead.
```

Keep the original content for at least one major version cycle to help users who haven't upgraded yet.

### CHANGELOG-history.md Convention

Always add an entry at the top of `CHANGELOG-history.md` for significant doc updates:

```markdown
## YYYY-MM-DD — Docs updated for vX.X.X

- Updated `16-new-features.md` with new `/example` command
- Added model entry in `22-models-and-costs.md`
- Bumped version note in `README.md`
```

### Cross-Reference Check

After updating any file, grep for mentions of the changed feature across all docs to find missed references:

```bash
# Find all references to a command or feature
grep -rn "feature-name\|/command" *.md

# Find all version references
grep -rn "v0\.0\.[0-9]*" *.md

# Find files mentioning a specific file
grep -rn "filename\.md" *.md
```

Update any stale references found.

### Documentation Style Reminders

- **Examples are executable** — all code blocks must be copy-paste ready
- **Progressive disclosure** — start simple, add complexity
- **Visual markers** — use ❌ for anti-patterns, ✅ for best practices
- **Terminology** — use "Copilot CLI" (not "GitHub Copilot CLI" after first mention), "slash commands" (lowercase), "Plan mode" (not "planning mode")
- **Command prompts** — use `>` for CLI prompts, `$` for shell prompts

### Common Pitfalls

- ❌ Updating only `16-new-features.md` without also checking `00-cheat-sheet.md` — new commands must appear in both
- ❌ Forgetting to update the version note in `README.md` footer
- ❌ Deleting deprecated content immediately — always use the deprecation callout pattern first
- ❌ Missing cross-references — a new command might be mentioned in the troubleshooting guide, examples file, or cheat sheet

### Best Practices

- ✅ Use the doc file map above for every release — don't rely on memory
- ✅ Always grep for cross-references after updating any file
- ✅ Write CHANGELOG entries as you go, not at the end
- ✅ Keep examples consistent across files (same command, same output format)
- ✅ Test that all relative links still work after adding or renaming sections

## Checklists

### Release Monitoring Checklist
- [ ] Check https://github.com/github/copilot-cli/releases for new releases
- [ ] Compare release version to `CHANGELOG-history.md` top entry
- [ ] Read the full release notes to identify what changed

### Doc Update Checklist
- [ ] Update `README.md` version note in footer
- [ ] Update `00-cheat-sheet.md` for any new commands/shortcuts
- [ ] Update `16-new-features.md` with a new version section
- [ ] Update all feature-specific files per the doc file map above
- [ ] Add deprecation callouts for removed/changed features
- [ ] Cross-reference grep across all `.md` files for changed feature names
- [ ] Update CHANGELOG-history.md

### PR / Commit Checklist
- [ ] All modified files listed in the commit description
- [ ] Version number consistent across all files
- [ ] No broken relative links
- [ ] Examples in code blocks are syntactically correct

## Resources

- [GitHub Copilot CLI Releases](https://github.com/github/copilot-cli/releases)
- [New Features Guide](../16-new-features.md)
- [CHANGELOG History](../CHANGELOG-history.md)
- [Repository README](../README.md)

## Related Skills

- `update-cli` — use to upgrade the CLI before auditing what changed
- `cli-expertise` — deep feature knowledge for understanding and documenting new capabilities
