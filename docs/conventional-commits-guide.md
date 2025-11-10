# Conventional Commits Guide

## Overview

Conventional Commits is a specification for adding human and machine-readable meaning to commit messages. It provides an easy set of rules for creating an explicit commit history, which makes it easier to write automated tools on top of.

**Official Specification**: https://www.conventionalcommits.org/

## Format

The commit message should be structured as follows:

```
<type>(<scope>): <subject>

[optional body]

[optional footer(s)]
```

### Short Format (BMAD Standard)

For most commits, use the short format (no body or footer):

```
<type>(<scope>): <subject>
```

**Example**: `feat(bmm): add suggest-commit workflow`

## Commit Types

### Primary Types

| Type       | Description                                             | When to Use                                     |
| ---------- | ------------------------------------------------------- | ----------------------------------------------- |
| `feat`     | A new feature                                           | Adding new functionality                        |
| `fix`      | A bug fix                                               | Fixing a defect or error                        |
| `docs`     | Documentation only changes                              | README, guides, comments                        |
| `test`     | Adding missing tests or correcting existing tests       | Unit, integration, e2e tests                    |
| `refactor` | Code change that neither fixes a bug nor adds a feature | Restructuring without changing behavior         |
| `chore`    | Changes to build process or auxiliary tools             | Dependencies, scripts, configuration            |
| `perf`     | Code change that improves performance                   | Optimization, caching, algorithmic improvements |
| `ci`       | Changes to CI configuration files and scripts           | GitHub Actions, CircleCI, Jenkins               |
| `build`    | Changes that affect the build system or external deps   | npm, webpack, rollup, dependencies              |
| `style`    | Changes that do not affect meaning (formatting, etc.)   | Whitespace, semicolons, formatting              |

### Type Selection Guide

**Q: New functionality?**
→ Use `feat`

**Q: Fixing broken code?**
→ Use `fix`

**Q: Only documentation?**
→ Use `docs`

**Q: Only tests?**
→ Use `test`

**Q: Improving performance?**
→ Use `perf`

**Q: Restructuring code without changing behavior?**
→ Use `refactor`

**Q: Build, dependencies, or tooling?**
→ Use `chore` or `build`

**Q: CI/CD pipelines?**
→ Use `ci`

**Q: Formatting or style?**
→ Use `style`

## Scope

The scope provides additional contextual information about where the change occurred.

### BMAD Scopes

| Scope      | Description                        | Example Files                                  |
| ---------- | ---------------------------------- | ---------------------------------------------- |
| `core`     | Core BMAD functionality            | `src/core/`                                    |
| `bmm`      | BMad Method module                 | `src/modules/bmm/`                             |
| `bmd`      | BMAD Development module            | `src/modules/bmd/`                             |
| `bmb`      | BMad Builder module                | `src/modules/bmb/`                             |
| `cis`      | Creative Intelligence Suite module | `src/modules/cis/`                             |
| `bmgd`     | Game Development module            | `src/modules/bmgd/`                            |
| `docs`     | Documentation                      | `docs/`                                        |
| `tools`    | CLI and build tools                | `tools/`                                       |
| `test`     | Testing infrastructure             | `test/`, `tests/`                              |
| `ci`       | Continuous Integration             | `.github/workflows/`                           |
| `deps`     | Dependencies                       | `package.json`, `package-lock.json`            |
| `config`   | Configuration files                | `.eslintrc.js`, `.prettierrc`, `tsconfig.json` |
| `multiple` | Changes spanning multiple scopes   | Mixed changes across modules                   |

### Scope Guidelines

- Use **specific module names** when changes are isolated to one module
- Use **broader categories** (core, docs, tools) for cross-cutting changes
- Use **multiple** when changes span several unrelated areas
- **Omit scope** if the change is truly global and doesn't fit any category

## Subject

The subject contains a succinct description of the change.

### Rules

1. **Max 50 characters** (recommended)
2. **No period** at the end
3. **Imperative mood** - "add" not "added", "fix" not "fixed"
4. **Lowercase** first letter (unless proper noun)
5. **Describe what changes** - not why or how

### Good Examples

✅ `feat(bmm): add suggest-commit workflow`
✅ `fix(core): resolve workflow time tracking bug`
✅ `docs(guides): add Conventional Commits guide`
✅ `test(bmm): add commit workflow tests`
✅ `refactor(tools): simplify installer logic`
✅ `chore(deps): update eslint to v8.5.0`
✅ `perf(core): optimize workflow loading`

### Bad Examples

