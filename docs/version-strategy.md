# Version Strategy Guide

This guide defines the versioning approach for BMAD projects following Semantic Versioning with the principle: **One Epic = One Minor Version**.

---

## Core Principle

**One Epic = One Minor Version**

Every completed epic represents new functionality and increments the minor version number. This creates a clear relationship between feature development and version numbers.

---

## Semantic Versioning (SemVer)

BMAD follows [Semantic Versioning 2.0.0](https://semver.org/).

### Format

```
MAJOR.MINOR.PATCH
```

### Version Components

**MAJOR** (X.0.0) - Breaking Changes

Incompatible API changes or major architectural shifts that break backward compatibility.

**Examples**:

- Complete API redesign (REST → GraphQL)
- Database schema migration requiring data loss
- Removing deprecated features
- Changing authentication mechanism

**MINOR** (x.X.0) - New Features

New functionality added in a backward-compatible manner. **One completed epic = one minor version bump**.

**Examples**:

- Adding user authentication system (Epic)
- Implementing payment processing (Epic)
- Adding dashboard analytics (Epic)
- Introducing notification system (Epic)

**PATCH** (x.x.X) - Bug Fixes

Backward-compatible bug fixes that don't add new features.

**Examples**:

- Fixing form validation bug
- Resolving memory leak
- Correcting calculation error
- Patching security vulnerability

---

## Epic-to-Version Mapping

### When an Epic is Completed

1. **Epic Done** → Bump minor version
2. **Update CHANGELOG** → Document epic completion
3. **Tag Release** → Create git tag
4. **Deploy** → Release to production

### Example Timeline

```
Sprint 1:
- Complete Epic-001: User Authentication
- Version: v1.1.0 (minor bump)

Sprint 2:
- Complete Epic-002: Dashboard Analytics
- Version: v1.2.0 (minor bump)
- Fix bug in authentication
- Version: v1.2.1 (patch)

Sprint 3:
- Complete Epic-003: Payment Processing
- Version: v1.3.0 (minor bump)

Sprint 6:
- Complete Epic-004: API v2 (Breaking Changes)
- Version: v2.0.0 (major bump)
```

---

## Version Bumping Rules

### Bump MAJOR When

- Breaking API changes
- Removing features
- Incompatible data migrations
- Changing core architecture
- Dropping support for platform versions

**Example**:

```
v1.5.2 → v2.0.0

BREAKING CHANGE: Authentication moved from sessions to JWT.
All clients must update authentication flow.
```

### Bump MINOR When

- Completing an epic (new feature)
- Adding new API endpoints
- Adding optional parameters
- Implementing new capabilities
- Adding backward-compatible functionality

**Example**:

```
v1.4.3 → v1.5.0

Epic: Add payment processing with Stripe integration.
New endpoints: POST /api/payments, GET /api/payments/:id
```

### Bump PATCH When

- Fixing bugs
- Improving performance
- Updating documentation
- Refactoring code (no functional changes)
- Security patches

**Example**:

```
v1.5.0 → v1.5.1

Fix: Resolve validation error in payment form.
```

---

## Pre-release Versions

### Alpha (Development)

Early development, frequent breaking changes.

**Format**: `v1.5.0-alpha.1`, `v1.5.0-alpha.2`

**Use when**: Feature under active development, not production-ready

### Beta (Testing)

Feature-complete, undergoing testing.

**Format**: `v1.5.0-beta.1`, `v1.5.0-beta.2`

**Use when**: Ready for broader testing, API may still change

### Release Candidate

Final testing before production release.

**Format**: `v1.5.0-rc.1`, `v1.5.0-rc.2`

**Use when**: Stable, only critical bugs will be fixed

### Example Flow

```
v1.4.0              (Current stable)
v1.5.0-alpha.1      (Start Epic-005 development)
v1.5.0-alpha.2      (More epic development)
v1.5.0-beta.1       (Feature complete, testing)
v1.5.0-beta.2       (Bug fixes during testing)
v1.5.0-rc.1         (Release candidate)
v1.5.0              (Production release - Epic-005 complete!)
```

---

## Git Tagging Strategy

### Creating Tags

```bash
# Patch release (bug fix)
git tag -a v1.5.1 -m "fix: resolve payment validation error"

# Minor release (epic completion)
git tag -a v1.6.0 -m "feat(epic-006): add notification system"

# Major release (breaking changes)
git tag -a v2.0.0 -m "BREAKING CHANGE: migrate to GraphQL API"

# Push tags
git push origin --tags
```

### Tag Message Format

```
v{VERSION}

{COMMIT_TYPE}: {DESCRIPTION}

Epic: {EPIC_ID} - {EPIC_NAME}
Stories: {STORY_IDS}

Changes:
- {CHANGE_1}
- {CHANGE_2}

Breaking Changes (if MAJOR):
- {BREAKING_CHANGE_1}
```

**Example**:

```
v1.5.0

feat(epic-005): add payment processing system

Epic: EPIC-005 - Payment Processing Integration
Stories: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6

Changes:
- Stripe payment gateway integration
- Payment history tracking
- Refund processing
- Invoice generation
- Webhook handling for payment events
```

---

## Release Workflow

### 1. Complete Epic

- All stories done
- All tests passing
- Documentation updated
- Code reviewed and merged

### 2. Update CHANGELOG

```markdown
## [1.5.0] - 2025-11-15

### Added

- **Epic-005: Payment Processing** - Stripe integration with payment history
  - Stories: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6
  - Function Points: 21
  - Sprint: 3

### Fixed

- Minor bug fixes in user profile display

### Changed

- Improved error messages for form validation
```

### 3. Bump Version

```bash
# Update package.json version
npm version minor -m "feat(epic-005): add payment processing"

# This creates a commit and tag automatically
```

### 4. Push and Deploy

```bash
# Push changes and tags
git push origin main
git push origin --tags

# Deploy to production
npm run deploy:production
```

---

## Version Matrix

| Change Type         | Epic Involved | Breaking Change | Version Bump | Example         |
| ------------------- | ------------- | --------------- | ------------ | --------------- |
| New Epic            | Yes           | No              | MINOR        | v1.4.0 → v1.5.0 |
| Bug Fix             | No            | No              | PATCH        | v1.5.0 → v1.5.1 |
| Security Patch      | No            | No              | PATCH        | v1.5.1 → v1.5.2 |
| API Change          | Maybe         | Yes             | MAJOR        | v1.5.2 → v2.0.0 |
| Performance         | No            | No              | PATCH        | v2.0.0 → v2.0.1 |
| New Epic + Breaking | Yes           | Yes             | MAJOR        | v2.0.1 → v3.0.0 |
| Docs Only           | No            | No              | PATCH\*      | v3.0.0 → v3.0.1 |

\* Some teams don't bump version for docs-only changes

---

## Epic Lifecycle and Versioning

### Phase 1: Epic Planning

```
Epic Status: Backlog
Version Impact: None yet
```

### Phase 2: Epic Development

```
Epic Status: In Progress
Version: v1.5.0-alpha.1 (development)
Stories: Being implemented
```

### Phase 3: Epic Testing

```
Epic Status: In Review
Version: v1.5.0-beta.1 (testing)
All stories: Complete
Tests: Passing
```

### Phase 4: Epic Completion

```
Epic Status: Done
Version: v1.5.0 (production)
Release Notes: Published
Git Tag: Created
```

---

## Multiple Epics in One Sprint

If multiple epics complete in the same sprint, you have two options:

### Option 1: Sequential Releases (Recommended)

Release each epic separately as you complete them.

```
Sprint 3:
- Day 3: Complete Epic-005 → Release v1.5.0
- Day 10: Complete Epic-006 → Release v1.6.0
```

**Pros**: Clear epic-to-version mapping, faster feedback  
**Cons**: More releases to manage

### Option 2: Combined Release

Wait for all epics and release together.

```
Sprint 3:
- Day 3: Complete Epic-005 (hold)
- Day 10: Complete Epic-006 (hold)
- Day 14: Release v1.6.0 (both epics)
```

**Pros**: Fewer releases, coordinated launch  
**Cons**: Loses epic-to-version clarity

**Recommendation**: Use Option 1 (Sequential) for most cases. Use Option 2 only when epics are highly interdependent.

---

## Version Documentation

### package.json

```json
{
  "name": "my-project",
  "version": "1.5.0",
  "description": "Project description"
}
```

### CHANGELOG.md

Follow [Keep a Changelog](https://keepachangelog.com/) format.

```markdown
# Changelog

## [Unreleased]

### Added

- Feature in development

## [1.5.0] - 2025-11-15

### Added

- **Epic-005: Payment Processing** - Complete Stripe integration

## [1.4.0] - 2025-11-01

### Added

- **Epic-004: Dashboard Analytics** - Real-time metrics dashboard

## [1.3.1] - 2025-10-28

### Fixed

- Bug in user authentication flow

[Unreleased]: https://github.com/org/repo/compare/v1.5.0...HEAD
[1.5.0]: https://github.com/org/repo/compare/v1.4.0...v1.5.0
[1.4.0]: https://github.com/org/repo/compare/v1.3.1...v1.4.0
[1.3.1]: https://github.com/org/repo/compare/v1.3.0...v1.3.1
```

---

## FAQ

### Q: What if an epic is too large?

**A**: Split it into smaller epics. Each sub-epic gets its own minor version.

```
Epic-005: Payment System (too large)
↓ Split into:
Epic-005a: Payment Gateway → v1.5.0
Epic-005b: Payment History → v1.6.0
Epic-005c: Refund Processing → v1.7.0
```

### Q: Do hotfixes follow this rule?

**A**: No. Hotfixes are always PATCH bumps regardless of complexity, because they fix existing functionality rather than adding features.

```
v1.5.0 (production)
↓ Critical bug found
v1.5.1 (hotfix) - PATCH bump
```

### Q: What about documentation updates?

**A**: Pure documentation changes can be PATCH bumps or no version change at all, depending on your team's preference.

### Q: Can we release without completing an epic?

**A**: Partial epic completion should trigger PATCH bumps for any bug fixes or improvements, but only complete epics trigger MINOR bumps.

---

## Tools and Automation

### Conventional Commits

Use commit messages to drive automatic versioning:

```bash
feat(epic-005): add Stripe payment integration  # MINOR
fix: resolve payment form validation            # PATCH
feat!: migrate to GraphQL API                   # MAJOR (! indicates breaking)
```

### Semantic Release

Automate versioning based on commit messages:

```bash
npm install --save-dev semantic-release

# .releaserc.json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/npm",
    "@semantic-release/git"
  ]
}
```

---

## Related Documentation

- **[Epic Backlog Template](../src/utility/templates/EPIC_BACKLOG_TEMPLATE.md)** - Track epics and versions
- **[Commit Message Template](../src/utility/templates/COMMIT_MESSAGE_TEMPLATE.md)** - Conventional commits
- **[Release Notes Template](../src/utility/templates/RELEASE_NOTES_TEMPLATE.md)** - Release documentation

---

**Last Updated**: 2025-11-09  
**Version**: 1.0  
**Semantic Versioning**: [semver.org](https://semver.org/)
