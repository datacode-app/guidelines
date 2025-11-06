# Implementation Guide

## Purpose
This guide bridges the gap between "knowing what to do" (guidelines) and "actually doing it" (implementation). It provides ready-to-use configuration files, scripts, and templates that make following our guidelines automatic and effortless.

---

## Philosophy

**Make the Right Thing the Easy Thing**

Following best practices should be:
- ✅ Automatic (enforced by tools)
- ✅ Fast (pre-configured, not manual)
- ✅ Clear (immediate feedback)
- ❌ NOT manual, slow, or confusing

---

## Quick Start: New Project Setup

### Option 1: Complete Setup (Recommended)

```bash
# 1. Clone the guidelines repo
git clone https://github.com/datacode-app/guidelines.git

# 2. Copy templates to your project
cp -r guidelines/templates/project-starter/* /path/to/your/project/
cd /path/to/your/project

# 3. Run setup script
./scripts/setup-guidelines.sh

# 4. Install dependencies
npm install

# Done! All guidelines are now enforced automatically.
```

### Option 2: Manual Setup (Pick and Choose)

Follow the sections below to set up individual pieces.

---

## Code Style Enforcement

### 1. ESLint Configuration

**File**: `.eslintrc.js`

```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'prettier', // Must be last
  ],
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 'latest',
    sourceType: 'module',
  },
  plugins: [
    'react',
    'react-hooks',
    '@typescript-eslint',
  ],
  rules: {
    // Enforce our code style guide
    '@typescript-eslint/explicit-function-return-type': 'warn',
    '@typescript-eslint/no-unused-vars': ['error', {
      argsIgnorePattern: '^_',
      varsIgnorePattern: '^_'
    }],
    '@typescript-eslint/no-explicit-any': 'warn',
    'no-console': ['warn', { allow: ['warn', 'error'] }],
    'prefer-const': 'error',
    'no-var': 'error',
    'react/prop-types': 'off', // Using TypeScript
    'react/react-in-jsx-scope': 'off', // React 17+
  },
  settings: {
    react: {
      version: 'detect',
    },
  },
};
```

### 2. Prettier Configuration

**File**: `.prettierrc`

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

**File**: `.prettierignore`

```
# Dependencies
node_modules/
dist/
build/
coverage/

# Generated files
*.min.js
*.bundle.js
package-lock.json
yarn.lock

# Config files
.env
.env.*
```

### 3. Editor Configuration

**File**: `.editorconfig`

```ini
root = true

[*]
indent_style = space
indent_size = 2
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true

[*.md]
trim_trailing_whitespace = false
```

### 4. VS Code Settings (Optional)

**File**: `.vscode/settings.json`

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "typescript.tsdk": "node_modules/typescript/lib"
}
```

---

## Git Workflow Automation

### 1. Commit Message Validation

**File**: `commitlint.config.js`

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [
      2,
      'always',
      [
        'feat',     // New feature
        'fix',      // Bug fix
        'docs',     // Documentation
        'style',    // Formatting
        'refactor', // Code restructuring
        'perf',     // Performance
        'test',     // Tests
        'chore',    // Maintenance
        'ci',       // CI/CD
        'revert',   // Revert commit
      ],
    ],
    'type-case': [2, 'always', 'lowerase'],
    'type-empty': [2, 'never'],
    'subject-empty': [2, 'never'],
    'subject-case': [2, 'always', 'sentence-case'],
    'subject-full-stop': [2, 'never', '.'],
    'header-max-length': [2, 'always', 100],
  },
};
```

### 2. Pre-commit Hooks

**File**: `.husky/pre-commit`

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

# Run lint-staged
npx lint-staged

# Check for secrets
./scripts/check-secrets.sh

echo "✅ Pre-commit checks passed!"
```

**File**: `package.json` (lint-staged config)

```json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx}": [
      "eslint --fix",
      "prettier --write",
      "jest --bail --findRelatedTests"
    ],
    "*.{json,css,md}": [
      "prettier --write"
    ]
  }
}
```

### 3. Commit Message Hook

**File**: `.husky/commit-msg`

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

# Validate commit message
npx --no -- commitlint --edit "$1"
```

