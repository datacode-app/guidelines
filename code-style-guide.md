# Code Style Guide

## Purpose
Consistent code style improves readability, reduces cognitive load, and prevents bikeshedding in code reviews. This guide defines our formatting, naming, and organizational standards.

## Core Philosophy

**Optimize for Reading, Not Writing**
Code is read 10x more than it's written. Prioritize clarity over cleverness.

**Consistency Over Personal Preference**
We use automated tools to enforce style. Don't fight the linter.

**Standards, Not Suggestions**
These aren't optional. All code must pass automated checks before merge.

---

## General Principles

### 1. Readability First
```javascript
// ❌ Clever but unclear
const u = d.filter(x => x.a && !x.d).map(x => ({...x, s: 'active'}));

// ✅ Clear and explicit
const activeUsers = users
  .filter(user => user.isActive && !user.isDeleted)
  .map(user => ({ ...user, status: 'active' }));
```

### 2. Avoid Magic Numbers
```javascript
// ❌ What does 86400000 mean?
const expiry = Date.now() + 86400000;

// ✅ Self-documenting
const MILLISECONDS_PER_DAY = 24 * 60 * 60 * 1000;
const expiry = Date.now() + MILLISECONDS_PER_DAY;
```

### 3. Fail Fast and Explicitly
```javascript
// ❌ Silent failures
function processUser(user) {
  if (user && user.email) {
    sendEmail(user.email);
  }
}

// ✅ Explicit validation
function processUser(user) {
  if (!user) {
    throw new Error('User is required');
  }
  if (!user.email) {
    throw new Error('User email is required');
  }
  sendEmail(user.email);
}
```

---

## Language-Specific Standards

## JavaScript/TypeScript

### File Organization

```typescript
// 1. Imports (external first, then internal)
import React, { useState } from 'react';
import { Button } from '@mui/material';

import { UserService } from '@/services/UserService';
import { formatDate } from '@/utils/date';
import type { User } from '@/types';

// 2. Constants
const MAX_RETRIES = 3;
const DEFAULT_TIMEOUT = 5000;

// 3. Types/Interfaces (TypeScript)
interface Props {
  userId: string;
  onSuccess: (user: User) => void;
}

// 4. Component/Main logic
export function UserProfile({ userId, onSuccess }: Props) {
  // Component implementation
}

// 5. Helper functions (if not extracted)
function validateEmail(email: string): boolean {
  // Implementation
}

// 6. Default export (if applicable)
export default UserProfile;
```

### Naming Conventions

```typescript
// Constants: SCREAMING_SNAKE_CASE
const API_BASE_URL = 'https://api.example.com';
const MAX_FILE_SIZE_MB = 10;

// Variables/Functions: camelCase
const userId = '123';
const userName = 'John Doe';
function calculateTotal() {}
function getUserById(id: string) {}

// Classes/Types/Interfaces: PascalCase
class UserService {}
interface UserData {}
type UserId = string;

// Components: PascalCase
function UserProfile() {}
const LoginForm = () => {};

// Private properties/methods: prefix with underscore (optional)
class Service {
  private _cache: Map<string, any>;

  private _clearCache() {}
}

// Boolean variables: is/has/should prefix
const isActive = true;
const hasPermission = false;
const shouldUpdate = true;

// Event handlers: handle/on prefix
function handleClick() {}
function onSubmit() {}
const handleUserDelete = () => {};
```

### Function Style

```typescript
// ✅ Prefer explicit return types (TypeScript)
function getUser(id: string): Promise<User> {
  return userService.findById(id);
}

// ✅ Prefer named functions for top-level
function calculateTax(amount: number): number {
  return amount * 0.1;
}

// ✅ Arrow functions for callbacks/inline
users.map(user => user.name);
const doubleNumbers = numbers.map(n => n * 2);

// ✅ Destructure parameters when appropriate
function createUser({ name, email, role }: UserInput) {
  // ...
}

// ✅ Use default parameters
function fetchUsers(page = 1, limit = 10) {
  // ...
}
```

### Async/Await

