# GitHub Copilot CLI Guide

## Project Overview

This is a comprehensive documentation repository for GitHub Copilot CLI. It consists of 15 interconnected markdown files covering installation, features, concepts, and best practices for using Copilot CLI.

## Repository Structure

```
.
├── 01-getting-started.md         # Installation and authentication
├── 02-basic-concepts.md          # Core concepts and components
├── 03-interactive-features.md    # Interface and interaction patterns
├── 04-slash-commands.md          # Command reference
├── 05-file-context.md            # File and context management
├── 06-code-editing.md            # Code editing workflows
├── 07-github-integration.md      # GitHub features and integration
├── 08-advanced-features.md       # MCP servers, skills, custom agents
├── 09-plan-mode.md               # Implementation planning
├── 10-best-practices.md          # Tips and effective usage patterns
├── 11-troubleshooting.md         # Common issues and solutions
├── 12-examples.md                # Real-world examples and tutorials
├── 13-agents-file.md             # AGENTS.md configuration guide
├── 14-skills-system.md           # Skills system documentation
├── 15-copilot-directory.md       # ~/.copilot/ directory structure
├── 16-v0.405-new-features.md     # New in v0.405: /init and /delegate
└── README.md                     # Entry point and guide structure
```

## Documentation Architecture

### Layered Information Structure

The guide is organized in a progressive learning path:

1. **Foundation** (01-02): Installation and basic concepts
2. **Core Features** (03-07): Interactive use, commands, files, editing, GitHub
3. **Advanced Topics** (08-09): Extensions, MCP servers, plan mode
4. **Reference** (10-12): Best practices, troubleshooting, examples
5. **Configuration** (13-15): AGENTS.md, skills, .copilot directory
6. **What's New** (16): v0.405 features including `/init` and enhanced `/delegate`

### Cross-References

Files heavily reference each other using relative links. When editing:
- Maintain consistent link formatting: `[Link Text](filename.md)`
- Update cross-references if section headings change
- Check that examples referenced in multiple files stay consistent

## Key Conventions

### Documentation Style

- **Examples are executable**: All code examples should be copy-paste ready
- **Progressive disclosure**: Start simple, add complexity gradually
- **Visual markers**: Use ❌/✅ for bad/good examples consistently
- **Command blocks**: Use triple backticks with language hints (bash, markdown, etc.)

### Section Structure Pattern

Most files follow this structure:
```markdown
# Title
Brief introduction (1-2 paragraphs)

## Table of Contents (if >5 sections)

## Main sections with examples

## Quick Reference (optional summary)

---
**Next:** [Next File](next-file.md)
**Previous:** [Previous File](previous-file.md)
```

### Terminology Consistency

- **Copilot CLI** (not "GitHub Copilot CLI" after first mention)
- **slash commands** (lowercase, e.g., `/help`, `/model`)
- **@ syntax** (for file references, e.g., `@filename.js`)
- **MCP servers** (Model Context Protocol servers)
- **Skills** (uppercase S when referring to the system)
- **Plan mode** (not "planning mode")

## Editing Guidelines

### When Adding New Features

1. Determine the appropriate file based on content type
2. Add entry to README.md table of contents if it's a new major section
3. Update cross-references in related files
4. Add to Quick Reference sections where applicable
5. Include examples that match the documentation style

### When Updating Existing Content

- Preserve the progressive learning flow
- Update version numbers if feature-specific
- Check for references to the changed content in other files
- Maintain consistency with example formats

### Code Examples Format

**Command line interactions:**
```
> user input here
AI response here
```

**Bash commands:**
```bash
copilot --version
gh --version
```

**File contents:**
```javascript
// Actual code examples
function example() {
  return true;
}
```

## Common Patterns

### Feature Documentation Template

```markdown
## Feature Name

Brief description (what it is)

### Why Use This

1-2 sentences explaining the value

### Basic Usage

Simple example

### Advanced Usage

More complex example

### Common Patterns

Typical use cases

### Tips

Dos and don'ts
```

### Troubleshooting Entry Format

```markdown
### Issue Description

**Symptoms:** What the user sees
**Cause:** Why it happens
**Solution:** Step-by-step fix
```

## Version Information

This guide is based on Copilot CLI version 0.0.405. When updating:
- Note version-specific features in the relevant sections
- Update the version note in README.md footer
- Mark deprecated features clearly

## Prerequisites for Contributors

- Understanding of Copilot CLI features and workflows
- Markdown proficiency
- Experience with terminal-based tools
- Familiarity with AI assistant concepts
