# Naming Conventions Guide

This guide defines naming standards for files, variables, functions, components, and other code artifacts in BMAD projects.

---

## Table of Contents

1. [Files and Directories](#files-and-directories)
2. [JavaScript/TypeScript](#javascripttypescript)
3. [React Components](#react-components)
4. [CSS and Styling](#css-and-styling)
5. [Database](#database)
6. [Constants and Enums](#constants-and-enums)
7. [Functions and Methods](#functions-and-methods)
8. [Boolean Variables](#boolean-variables)
9. [Examples and Anti-patterns](#examples-and-anti-patterns)

---

## Files and Directories

### Rule: Use `kebab-case`

All file and directory names should use lowercase with hyphens.

**Format**: `word-word-word.extension`

**Examples**:

```
✅ user-authentication.md
✅ api-integration-guide.md
✅ payment-processing.ts
✅ user-profile-card.tsx
✅ epic-001-authentication.md
```

**Anti-patterns**:

```
❌ UserAuthentication.md        // PascalCase
❌ user_authentication.md        // snake_case
❌ userAuthentication.md         // camelCase
❌ USERAUTHENTICATION.md         // ALL CAPS
```

### Exceptions

- Configuration files: Follow tool conventions (`package.json`, `tsconfig.json`)
- Special files: `.gitignore`, `README.md`, `CHANGELOG.md`
- Component files in React: Use PascalCase matching component name

---

## JavaScript/TypeScript

### Variables: Use `camelCase`

**Format**: `firstWordSecondWord`

**Examples**:

```javascript
✅ const userName = 'Alice';
✅ const userProfileData = fetchUserData();
✅ let currentIndex = 0;
✅ const apiEndpoint = '/api/v1/users';
```

**Anti-patterns**:

```javascript
❌ const UserName = 'Alice';           // PascalCase
❌ const user_name = 'Alice';          // snake_case
❌ const USERN AME = 'Alice';           // spaces
```

### Private Variables: Prefix with `_`

```javascript
class UserService {
  _privateCache = new Map();

  _internalHelper() {
    return this._privateCache.get('key');
  }
}
```

---

## React Components

### Rule: Use `PascalCase`

Component names and their files should use PascalCase.

**Format**: `FirstWordSecondWord`

**Examples**:

```tsx
✅ UserProfile.tsx
✅ NavigationBar.tsx
✅ PaymentForm.tsx
✅ DashboardCard.tsx

// Component definition
export const UserProfile = () => {
  return <div>Profile</div>;
};
```

**Anti-patterns**:

```tsx
❌ userProfile.tsx          // camelCase
❌ user-profile.tsx         // kebab-case
❌ user_profile.tsx         // snake_case
```

### Composite Components

Use clear hierarchical naming:

```tsx
✅ UserProfile.tsx          // Parent
✅ UserProfileHeader.tsx    // Child
✅ UserProfileAvatar.tsx    // Child
✅ UserProfileStats.tsx     // Child
```

---

## CSS and Styling

### Classes: Use `kebab-case`

**Format**: `word-word-word`

**Examples**:

```css
✅ .user-profile-card {
}
✅ .navigation-menu {
}
✅ .button-primary {
}
✅ .alert-danger {
}
```

**Anti-patterns**:

```css
❌ .UserProfileCard { }      // PascalCase
❌ .user_profile_card { }    // snake_case
❌ .userProfileCard { }      // camelCase
```

### BEM Methodology (Optional)

Block Element Modifier pattern:

```css
.card {
} /* Block */
.card__header {
} /* Element */
.card--featured {
} /* Modifier */

.user-profile {
}
.user-profile__avatar {
}
.user-profile--compact {
}
```

### CSS Modules

```typescript
// UserProfile.module.css
.container { }
.header { }
.avatar { }

// UserProfile.tsx
import styles from './UserProfile.module.css';

<div className={styles.container}>
  <div className={styles.header}>...</div>
</div>
```

---

## Database

### Tables: Use `snake_case`

**Format**: `word_word_word`

**Examples**:

```sql
✅ CREATE TABLE user_profiles (...);
✅ CREATE TABLE payment_transactions (...);
✅ CREATE TABLE api_keys (...);
```

**Anti-patterns**:

```sql
❌ CREATE TABLE UserProfiles (...);      // PascalCase
❌ CREATE TABLE userProfiles (...);      // camelCase
❌ CREATE TABLE user-profiles (...);     // kebab-case
```

### Columns: Use `snake_case`

```sql
✅ user_id
✅ created_at
✅ updated_at
✅ is_active
✅ email_verified_at
```

### Foreign Keys: Use `table_id`

```sql
✅ user_id        -- References users table
✅ product_id     -- References products table
✅ order_id       -- References orders table
```

---

## Constants and Enums

### Rule: Use `SCREAMING_SNAKE_CASE`

**Format**: `WORD_WORD_WORD`

**Examples**:

```javascript
✅ const MAX_RETRY_COUNT = 3;
✅ const API_BASE_URL = 'https://api.example.com';
✅ const DEFAULT_TIMEOUT_MS = 5000;
✅ const HTTP_STATUS_OK = 200;
```

**Anti-patterns**:

```javascript
❌ const maxRetryCount = 3;           // camelCase
❌ const max_retry_count = 3;         // snake_case (lowercase)
❌ const MaxRetryCount = 3;           // PascalCase
```

### Enums (TypeScript)

```typescript
✅ enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
  GUEST = 'guest'
}

✅ enum HttpStatus {
  OK = 200,
  NOT_FOUND = 404,
  SERVER_ERROR = 500
}
```

---

## Functions and Methods

### Rule: Use `camelCase` with verb prefix

**Format**: `verbNoun` or `verbNounContext`

**Examples**:

```javascript
✅ getUserById(id)
✅ fetchUserData()
✅ createUserProfile(data)
✅ updateUserEmail(id, email)
✅ deleteUserAccount(id)
✅ validateEmailFormat(email)
✅ calculateTotalPrice(items)
✅ parseJsonResponse(response)
```

**Anti-patterns**:

```javascript
❌ UserById(id)              // Missing verb
❌ user(id)                  // Too vague
❌ GetUserById(id)           // PascalCase
❌ get_user_by_id(id)        // snake_case
```

### Common Verb Prefixes

**Data Operations**:

- `get` - Retrieve data (synchronous)
- `fetch` - Retrieve data (asynchronous/API)
- `load` - Load data into memory
- `create` - Create new record
- `update` - Modify existing record
- `delete` - Remove record
- `save` - Persist data

**Validation/Checks**:

- `validate` - Check validity
- `verify` - Confirm authenticity
- `check` - Boolean check
- `ensure` - Guarantee condition
- `is` - Boolean query (see Boolean section)

**Transformation**:

- `parse` - Convert string to data
- `format` - Convert data to string
- `transform` - Change structure
- `convert` - Change type
- `map` - Array transformation
- `filter` - Array filtering
- `reduce` - Array reduction

**Actions**:

- `send` - Transmit data
- `handle` - Process event
- `render` - Display UI
- `calculate` - Perform computation
- `generate` - Create output
- `process` - Transform input

---

## Boolean Variables

### Rule: Use `is`, `has`, `can`, or `should` prefix

**Format**: `prefixAdjective` or `prefixVerb`

**Examples**:

```javascript
✅ const isActive = true;
✅ const hasPermission = checkPermission();
✅ const canEdit = user.role === 'admin';
✅ const shouldRedirect = !isAuthenticated;
✅ const isLoading = false;
✅ const hasError = error !== null;
✅ const canDelete = permissions.includes('delete');
```

**Anti-patterns**:

```javascript
❌ const active = true;              // Missing prefix
❌ const permission = true;          // Ambiguous
❌ const loading = false;            // Not clearly boolean
```

### Common Boolean Patterns

**State**:

```javascript
(isActive, isEnabled, isDisabled, isVisible, isHidden);
(isOpen, isClosed, isExpanded, isCollapsed);
(isLoading, isProcessing, isPending);
```

**Permissions/Capabilities**:

```javascript
(canEdit, canDelete, canView, canCreate);
(hasAccess, hasPermission, hasRole);
```

**Validation**:

```javascript
(isValid, isInvalid, isEmpty, isComplete);
(hasError, hasWarning, hasChanges);
```

**Conditions**:

```javascript
(shouldUpdate, shouldRender, shouldRedirect);
(willExpire, willChange, willRetry);
```

---

## Examples and Anti-patterns

### File Structure Example

```
✅ Good Structure:
src/
├── components/
│   ├── UserProfile.tsx           // PascalCase component
│   ├── NavigationBar.tsx
│   └── PaymentForm.tsx
├── utils/
│   ├── date-formatter.ts         // kebab-case utility
│   ├── api-client.ts
│   └── validation-helpers.ts
├── services/
│   ├── user-service.ts
│   └── payment-service.ts
└── constants/
    └── api-endpoints.ts

❌ Bad Structure:
src/
├── components/
│   ├── user_profile.tsx          // snake_case
│   ├── Navigation-bar.tsx        // Mixed case
│   └── paymentForm.tsx           // camelCase
├── Utils/                        // Capitalized
│   ├── DateFormatter.ts          // PascalCase
│   └── api_client.ts             // snake_case
```

### Code Example

```typescript
// ✅ Good: Consistent naming conventions
const API_BASE_URL = 'https://api.example.com';
const MAX_RETRIES = 3;

interface UserProfile {
  userId: string;
  userName: string;
  isActive: boolean;
  createdAt: Date;
}

const fetchUserProfile = async (userId: string): Promise<UserProfile> => {
  const isValidId = validateUserId(userId);

  if (!isValidId) {
    throw new Error('Invalid user ID');
  }

  const response = await fetch(`${API_BASE_URL}/users/${userId}`);
  return parseUserResponse(response);
};

// ✅ Good: Clear, descriptive function names
const validateEmailFormat = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
};

const calculateTotalPrice = (items: CartItem[]): number => {
  return items.reduce((total, item) => total + item.price * item.quantity, 0);
};
```

```typescript
// ❌ Bad: Inconsistent naming
const api_base_url = 'https://api.example.com'; // snake_case
const MaxRetries = 3; // PascalCase

interface user_profile {
  // snake_case
  UserId: string; // PascalCase
  user_name: string; // snake_case
  active: boolean; // Missing prefix
  created: Date; // Ambiguous
}

const FetchUserProfile = async (user_id: string) => {
  // Mixed
  const valid = ValidateUserId(user_id); // Unclear
  // ...
};
```

---

## Quick Reference

| Context            | Convention               | Example              |
| ------------------ | ------------------------ | -------------------- |
| Files/Directories  | `kebab-case`             | `user-profile.ts`    |
| Variables          | `camelCase`              | `userName`           |
| Functions          | `camelCase` + verb       | `getUserData()`      |
| Constants          | `SCREAMING_SNAKE_CASE`   | `MAX_RETRY_COUNT`    |
| Classes/Components | `PascalCase`             | `UserProfile`        |
| CSS Classes        | `kebab-case`             | `.user-profile-card` |
| Database Tables    | `snake_case`             | `user_profiles`      |
| Database Columns   | `snake_case`             | `created_at`         |
| Booleans           | `is/has/can` + camelCase | `isActive`           |
| Private Fields     | `_camelCase`             | `_privateData`       |

---

## Enforcement

### ESLint Rules

```javascript
// .eslintrc.js
module.exports = {
  rules: {
    camelcase: ['error', { properties: 'never' }],
    '@typescript-eslint/naming-convention': [
      'error',
      { selector: 'variable', format: ['camelCase', 'UPPER_CASE'] },
      { selector: 'function', format: ['camelCase'] },
      { selector: 'typeLike', format: ['PascalCase'] },
    ],
  },
};
```

### Code Review Checklist

- [ ] File names use kebab-case
- [ ] Variables and functions use camelCase
- [ ] Constants use SCREAMING_SNAKE_CASE
- [ ] Components use PascalCase
- [ ] CSS classes use kebab-case
- [ ] Database tables/columns use snake_case
- [ ] Boolean variables have clear prefixes
- [ ] Functions have verb prefixes
- [ ] No mixed naming conventions in same file

---

## Related Documentation

- **[Docs Structure](../.docs-structure.md)** - Documentation organization
- **[Version Strategy](./version-strategy.md)** - Versioning approach
- **[FDD/SDD Guide](./fdd-sdd-guide.md)** - Development methodology

---

**Last Updated**: 2025-11-09  
**Version**: 1.0
