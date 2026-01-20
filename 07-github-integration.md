# GitHub Integration

GitHub Copilot CLI has deep integration with GitHub, allowing you to work with repositories, issues, pull requests, and more directly from your terminal.

## Prerequisites

To use GitHub features, you need:

1. **GitHub CLI (gh) installed and authenticated**
   - Install: `brew install gh` (macOS) or see [cli.github.com](https://cli.github.com/)
   - Authenticate: `gh auth login`
   - Verify: `gh auth status`
   - **This is required for GitHub integration to work**

2. **Authenticated GitHub account** - via `/login` or PAT

3. **Git repository** - Working directory must be a git repo

4. **Remote configured** - Usually `origin` pointing to GitHub

5. **Copilot subscription** - Active subscription with your GitHub account

## Working with Git Repositories

### Repository Detection

When you launch Copilot CLI in a Git repository, it automatically:

- Detects the repository
- Reads Git status
- Identifies remote URLs
- Tracks current branch

```
> What GitHub repository am I in?

AI: You're in the repository: username/my-project
    Remote: https://github.com/username/my-project.git
    Current branch: main
    Status: 3 modified files, 1 untracked file
```

### Git Operations

```
> Show me git status

> What changed since last commit?

> Show me the diff of staged files

> What's the commit history for the last 10 commits?
```

### Branch Management

```
> Create a new branch called feature-login

> Switch to the develop branch

> What branch am I on?

> List all branches
```

### Commits

```
> Show me the last commit

> What changed in commit abc123?

> Show me commits by author John

> Find commits that modified @src/auth.js
```

## Pull Requests

### Viewing Pull Requests

```
> Show me open pull requests for this repo

> What's in PR #42?

> Show me pull requests assigned to me

> List PRs with label "bug"
```

### PR Details

```
> Show details of PR #123

AI: Pull Request #123: "Add user authentication"
    Author: @username
    Status: Open
    Checks: ✅ All passing
    Reviews: 2 approved
    Files changed: 5
    +234 -67 lines
```

### PR Diffs

```
> Show me the diff of PR #42

> What files changed in PR #123?

> Review the changes in PR #56
```

### PR Comments

```
> Show comments on PR #42

> What did reviewers say about PR #123?
```

## Creating Pull Requests with /delegate

The `/delegate` command creates PRs automatically:

### Basic Delegation

```
> /delegate Fix typo in README

What happens:
1. ✅ Creates new branch: copilot-fix-readme-typo
2. ✅ Makes changes to README.md
3. ✅ Commits changes
4. ✅ Pushes to remote
5. ✅ Creates pull request
6. 🔗 Returns PR URL
```

### Complex Delegations

```
> /delegate Add user authentication with JWT tokens
  - Create auth middleware
  - Add login/register endpoints
  - Add password hashing
  - Update user model
  - Add tests
```

AI will:
1. Plan all changes
2. Create feature branch
3. Implement all features
4. Commit changes
5. Push and create PR

### Delegation Best Practices

✅ **Be specific** - Clear description helps AI understand  
✅ **Scope appropriately** - Not too large for single PR  
✅ **Review before merge** - Check the PR thoroughly  
✅ **Provide context** - Mention related issues or requirements  

Example:

```
> /delegate Implement issue #45
  Add pagination to user list endpoint
  Default 20 items per page
  Accept page and limit query params
  Update API docs
```

## Issues

### Viewing Issues

```
> Show me open issues

> What's issue #42 about?

> List issues assigned to me

> Show issues with label "enhancement"
```

### Issue Details

```
> Show details of issue #123

AI: Issue #123: "Add dark mode support"
    Author: @username
    Status: Open
    Labels: enhancement, good-first-issue
    Assignees: @dev1, @dev2
    Created: 3 days ago
    Comments: 5
    
    Description:
    Users have requested dark mode...
```

### Working with Issues

```
> /delegate Implement issue #42

> Create a PR to fix issue #67

> Reference issue #123 in my next commit
```

### Creating Issues

While you can't create issues directly, you can prepare the content:

```
> Draft an issue for the bug I found in @src/auth.js

AI: Here's a suggested issue:

Title: Authentication fails with expired tokens

Description:
When a user's token expires, the auth middleware
throws an unhandled exception instead of returning 401.

Steps to reproduce:
1. Log in with valid credentials
2. Wait 24 hours
3. Make API request
4. Server crashes

Expected: Return 401 Unauthorized
Actual: Server error 500

Code location: src/auth.js line 42

[Copy this to GitHub to create issue]
```

## Repository Information

### Repository Metadata

```
> Tell me about this repository

AI: Repository: username/my-project
    Description: A web application for task management
    Language: JavaScript (72%), CSS (18%), HTML (10%)
    Stars: 42
    Forks: 7
    License: MIT
    Last updated: 2 hours ago
```

### Repository Structure

```
> Show me the repository structure

> What's in the src directory?

> List all configuration files
```

### README and Documentation

```
> Show me the README

> What does the CONTRIBUTING.md say?

> Read @docs/API.md
```

## Working Across Repositories

### Multiple Repositories

You can reference other repositories:

```
> Show me issues in username/other-repo

> What's the latest release of facebook/react?

> Compare my implementation with the example in owner/example-repo
```

### Cross-Repository Code Reference

```
> How does username/similar-project handle authentication?

> Show me the implementation in owner/reference-repo @src/auth.js

> What patterns does popular-framework/core use?
```

## GitHub Actions

### Workflow Status

```
> Show me the status of CI/CD workflows

> Did the tests pass on the last commit?

> What's the status of GitHub Actions?
```

### Workflow Files

```
> @.github/workflows/ci.yml explain this workflow

> Update @.github/workflows/deploy.yml to include staging

> Create a GitHub Actions workflow for testing
```

## Releases

### Viewing Releases

```
> Show me the latest release

> What changed in version 2.0.0?

> List all releases
```

### Release Notes

```
> Show release notes for v1.5.0

> What's new in the latest version?
```

## Collaborators and Teams

### Repository Access

```
> Who has access to this repository?

> Show me the collaborators

> What permissions does @username have?
```

### Code Owners

```
> Who owns @src/api/**/*.js according to CODEOWNERS?

> Show me the CODEOWNERS file

> Who should review changes to @docs/?
```

## GitHub Search

### Search Code

```
> Search for "authentication" in this repository

> Find all usages of the Login component

> Where is JWT_SECRET used?
```

### Search Issues and PRs

```
> Find issues mentioning "performance"

> Search PRs that modified @src/auth.js

> Find closed issues with label "bug"
```

## Webhooks and Integrations

### Viewing Integrations

```
> What integrations are enabled for this repo?

> Show me webhook configurations

> What CI/CD is set up?
```

## Security

### Security Alerts

```
> Are there any security vulnerabilities?

> Show me Dependabot alerts

> Check for security issues in dependencies
```

### Dependency Updates

```
> What dependencies need updating?

> Show me outdated packages

> Are there any vulnerable dependencies?
```

## Practical GitHub Workflows

### Workflow 1: Bug Fix from Issue

```
1. > Show me issue #42
2. [Read issue details]
3. > /delegate Fix issue #42
   [AI creates branch, fixes issue, creates PR]
4. > Show me PR #123 that was just created
5. [Review PR]
6. [Merge on GitHub]
```

### Workflow 2: Feature Development

```
1. > Create a new branch called feature-search
2. > Implement search functionality:
     - Add search endpoint to API
     - Create search UI component
     - Add search tests
3. [AI implements]
4. > Run tests
5. > Create a commit with message "Add search feature"
6. > Push to origin feature-search
7. [Create PR on GitHub]
```

### Workflow 3: PR Review

```
1. > Show me PR #45
2. > Show the diff of PR #45
3. > Review @changed-file.js for issues
4. > Are there any security concerns?
5. > Do the changes follow best practices?
6. [Add review comments on GitHub]
```

### Workflow 4: Release Preparation

```
1. > What commits are new since last release?
2. > Generate release notes from commits
3. > Update @CHANGELOG.md with new version
4. > Update version in @package.json to 2.0.0
5. > Create a commit "Release v2.0.0"
6. > Push and tag as v2.0.0
```

## GitHub-Specific Commands

### Repository Commands

```
> Clone repository username/repo-name

> Initialize a new git repository

> Add remote origin <url>

> Set upstream branch
```

### Branch Commands

```
> Create and switch to new branch

> Merge branch-name into current branch

> Rebase current branch on main

> Delete branch branch-name
```

### Commit Commands

```
> Stage all changes

> Commit with message "feat: add login"

> Amend last commit

> Cherry-pick commit abc123
```

### Remote Commands

```
> Push current branch to origin

> Pull latest changes from main

> Fetch all remotes

> Push tags to remote
```

## GitHub CLI Integration

Copilot CLI uses `gh` (GitHub CLI) under the hood for GitHub features. You can also use gh commands directly:

### Direct gh Commands

```
> Use gh to create an issue

> Run gh pr list

> Use gh to clone username/repo

> Run gh release create v1.0.0

> !gh repo view owner/repo

> !gh issue create --title "Bug" --body "Description"
```

### Common gh Commands with Copilot CLI

```
# View repository
> Show me repository information
[Uses: gh repo view]

# Create issue
> Create a GitHub issue for the bug I found
[Uses: gh issue create]

# List PRs
> Show open pull requests
[Uses: gh pr list --state open]

# Create PR
> /delegate Fix the authentication bug
[Uses: gh pr create]

# View PR
> Show details of PR #42
[Uses: gh pr view 42]

# Clone repository
> Clone the repository username/project
[Uses: gh repo clone username/project]
```

### Verify gh CLI is Working

```
> !gh auth status

> !gh repo view

> !gh pr list
```

If gh commands fail, authenticate:
```bash
gh auth login
```

## Best Practices

### DO:

✅ **Work in Git repo** - Most features require Git  
✅ **Use /delegate** - For automated PRs  
✅ **Reference issues** - Link PRs to issues  
✅ **Review PRs** - Check delegated changes  
✅ **Keep branches focused** - One feature per branch  

### DON'T:

❌ **Delegate huge changes** - Keep PRs reasonable  
❌ **Skip review** - Always check AI-generated PRs  
❌ **Force push** - Unless you know what you're doing  
❌ **Ignore CI failures** - Fix them before merging  

## Troubleshooting

### Authentication Issues

```
> I can't access the repository

Solutions:
1. Check gh CLI: gh auth status
2. Authenticate gh: gh auth login
3. Check Copilot: /user show
4. Try: /logout then /login
5. Verify Copilot subscription
6. Check repo permissions
```

### GitHub CLI Not Working

```
> GitHub features not working

Solutions:
1. Verify gh installed: gh --version
2. Authenticate: gh auth login
3. Check status: gh auth status
4. Test gh directly: gh repo view
5. Refresh token: gh auth refresh
```

### Remote Issues

```
> Push failed or remote not found

Solutions:
1. Verify remote: git remote -v
2. Check branch tracking
3. Ensure proper permissions
4. Try HTTPS vs SSH URL
```

### API Rate Limits

```
> Getting rate limit errors

Solutions:
1. Wait for rate limit reset
2. Use authenticated requests
3. Check: /usage
4. Reduce API calls
```

## Examples

### Example 1: Quick Fix PR

```
> /delegate Fix typo in README line 23
  Change "recieve" to "receive"

✅ Created branch: copilot-fix-readme-typo
✅ Fixed typo in README.md
✅ Committed: "Fix typo in README"
✅ Pushed to origin
✅ Created PR #78
🔗 https://github.com/user/repo/pull/78
```

### Example 2: Feature Implementation

```
> Show me issue #45

Issue #45: Add export to CSV feature

> /delegate Implement issue #45
  Add CSV export button to data table
  Create downloadCSV function
  Include all visible columns
  Format dates properly

[AI implements everything]

✅ Created PR #79: "Add CSV export feature (fixes #45)"
```

### Example 3: Investigation

```
> Show me recent PRs that modified @src/api/auth.js

AI: Found 3 PRs:
  • PR #67: "Update auth middleware" (merged 2 days ago)
  • PR #54: "Add JWT refresh" (merged 1 week ago)  
  • PR #42: "Fix auth bug" (merged 2 weeks ago)

> Show me the diff from PR #67

[AI shows changes]

> Why was this change made?

[AI explains based on PR description and comments]
```

## Quick Reference

```bash
# Pull Requests
Show PR #<number>
Show open PRs
/delegate <description>

# Issues
Show issue #<number>
Show open issues
List issues with label "bug"

# Repository
Show repo info
What changed recently?
Show commit history

# Branches
Create branch <name>
Switch to <branch>
Show current branch

# GitHub Actions
Show workflow status
Did tests pass?
```

---

**Next:** [Advanced Features](08-advanced-features.md)  
**Previous:** [Code Editing and Development](06-code-editing.md)