### 4. Secrets Detection Script

**File**: `scripts/check-secrets.sh`

```bash
#!/bin/bash

# Patterns that indicate secrets
PATTERNS=(
  "password.*=.*['\"].*['\"]"
  "api.?key.*=.*['\"].*['\"]"
  "secret.*=.*['\"].*['\"]"
  "token.*=.*['\"].*['\"]"
  "bearer.*['\"].*['\"]"
  "sk_live_"
  "pk_live_"
)

# Check staged files
STAGED_FILES=$(git diff --cached --name-only)

for FILE in $STAGED_FILES; do
  for PATTERN in "${PATTERNS[@]}"; do
    if grep -iE "$PATTERN" "$FILE" > /dev/null 2>&1; then
      echo "❌ ERROR: Possible secret detected in $FILE"
      echo "Pattern matched: $PATTERN"
      echo ""
      echo "If this is a false positive, add to .secretsignore"
      exit 1
    fi
  done
done

exit 0
```

### 5. Pull Request Template

**File**: `.github/PULL_REQUEST_TEMPLATE.md`

```markdown
## What
<!-- Brief description of what this PR does (1-2 sentences) -->

## Why
<!-- Why is this change needed? Link to ticket if applicable -->
Fixes #

## How
<!-- High-level explanation of the approach taken -->

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated (if applicable)
- [ ] Manual testing performed
- [ ] Edge cases considered

## Screenshots/Videos
<!-- If UI changes, add before/after screenshots -->

## Deployment Notes
<!-- Any special steps needed? Database migrations? Environment variables? -->

## Checklist
- [ ] Code follows [Code Style Guide](../code-style-guide.md)
- [ ] Tests passing (coverage ≥ 80%)
- [ ] Self-reviewed the diff
- [ ] No debugging code (console.log, debugger)
- [ ] No secrets or sensitive data committed
- [ ] Documentation updated
- [ ] Breaking changes documented (if any)
- [ ] [Definition of Done](../definition-of-done.md) criteria met

## Reviewer Notes
<!-- Anything specific for reviewers to focus on? -->
```

---

## CI/CD Automation

### 1. Quality Checks Workflow

**File**: `.github/workflows/quality.yml`

```yaml
name: Quality Checks

on:
  pull_request:
    branches: [main, develop]
  push:
    branches: [main, develop]

jobs:
  lint:
    name: Lint Code
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint

      - name: Run Prettier check
        run: npm run format:check

  test:
    name: Run Tests
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests with coverage
        run: npm run test:coverage

      - name: Check coverage threshold
        run: |
          COVERAGE=$(npx nyc report --reporter=text-summary | grep "Lines" | awk '{print $3}' | sed 's/%//')
          if (( $(echo "$COVERAGE < 80" | bc -l) )); then
            echo "❌ Coverage is $COVERAGE%, minimum is 80%"
            exit 1
          fi
          echo "✅ Coverage is $COVERAGE%"

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info

  security:
    name: Security Audit
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Run npm audit
        run: npm audit --audit-level=high

      - name: Check for secrets
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          base: main

  typecheck:
    name: TypeScript Type Check
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run type check
        run: npm run typecheck
```

### 2. Commit Message Validation

**File**: `.github/workflows/commit-lint.yml`

```yaml
name: Lint Commit Messages

on:
  pull_request:
    types: [opened, edited, synchronize, reopened]

jobs:
  commitlint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install commitlint
        run: |
          npm install --save-dev @commitlint/cli @commitlint/config-conventional

      - name: Validate PR title
        run: echo "${{ github.event.pull_request.title }}" | npx commitlint

      - name: Validate commit messages
        run: npx commitlint --from ${{ github.event.pull_request.base.sha }} --to ${{ github.event.pull_request.head.sha }}
```

---

## Testing Infrastructure

### 1. Jest Configuration