```typescript
// ❌ Promise chains
function getUser(id) {
  return fetch(`/api/users/${id}`)
    .then(res => res.json())
    .then(data => data.user)
    .catch(err => console.error(err));
}

// ✅ Async/await
async function getUser(id: string): Promise<User> {
  try {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();
    return data.user;
  } catch (error) {
    console.error('Failed to fetch user:', error);
    throw error;
  }
}
```

### Error Handling

```typescript
// ✅ Always handle errors explicitly
try {
  const result = await riskyOperation();
  return result;
} catch (error) {
  logger.error('Operation failed', { error, context });
  throw new ApplicationError('Failed to complete operation', { cause: error });
}

// ✅ Use custom error types
class ValidationError extends Error {
  constructor(message: string, public field: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

// ✅ Provide context in errors
throw new ValidationError('Email is invalid', 'email');
```

### Comments

```typescript
// ❌ Obvious comments (don't)
// Increment counter
counter++;

// ❌ Commented-out code (delete it, Git remembers)
// const oldFunction = () => { ... }

// ✅ Explain WHY, not WHAT
// Use exponential backoff to avoid overwhelming the API
await retry(apiCall, { maxAttempts: 3, backoff: 'exponential' });

// ✅ Document complex logic
/**
 * Calculates user permissions based on role hierarchy.
 *
 * Admins inherit all permissions from lower roles.
 * Role hierarchy: admin > manager > user > guest
 *
 * @param user - User object with role
 * @returns Set of permission strings
 */
function calculatePermissions(user: User): Set<string> {
  // Implementation
}

// ✅ TODO comments with ticket reference
// TODO(TICKET-123): Refactor to use new permission system
```

### Formatting (Prettier Enforced)

```typescript
// Indentation: 2 spaces (no tabs)
function example() {
  if (condition) {
    doSomething();
  }
}

// Max line length: 100 characters
// Long lines break at logical points
const result = veryLongFunctionName(
  firstArgument,
  secondArgument,
  thirdArgument
);

// Trailing commas: always (easier diffs)
const obj = {
  name: 'John',
  age: 30,
  email: 'john@example.com', // Trailing comma
};

// Semicolons: always (TypeScript/JavaScript)
const x = 5;
const y = 10;

// Quotes: single for code, double for JSX
const name = 'John';
const element = <div className="container">Hello</div>;

// Object/Array spacing
const obj = { name: 'John', age: 30 };
const arr = [1, 2, 3];
```

---

## React/JSX

### Component Structure

```typescript
import React, { useState, useEffect } from 'react';
import type { FC } from 'react';

// 1. Types
interface UserCardProps {
  user: User;
  onEdit?: (user: User) => void;
  className?: string;
}

// 2. Component
export const UserCard: FC<UserCardProps> = ({
  user,
  onEdit,
  className = ''
}) => {
  // 3. Hooks (useState, useEffect, custom hooks)
  const [isEditing, setIsEditing] = useState(false);

  useEffect(() => {
    // Effect logic
  }, []);

  // 4. Event handlers
  const handleEdit = () => {
    setIsEditing(true);
    onEdit?.(user);
  };

  // 5. Render helpers (if needed)
  const renderActions = () => (
    <div className="actions">
      <button onClick={handleEdit}>Edit</button>
    </div>
  );

  // 6. Early returns
  if (!user) {
    return <div>No user data</div>;
  }

  // 7. Main render
  return (
    <div className={`user-card ${className}`}>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      {renderActions()}
    </div>
  );
};
```

### JSX Best Practices

```typescript
// ✅ Self-closing tags
<UserCard user={user} />

// ✅ Boolean props shorthand
<Button disabled />
// Not: <Button disabled={true} />

// ✅ Conditional rendering - ternary for both cases
{isLoading ? <Spinner /> : <Content />}

// ✅ Conditional rendering - && for one case
{error && <ErrorMessage error={error} />}

// ✅ Avoid inline functions in render (use useCallback)
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);

<Button onClick={handleClick} />
// Not: <Button onClick={() => doSomething(id)} />

// ✅ Key prop for lists (stable, unique ID)
{users.map(user => (
  <UserCard key={user.id} user={user} />
))}
// Not: key={index}
```

