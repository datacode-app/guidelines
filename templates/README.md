# Templates Directory

This directory contains ready-to-use configuration files and templates that implement our engineering guidelines.

## Quick Start

### For New Projects

```bash
# Copy all starter files to your new project
cp -r templates/project-starter/* /path/to/your/project/
cd /path/to/your/project

# Run setup
chmod +x scripts/setup-guidelines.sh
./scripts/setup-guidelines.sh

# Install dependencies
npm install

# Done! Guidelines are now enforced automatically.
```

### For Existing Projects

Pick and choose what you need:

```bash
# Code style
cp templates/project-starter/.eslintrc.js /path/to/project/
cp templates/project-starter/.prettierrc /path/to/project/

# Git hooks
cp -r templates/project-starter/.husky /path/to/project/
cp templates/project-starter/commitlint.config.js /path/to/project/

# CI/CD
cp -r templates/project-starter/.github /path/to/project/

# Scripts
cp -r templates/project-starter/scripts /path/to/project/
```

## What's Included

### `/project-starter/`
Complete starter template with all configurations:

- **Code Quality**
  - `.eslintrc.js` - ESLint configuration
  - `.prettierrc` - Prettier configuration
  - `.editorconfig` - Editor configuration

- **Git Workflow**
  - `.husky/` - Git hooks (pre-commit, commit-msg)
  - `commitlint.config.js` - Commit message validation
  - `.github/PULL_REQUEST_TEMPLATE.md` - PR template

- **CI/CD**
  - `.github/workflows/quality.yml` - Automated quality checks
  - `.github/workflows/commit-lint.yml` - Commit message validation

- **Testing**
  - `jest.config.js` - Jest configuration with coverage thresholds

- **Scripts**
  - `scripts/setup-guidelines.sh` - Automated setup
  - `scripts/check-secrets.sh` - Secret detection
  - `scripts/validate-pr.sh` - Local PR validation

- **Editor**
  - `.vscode/settings.json` - VS Code configuration

- **Package Management**
  - `package.json` - Scripts and dependencies template

## Usage Examples

### Setting Up a New React Project

```bash
# Create new React app
npx create-react-app my-app --template typescript
cd my-app

# Apply guidelines
cp -r /path/to/guidelines/templates/project-starter/{.eslintrc.js,.prettierrc,.husky,commitlint.config.js} .
cp -r /path/to/guidelines/templates/project-starter/.github .
cp -r /path/to/guidelines/templates/project-starter/scripts .

# Run setup
chmod +x scripts/setup-guidelines.sh
./scripts/setup-guidelines.sh
```

### Setting Up a Node.js API Project

```bash
# Create new Node project
mkdir my-api && cd my-api
npm init -y

# Apply guidelines
cp -r /path/to/guidelines/templates/project-starter/* .

# Install TypeScript and dependencies
npm install typescript @types/node --save-dev

# Run setup
./scripts/setup-guidelines.sh
```

## Customization

These templates are starting points. Customize them for your specific needs:

1. **ESLint**: Add project-specific rules to `.eslintrc.js`
2. **CI/CD**: Adjust workflows in `.github/workflows/`
3. **Coverage**: Modify thresholds in `jest.config.js`
4. **Scripts**: Extend `package.json` scripts

## Maintenance

When guidelines are updated:

1. Update these templates
2. Version the change
3. Communicate to teams
4. Provide migration guide if breaking

## Support

Questions about templates? Ask in #engineering Slack or see [Implementation Guide](../implementation-guide.md).