**File**: `jest.config.js`

```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  roots: ['<rootDir>/src'],
  testMatch: ['**/__tests__/**/*.ts', '**/?(*.)+(spec|test).ts'],
  transform: {
    '^.+\\.ts$': 'ts-jest',
  },
  collectCoverageFrom: [
    'src/**/*.{js,ts}',
    '!src/**/*.d.ts',
    '!src/**/*.test.ts',
    '!src/**/__tests__/**',
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
  coverageReporters: ['text', 'text-summary', 'html', 'lcov'],
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
};
```

### 2. Package.json Scripts

**File**: `package.json` (scripts section)

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",

    "lint": "eslint src --ext .ts,.tsx",
    "lint:fix": "eslint src --ext .ts,.tsx --fix",
    "format": "prettier --write \"src/**/*.{ts,tsx,css,md}\"",
    "format:check": "prettier --check \"src/**/*.{ts,tsx,css,md}\"",
    "typecheck": "tsc --noEmit",

    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:ci": "jest --ci --coverage --maxWorkers=2",

    "validate": "npm run lint && npm run typecheck && npm run test:coverage",
    "prepare": "husky install"
  },
  "devDependencies": {
    "@commitlint/cli": "^17.0.0",
    "@commitlint/config-conventional": "^17.0.0",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "eslint": "^8.0.0",
    "eslint-config-prettier": "^9.0.0",
    "eslint-plugin-react": "^7.33.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "husky": "^8.0.0",
    "jest": "^29.0.0",
    "lint-staged": "^13.0.0",
    "prettier": "^3.0.0",
    "ts-jest": "^29.0.0",
    "typescript": "^5.0.0"
  }
}
```

---

## Setup Automation

### Master Setup Script

**File**: `scripts/setup-guidelines.sh`

```bash
#!/bin/bash

echo "🚀 Setting up Datacode.app Engineering Guidelines..."
echo ""

# Check if npm is installed
if ! command -v npm &> /dev/null; then
    echo "❌ npm is not installed. Please install Node.js first."
    exit 1
fi

# Install dependencies
echo "📦 Installing dependencies..."
npm install

# Initialize Git hooks
echo "🪝 Setting up Git hooks..."
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
chmod +x .husky/pre-commit
chmod +x .husky/commit-msg