---

## CSS/Styling

### Naming (BEM or Tailwind)

```css
/* BEM (Block Element Modifier) */
.user-card { }
.user-card__header { }
.user-card__title { }
.user-card--highlighted { }

/* Or use utility-first (Tailwind) */
<div className="flex items-center gap-4 p-4 bg-white rounded-lg shadow">
```

### Organization

```css
/* 1. Layout */
.container {
  display: flex;
  flex-direction: column;

  /* 2. Size */
  width: 100%;
  max-width: 1200px;

  /* 3. Spacing */
  padding: 1rem;
  margin: 0 auto;

  /* 4. Typography */
  font-size: 1rem;
  line-height: 1.5;

  /* 5. Visual */
  background-color: white;
  border: 1px solid #e5e7eb;
  border-radius: 0.5rem;

  /* 6. Misc */
  cursor: pointer;
  transition: all 0.2s;
}
```

### Variables (CSS Custom Properties)

```css
:root {
  /* Colors */
  --color-primary: #3b82f6;
  --color-secondary: #8b5cf6;
  --color-success: #10b981;
  --color-error: #ef4444;

  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;

  /* Typography */
  --font-sans: system-ui, -apple-system, sans-serif;
  --font-mono: 'Courier New', monospace;
}
```

---

## SQL

### Formatting

```sql
-- ✅ Keywords in UPPERCASE
SELECT
  u.id,
  u.name,
  u.email,
  COUNT(o.id) AS order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.is_active = TRUE
  AND u.created_at >= '2024-01-01'
GROUP BY u.id, u.name, u.email
HAVING COUNT(o.id) > 5
ORDER BY order_count DESC
LIMIT 10;

-- ✅ Use meaningful aliases
SELECT
  u.name AS user_name,
  o.total AS order_total
FROM users u
JOIN orders o ON u.id = o.user_id;

-- ✅ Indent subqueries
SELECT *
FROM (
  SELECT
    user_id,
    SUM(amount) AS total_spent
  FROM orders
  GROUP BY user_id
) AS user_totals
WHERE total_spent > 1000;
```

### Naming Conventions

```sql
-- Tables: plural, snake_case
CREATE TABLE users (...);
CREATE TABLE order_items (...);

-- Columns: snake_case
CREATE TABLE users (
  id UUID PRIMARY KEY,
  first_name VARCHAR(100),
  email VARCHAR(255),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Indexes: idx_table_column(s)
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_user_created ON orders(user_id, created_at);

-- Foreign keys: fk_table_referenced_table
ALTER TABLE orders
  ADD CONSTRAINT fk_orders_users
  FOREIGN KEY (user_id) REFERENCES users(id);
```

---

## Git Commit Messages

See [Git Workflow Guide](git-workflow-guide.md) for full details.

**Format:**
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Examples:**
```
feat(auth): add OAuth2 login support

fix(payments): handle declined card responses correctly

docs(api): update authentication endpoints documentation

refactor(dashboard): simplify chart rendering logic

test(users): add integration tests for user CRUD operations
```

---

## Testing

### Test Naming

```typescript
// Pattern: "should [expected behavior] when [condition]"
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user when valid data provided', async () => {
      // Test
    });

    it('should throw ValidationError when email is invalid', async () => {
      // Test
    });

    it('should throw ConflictError when email already exists', async () => {
      // Test
    });
  });
});
```

### Test Organization

```typescript
describe('Component/Function Name', () => {
  // Setup
  let mockDependency: jest.Mock;

  beforeEach(() => {
    mockDependency = jest.fn();
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  // Group related tests
  describe('specific functionality', () => {
    it('should behave correctly in normal case', () => {
      // Arrange
      const input = 'test';

      // Act
      const result = functionUnderTest(input);

      // Assert
      expect(result).toBe('expected');
    });
  });
});
```

---

## File Naming

### JavaScript/TypeScript

