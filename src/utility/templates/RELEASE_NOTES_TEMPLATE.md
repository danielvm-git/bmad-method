# Release Notes Template

Use this template for creating release notes following Semantic Versioning (SemVer).

---

# Release v{MAJOR}.{MINOR}.{PATCH}

**Release Date**: YYYY-MM-DD  
**Release Type**: [Major | Minor | Patch]

## Summary

Brief overview of this release (2-3 sentences describing the main theme or focus).

---

## Added

New features and capabilities introduced in this release.

- **Feature Name**: Description of what was added and why it's valuable
- **Feature Name**: Description of what was added and why it's valuable

## Changed

Changes to existing functionality.

- **Component/Feature**: Description of what changed and impact on users
- **Component/Feature**: Description of what changed and impact on users

## Fixed

Bug fixes and issue resolutions.

- **Issue #123**: Description of the bug and how it was fixed
- **Issue #124**: Description of the bug and how it was fixed

## Deprecated

Features that are deprecated and will be removed in future versions.

- **Feature Name**: What is deprecated, when it will be removed, and recommended alternative

## Removed

Features or functionality removed in this release.

- **Feature Name**: What was removed and migration path if applicable

## Security

Security improvements and vulnerability fixes.

- **CVE-YYYY-XXXXX**: Description of security issue and fix

---

## Breaking Changes

List any breaking changes that require user action or migration.

- **Change Description**: What breaks, who is affected, and migration steps

## Migration Guide

Step-by-step instructions for upgrading from previous version.

1. **Backup**: Backup your current installation
2. **Update Dependencies**: Run `npm install` or equivalent
3. **Configuration Changes**: Update any configuration files
4. **Data Migration**: Run migration scripts if needed
5. **Verify**: Test critical functionality

## Known Issues

List of known issues in this release and workarounds if available.

- **Issue Description**: Temporary workaround or expected fix timeline

---

## Version Comparison

- **Full Changelog**: [v{PREVIOUS}...v{CURRENT}](https://github.com/org/repo/compare/v{PREVIOUS}...v{CURRENT})
- **Previous Release**: [v{PREVIOUS}](https://github.com/org/repo/releases/tag/v{PREVIOUS})

## Credits

- **Contributors**: @username1, @username2, @username3
- **Special Thanks**: Recognition for significant contributions

---

## Semantic Versioning Guide

**MAJOR** (X.0.0): Incompatible API changes or breaking changes  
**MINOR** (x.X.0): New functionality in a backward-compatible manner  
**PATCH** (x.x.X): Backward-compatible bug fixes

### BMAD Version Strategy

- **One Epic = One Minor Version**: Each epic represents new functionality and bumps the minor version
- **Bug Fixes = Patch**: Bug fixes increment the patch version
- **Breaking Changes = Major**: API changes or major refactors bump the major version

## Examples by Release Type

### Major Release (Breaking Changes)

```markdown
# Release v2.0.0

## Summary

Complete rewrite of authentication system with OAuth 2.0 support.

## Breaking Changes

- Authentication API endpoints changed from `/auth/*` to `/api/v2/auth/*`
- Session tokens now use JWT format instead of UUID
- User object structure changed (see migration guide)
```

### Minor Release (New Features)

```markdown
# Release v1.5.0

## Summary

Added dark mode support and export functionality.

## Added

- **Dark Mode**: Toggle between light and dark themes in settings
- **Export to CSV**: Export data tables to CSV format
```

### Patch Release (Bug Fixes)

```markdown
# Release v1.4.3

## Summary

Bug fixes for form validation and performance improvements.

## Fixed

- **Issue #234**: Form validation error on empty optional fields
- **Issue #235**: Memory leak in real-time updates
```

---

**Template Version**: 1.0  
**Last Updated**: 2025-11-09