# Make scripts executable
echo "🔧 Making scripts executable..."
chmod +x scripts/*.sh

# Run initial validation
echo "✅ Running initial validation..."
npm run lint
npm run typecheck

echo ""
echo "🎉 Setup complete!"
echo ""
echo "Next steps:"
echo "  1. Review .env.example and create your .env file"
echo "  2. Run 'npm run dev' to start development"
echo "  3. Read the Quick Reference: docs/quick-reference.md"
echo ""
echo "All guidelines are now enforced automatically! 🚀"
```

---

## Quick Validation Script

**File**: `scripts/validate-pr.sh`

```bash
#!/bin/bash

echo "🔍 Validating PR requirements..."
echo ""

# Run linter
echo "1️⃣ Running linter..."
npm run lint || { echo "❌ Linting failed"; exit 1; }
echo "✅ Linting passed"
echo ""

# Run type check
echo "2️⃣ Running type check..."
npm run typecheck || { echo "❌ Type check failed"; exit 1; }
echo "✅ Type check passed"
echo ""

# Run tests with coverage
echo "3️⃣ Running tests..."
npm run test:coverage || { echo "❌ Tests failed"; exit 1; }
echo "✅ Tests passed"
echo ""

# Check coverage
echo "4️⃣ Checking coverage threshold..."
COVERAGE=$(npx nyc report --reporter=text-summary | grep "Lines" | awk '{print $3}' | sed 's/%//')
if (( $(echo "$COVERAGE < 80" | bc -l) )); then
  echo "❌ Coverage is $COVERAGE%, minimum is 80%"
  exit 1
fi
echo "✅ Coverage is $COVERAGE%"
echo ""

# Check for secrets
echo "5️⃣ Checking for secrets..."
./scripts/check-secrets.sh || { echo "❌ Possible secrets detected"; exit 1; }
echo "✅ No secrets detected"
echo ""

echo "🎉 All checks passed! Your PR is ready."
```

---

## Installation Guide

### For New Projects

1. **Install Node.js** (if not already installed)
   ```bash
   # macOS
   brew install node

   # Ubuntu/Debian
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt-get install -y nodejs
   ```

2. **Copy template files to your project**
   ```bash
   cp -r templates/project-starter/* /path/to/your/project/
   cd /path/to/your/project
   ```

3. **Run setup script**
   ```bash
   chmod +x scripts/setup-guidelines.sh
   ./scripts/setup-guidelines.sh
   ```

4. **Commit the configuration**
   ```bash
   git add .
   git commit -m "chore: add engineering guidelines configuration"
   git push
   ```

### For Existing Projects

1. **Create a new branch**
   ```bash
   git checkout -b chore/add-guidelines-config
   ```

2. **Copy configuration files** (pick what you need)
   ```bash
   # ESLint & Prettier
   cp templates/.eslintrc.js .
   cp templates/.prettierrc .

   # Git hooks
   cp -r templates/.husky .
   cp templates/commitlint.config.js .

   # CI/CD
   mkdir -p .github/workflows
   cp templates/.github/workflows/* .github/workflows/

   # Scripts
   mkdir -p scripts
   cp templates/scripts/* scripts/
   chmod +x scripts/*.sh
   ```

3. **Install dependencies**
   ```bash
   npm install --save-dev \
     eslint \
     prettier \
     @typescript-eslint/eslint-plugin \
     @typescript-eslint/parser \
     eslint-config-prettier \
     husky \
     lint-staged \
     @commitlint/cli \
     @commitlint/config-conventional
   ```

4. **Initialize hooks**
   ```bash
   npx husky install
   npx husky add .husky/pre-commit "npx lint-staged"
   npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
   ```

5. **Test and commit**
   ```bash
   npm run lint
   npm run test
   git add .
   git commit -m "chore: add engineering guidelines configuration"
   git push -u origin chore/add-guidelines-config
   ```

---

## Troubleshooting

### "Husky hooks not running"

```bash
# Reinstall hooks
rm -rf .husky
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
chmod +x .husky/*
```

### "ESLint errors in node_modules"

Add to `.eslintignore`:
```
node_modules/
dist/
build/
.next/
```

### "Tests failing in CI but passing locally"

Ensure consistent Node.js version:
```yaml
# In .github/workflows/*.yml
- uses: actions/setup-node@v3
  with:
    node-version: '18'  # Match your local version
```

### "Commit message validation failing"

Check your message format:
```bash
# ✅ Good
feat: add user authentication
fix(api): resolve timeout issue

# ❌ Bad
Added new feature
fixed bug
```

---

## Maintenance

### Updating Dependencies

```bash
# Check for outdated packages
npm outdated

# Update non-breaking changes
npm update

# Update to latest (may have breaking changes)
npx npm-check-updates -u
npm install
```

### Updating Configuration

1. Test changes in a feature branch
2. Run `npm run validate` to ensure nothing breaks
3. Create PR with configuration changes
4. Get team review before merging

---

## Resources

- **ESLint**: https://eslint.org/docs/latest/
- **Prettier**: https://prettier.io/docs/en/
- **Husky**: https://typicode.github.io/husky/
- **Commitlint**: https://commitlint.js.org/
- **lint-staged**: https://github.com/okonet/lint-staged
- **GitHub Actions**: https://docs.github.com/en/actions

---

## Getting Help

- **Configuration issues**: Ask in #engineering Slack
- **CI/CD problems**: Tag @devops-team
- **Questions about guidelines**: See [Guidelines Governance](guidelines-governance.md)

---

**With these configurations, following our guidelines becomes automatic. The tools do the work, so you can focus on building great software.**

**Last Updated**: 2024-03-16
**Maintained by**: Engineering Leadership