❌ `feat(bmm): Added suggest commit workflow.` (past tense, period, capitalized)
❌ `fix: bug fix` (too vague, no scope)
❌ `update` (not a valid type)
❌ `feat(bmm): implement a new workflow for suggesting commit messages based on git status analysis` (too long)
❌ `fixed the bug in workflow engine` (no type, no scope, past tense)

## Breaking Changes

If a commit introduces a breaking change (incompatible API change), include `BREAKING CHANGE:` in the commit footer or add `!` after the type/scope.

**With exclamation mark:**

```
feat(api)!: remove deprecated endpoint
```

**With footer:**

```
feat(api): update authentication flow

BREAKING CHANGE: Authentication now requires OAuth2. API key auth is removed.
```

## Body (Optional)

The body should provide more detailed explanation of:

- **Why** the change was made
- **What** problem it solves
- **How** it differs from previous behavior

Use the body for complex changes that need explanation. Most BMAD commits should be simple enough to not require a body.

**Example with body:**

```
refactor(core): optimize workflow caching

Previous implementation cached entire workflow objects in memory,
leading to high memory usage for large workflows. New implementation
caches only workflow metadata and loads full content on demand.

This reduces average memory usage by 60% in projects with 50+ workflows.
```

## Footer (Optional)

The footer contains references to issues, breaking changes, or other metadata.

**Issue references:**

```
fix(bmm): resolve sprint status update bug

Closes #123
Refs #456, #789
```

**Breaking changes:**

```
BREAKING CHANGE: API endpoint structure changed
```

## Complete Examples

### Feature Addition

```
feat(bmm): add suggest-commit workflow

Implements automated commit message generation using Conventional
Commits specification. Analyzes git status and suggests appropriate
type, scope, and subject based on changed files.

Includes time tracking integration and persists workflow logs.
```

### Bug Fix

```
fix(core): resolve workflow time tracking calculation

Fixed duration calculation that was showing negative values when
workflows spanned midnight. Now correctly handles date boundaries
and timezone offsets.

Closes #234
```

### Documentation

```
docs(guides): add Conventional Commits guide

Comprehensive guide covering commit types, scope selection, subject
formatting rules, and BMAD-specific conventions.
```

### Refactoring

```
refactor(tools): simplify installer path resolution

Replaced complex path resolution logic with consistent pattern
using path.resolve(). No functional changes, improved readability.
```

### Chore

```
chore(deps): update development dependencies

- eslint: 8.4.0 → 8.5.0
- prettier: 2.8.0 → 2.8.1
- jest: 29.3.0 → 29.3.1
```

### Performance

```
perf(core): cache workflow template parsing

Added LRU cache for parsed workflow templates. Reduces repeated
parsing overhead by 70% in workflows with many sub-workflows.
```

## BMAD Workflow Integration

BMAD provides the `suggest-commit` workflow to automate commit message generation:

**Standalone usage:**

```bash
@bmad/bmm/agents/dev
*suggest-commit
```

**Automatic integration:**

The suggest-commit workflow is automatically offered after completing workflows that modify files (dev-story, story-done, etc.).

## Benefits

### For Developers

- Consistent commit history across team
- Clear understanding of what each commit does
- Easy to find specific changes in git log
- Automated changelog generation possible

### For Tools

- Automated version bumping (semver)
- Automated changelog generation
- Better git bisect operations
- Improved code review processes

### For Project Management

- Track features vs fixes vs chores
- Measure development velocity
- Identify breaking changes quickly
- Generate release notes automatically

## Quick Reference Cheatsheet

```
Common Patterns:

feat(scope): add new feature
fix(scope): resolve bug in feature
docs(scope): update documentation
test(scope): add missing tests
refactor(scope): restructure code
chore(scope): update dependencies
perf(scope): improve performance
ci(scope): update CI configuration
build(scope): modify build system
style(scope): format code

Scopes: core, bmm, bmd, bmb, cis, bmgd, docs, tools, test, ci, deps, config, multiple

Subject Rules:
- Max 50 chars
- Imperative mood
- Lowercase start
- No period
```

## Resources

- **Official Specification**: https://www.conventionalcommits.org/
- **Commit Template**: `src/utility/templates/COMMIT_MESSAGE_TEMPLATE.md`
- **BMAD Workflow**: `src/modules/bmm/workflows/git/suggest-commit/`
- **Angular Convention**: https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit

## See Also

- [Version Strategy](./version-strategy.md) - One Epic = One Minor Version
- [Release Notes Template](../src/utility/templates/RELEASE_NOTES_TEMPLATE.md) - Semantic versioning format
- [FDD/SDD Guide](./fdd-sdd-guide.md) - Feature-Driven and Spec-Driven Development

---

**Last Updated**: 2025-11-09

**Status**: Production Ready

**Feedback**: Submit issues or suggestions to the BMAD development team