```
// Components: PascalCase
UserProfile.tsx
LoginForm.tsx

// Utilities/Helpers: camelCase
dateFormatter.ts
apiClient.ts

// Constants: SCREAMING_SNAKE_CASE or camelCase
API_CONSTANTS.ts
config.ts

// Tests: same name as file being tested + .test or .spec
UserProfile.test.tsx
dateFormatter.spec.ts

// Types: camelCase or descriptive
types.ts
user.types.ts
api.types.ts
```

### Structure

```
src/
├── components/
│   ├── common/          # Shared components
│   │   ├── Button.tsx
│   │   └── Input.tsx
│   └── features/        # Feature-specific components
│       └── user/
│           ├── UserProfile.tsx
│           └── UserProfile.test.tsx
├── services/            # Business logic
│   ├── UserService.ts
│   └── UserService.test.ts
├── utils/               # Pure utility functions
│   ├── date.ts
│   └── validation.ts
├── hooks/               # Custom React hooks
│   └── useAuth.ts
├── types/               # TypeScript types
│   └── user.types.ts
├── constants/           # App constants
│   └── api.ts
└── config/              # Configuration
    └── env.ts
```

---

## Automated Enforcement

### Tools We Use

```json
// package.json
{
  "scripts": {
    "lint": "eslint src --ext .ts,.tsx",
    "lint:fix": "eslint src --ext .ts,.tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx,css,md}\"",
    "format:check": "prettier --check \"src/**/*.{ts,tsx,css,md}\"",
    "typecheck": "tsc --noEmit"
  },
  "devDependencies": {
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "eslint": "^8.0.0",
    "eslint-config-prettier": "^9.0.0",
    "eslint-plugin-react": "^7.33.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "prettier": "^3.0.0"
  }
}
```

### ESLint Configuration

```javascript
// .eslintrc.js
module.exports = {
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'prettier' // Must be last
  ],
  rules: {
    // Enforce our conventions
    '@typescript-eslint/explicit-function-return-type': 'warn',
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn',
    'react/prop-types': 'off', // Using TypeScript
    'react/react-in-jsx-scope': 'off', // React 17+
    'no-console': ['warn', { allow: ['warn', 'error'] }],
    'prefer-const': 'error',
    'no-var': 'error'
  }
};
```

### Prettier Configuration

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "avoid"
}
```

### Pre-commit Hooks

```json
// package.json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{ts,tsx}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{css,md}": [
      "prettier --write"
    ]
  }
}
```

---

## Code Review Checklist

During code review, verify:

- [ ] Code follows naming conventions
- [ ] Functions are appropriately sized (< 50 lines ideally)
- [ ] No commented-out code
- [ ] No TODO without ticket reference
- [ ] Linter passes with no warnings
- [ ] Prettier formatted
- [ ] Type-safe (TypeScript - no `any` unless justified)
- [ ] Error handling present
- [ ] Meaningful variable/function names

---

## Language-Specific Resources

### JavaScript/TypeScript
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [TypeScript Do's and Don'ts](https://www.typescriptlang.org/docs/handbook/declaration-files/do-s-and-don-ts.html)
- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)

### React
- [React Best Practices](https://react.dev/learn/thinking-in-react)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)

### SQL
- [SQL Style Guide](https://www.sqlstyle.guide/)

---

## Exceptions

Style rules can be broken when:

1. **Performance-critical code** - Document why
2. **Third-party integration** - Match their style in integration layer
3. **Legacy code** - Don't mix styles; either update all or none
4. **Generated code** - Mark clearly, don't manually edit

When breaking rules, add comment:
```typescript
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const dynamicData: any = thirdPartyLib.getData();
// Reason: Third-party library doesn't provide types
```

---

## When in Doubt

1. **Check existing codebase** - Follow established patterns
2. **Ask the team** - Discuss in #engineering
3. **Propose a standard** - If unclear, suggest and document
4. **Automate** - If debated repeatedly, add linter rule

---

**Remember**: The goal is consistency and readability, not perfection. Use automated tools to enforce style so code reviews can focus on logic, architecture, and business value.

**Last Updated**: 2024-03-16
**Next Review**: Quarterly
