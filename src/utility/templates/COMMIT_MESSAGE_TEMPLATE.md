# Commit Message Template

This template follows the [Conventional Commits](https://www.conventionalcommits.org/) specification.

---

## Format

```
type(scope): subject

[optional body]

[optional footer]
```

### Components

- **type**: The type of change (required)
- **scope**: The area of code affected (optional but recommended)
- **subject**: Brief description of the change (required, max 50 characters)
- **body**: Detailed explanation (optional)
- **footer**: Breaking changes, issue references (optional)

---

## Commit Types

### `feat` - New Feature

Add new functionality or capabilities.

**Examples**:

```
feat(auth): add OAuth 2.0 login support
feat(api): implement user profile endpoint
feat(ui): add dark mode toggle
```

### `fix` - Bug Fix

Fix a bug or issue.

**Examples**:

```
fix(validation): correct email regex pattern
fix(api): resolve null pointer in user lookup
fix(ui): fix button alignment on mobile
```

### `docs` - Documentation

Add or update documentation (no code changes).

**Examples**:

```
docs(readme): update installation instructions
docs(api): add endpoint examples to swagger
docs: fix typos in contributing guide
```

### `test` - Tests

Add, update, or fix tests (no production code changes).

**Examples**:

```
test(auth): add unit tests for login flow
test(api): increase coverage for user service
test: fix flaky e2e test for checkout
```

### `refactor` - Refactoring

Restructure code without changing functionality.

**Examples**:

```
refactor(auth): extract validation to separate module
refactor(api): simplify error handling logic
refactor: rename getUserData to fetchUserProfile
```

### `chore` - Maintenance

Updates to build process, dependencies, or tooling.

**Examples**:

```
chore(deps): update react to v18.2.0
chore: upgrade node version to 20.x
chore(ci): add code coverage reporting
```

### `perf` - Performance

Improve performance without changing functionality.

**Examples**:

```
perf(api): add database query indexing
perf(ui): lazy load images below fold
perf: reduce bundle size by 15%
```

### `ci` - Continuous Integration

Changes to CI/CD configuration or scripts.

**Examples**:

```
ci: add automated deployment to staging
ci(github): update workflow to use Node 20
ci: configure dependabot auto-merge
```

### `build` - Build System

Changes to build configuration or dependencies.

**Examples**:

```
build: update webpack config for tree shaking
build(npm): add production build script
build: configure typescript strict mode
```

### `style` - Code Style

Code formatting changes (whitespace, semicolons, etc).

**Examples**:

```
style: format code with prettier
style(css): organize imports alphabetically
style: fix eslint warnings
```

---

## Scopes

Common scopes for organizing commits by area:

**Frontend**:

- `ui` - User interface components
- `auth` - Authentication/authorization
- `routing` - Navigation and routing
- `forms` - Form components and validation

**Backend**:

- `api` - API endpoints
- `db` - Database changes
- `auth` - Authentication/authorization
- `middleware` - Express/framework middleware

**Infrastructure**:

- `ci` - Continuous integration
- `docker` - Docker configuration
- `deploy` - Deployment scripts
- `deps` - Dependencies

**Documentation**:

- `readme` - README file
- `docs` - Documentation folder
- `api-docs` - API documentation
- `comments` - Code comments

---

## Subject Line Rules

1. **Max 50 characters**: Keep it concise
2. **Imperative mood**: Use "add" not "added" or "adds"
3. **No period**: Don't end with a period
4. **Lowercase**: Start with lowercase letter (except proper nouns)
5. **Be specific**: Describe what changed, not how

### Good Examples

```
feat(api): add user profile endpoint
fix(auth): resolve token expiration bug
docs: update installation guide
```

### Bad Examples

```
feat(api): Added user profile endpoint.  // ❌ Wrong tense, ends with period
Fix: Fixed a bug in the auth module      // ❌ Too vague, wrong capitalization
Update code                              // ❌ No type, too vague
```

---

## Optional Body

Use the body to explain **what** and **why**, not **how**.

```
feat(api): add rate limiting to public endpoints

Implement rate limiting to prevent abuse of public APIs.
Uses redis for distributed rate limiting across instances.
Default limit: 100 requests per minute per IP.
```

---

## Optional Footer

### Breaking Changes

Prefix with `BREAKING CHANGE:` to indicate breaking changes.

```
feat(api): redesign authentication endpoints

BREAKING CHANGE: Auth endpoints moved from /auth to /api/v2/auth
Migration guide: docs/migration-v2.md
```

### Issue References

Reference GitHub issues or tickets.

```
fix(ui): correct button alignment on mobile

Fixes #123
Closes #124, #125
Related to #126
```

---

## Complete Examples

### Feature with Body

```
feat(payment): integrate Stripe payment processing

Add Stripe checkout flow for subscription purchases.
Supports credit cards and Apple Pay.
Includes webhook handling for payment events.

Closes #234
```

### Bug Fix with Details

```
fix(auth): resolve session timeout during file upload

Large file uploads were timing out the session due to
default 30-minute timeout. Increased timeout to 2 hours
and added activity tracking to prevent premature logout.

Fixes #456
```

### Breaking Change

```
feat(api): migrate to GraphQL

Replace REST API with GraphQL endpoint for better performance
and flexibility. All existing REST endpoints are deprecated.

BREAKING CHANGE: REST endpoints will be removed in v3.0.0
Migration guide: docs/rest-to-graphql-migration.md

Closes #789
```

---

## Quick Reference

```
feat: New feature
fix: Bug fix
docs: Documentation
test: Tests
refactor: Code restructure
chore: Maintenance
perf: Performance
ci: CI/CD changes
build: Build system
style: Code formatting
```

---

## Tools & Automation

### Commitlint

Use [commitlint](https://commitlint.js.org/) to enforce conventional commits:

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

### Git Hooks

Use [husky](https://typicode.github.io/husky/) to validate commits:

```bash
npm install --save-dev husky
npx husky add .husky/commit-msg 'npx commitlint --edit $1'
```

### Semantic Release

Use [semantic-release](https://semantic-release.gitbook.io/) to automate versioning based on commits.

---

## Resources

- **Conventional Commits**: https://www.conventionalcommits.org/
- **Commitlint**: https://commitlint.js.org/
- **Angular Commit Guidelines**: https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit

---

**Template Version**: 1.0  
**Last Updated**: 2025-11-09
