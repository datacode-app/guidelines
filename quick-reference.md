# Quick Reference Guide

## Purpose
Fast access to commonly used commands, workflows, and checklists. Print or bookmark this page for daily reference.

---

## 🚀 Daily Workflow

### Starting Your Day

```bash
# 1. Update your local main branch
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/TICKET-123-description

# 3. Start coding!
```

### During Development

```bash
# Check what changed
git status
git diff

# Commit frequently (atomic commits)
git add src/file.ts
git commit -m "feat(scope): clear description"

# Push at least daily
git push -u origin feature/TICKET-123-description
```

### Before Creating PR

```bash
# Update with latest main
git fetch origin
git rebase origin/main

# Run checks
npm run lint
npm run test
npm run typecheck

# Push (force if rebased)
git push --force-with-lease

# Create PR via GitHub UI
```

---

## 📝 Commit Message Format

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

### Types
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code restructuring
- `test`: Adding tests
- `chore`: Maintenance

### Examples
```
feat(auth): add OAuth2 login support
fix(payments): handle declined cards correctly
docs(api): update authentication endpoints
refactor(dashboard): simplify chart rendering
test(users): add integration tests for CRUD
```

---

## 🌿 Common Git Operations

### Branch Management

```bash
# List branches
git branch                    # Local
git branch -r                 # Remote
git branch -a                 # All

# Switch branches
git checkout main
git checkout feature/my-branch

# Create and switch
git checkout -b feature/new-branch

# Delete branch
git branch -d feature/old-branch      # Safe delete
git branch -D feature/old-branch      # Force delete
```

### Updating Your Branch

```bash
# Option 1: Rebase (preferred - clean history)
git fetch origin
git rebase origin/main
git push --force-with-lease

# Option 2: Merge (when rebase is complex)
git fetch origin
git merge origin/main
git push
```

### Stashing Work

```bash
# Save work-in-progress
git stash save "WIP: feature description"

# List stashes
git stash list

# Apply latest stash
git stash pop

# Apply specific stash
git stash apply stash@{1}

# Clear all stashes
git stash clear
```

### Undoing Changes

```bash
# Undo uncommitted changes
git checkout -- file.ts              # Single file
git reset --hard HEAD                # All files (careful!)

# Undo last commit (keep changes)
git reset HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# Amend last commit
git commit --amend -m "Updated message"
```

### Viewing History

```bash
# Show commits
git log
git log --oneline
git log --graph --oneline --all

# Show changes in commit
git show abc123

# Show file history
git log -- path/to/file

# Find who changed what
git blame file.ts
```

---

## ✅ Pre-Commit Checklist

Before committing:
- [ ] Code runs without errors
- [ ] Tests pass (`npm test`)
- [ ] Linter passes (`npm run lint`)
- [ ] Types check (`npm run typecheck`)
- [ ] No debugging code (console.log, debugger)
- [ ] No commented-out code
- [ ] Meaningful commit message

---

## 🔍 Pre-PR Checklist

Before creating pull request:
- [ ] Branch updated with latest main
- [ ] All commits follow commit message format
- [ ] Tests added for new features
- [ ] Tests updated for changed features
- [ ] Documentation updated
- [ ] No secrets or sensitive data
- [ ] PR description filled out
- [ ] Self-reviewed the diff
- [ ] Ready for production

---

## 📋 PR Description Template

```markdown
## What
Brief description of what this PR does (1-2 sentences)

## Why
Why is this change needed? Link to ticket if applicable

## How
High-level explanation of the approach

## Testing
- [ ] Unit tests added/updated
- [ ] Manual testing performed
- [ ] Edge cases considered

## Screenshots
(If UI changes)

## Deployment Notes
Any special steps? Database migrations? Environment variables?

## Checklist
- [ ] Code follows style guide
- [ ] Tests passing
- [ ] Documentation updated
- [ ] No secrets committed
```

---

## 🧪 Testing Commands

```bash
# Run all tests
npm test

# Run specific test file
npm test UserService.test.ts

# Run tests in watch mode
npm test -- --watch

# Run tests with coverage
npm test -- --coverage

# Run only unit tests
npm run test:unit

# Run only integration tests
npm run test:integration

# Run E2E tests
npm run test:e2e
```

---

## 🔒 Security Checks

### Before Committing

```bash
# Check for secrets
git diff | grep -i "api.key\|password\|secret\|token"

# Run security audit
npm audit

# Fix vulnerabilities
npm audit fix
```

### Common Secret Patterns to Avoid

```javascript
❌ Don't Commit:
const API_KEY = 'sk_live_abc123';
const password = 'password123';
process.env.SECRET = 'hardcoded-secret';

✅ Instead Use:
const API_KEY = process.env.API_KEY;
// And keep .env in .gitignore
```

---

## 🐛 Debugging Workflow

### When Something Breaks

1. **Reproduce** - Can you make it happen again?
2. **Isolate** - Minimum steps to reproduce?
3. **Check Logs** - What do errors say?
4. **Recent Changes** - What changed recently?
5. **Git Bisect** - Find the breaking commit
6. **Ask for Help** - Stuck? Ask in #engineering

### Git Bisect (Find Breaking Commit)

```bash
# Start bisect
git bisect start
git bisect bad                    # Current commit is broken
git bisect good abc123            # This old commit worked

# Git will checkout commits for you to test
# After each test:
git bisect good    # If works
git bisect bad     # If broken

# When done
git bisect reset
```

---

## 📦 NPM/Package Management

```bash
# Install dependencies
npm install                       # From package.json
npm ci                            # Clean install (uses lock file exactly)

# Add dependency
npm install lodash
npm install --save-dev jest       # Dev dependency

# Update dependencies
npm update
npm outdated                      # Check for updates

# Remove dependency
npm uninstall lodash

# Check for vulnerabilities
npm audit
npm audit fix
```

---

## 🔥 Emergency Hotfix Process

```bash
# 1. Create hotfix branch from main
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug-fix

# 2. Make minimal fix
# (edit files)

# 3. Test thoroughly
npm test

# 4. Commit and push
git add .
git commit -m "fix: critical bug description"
git push -u origin hotfix/critical-bug-fix

# 5. Create PR marked "HOTFIX" - fast-track review

# 6. After merge, create follow-up ticket for:
#    - Comprehensive tests
#    - Root cause analysis
#    - Preventive measures
```

---

## 📊 Code Review Quick Guide

### As Author

```markdown
1. Self-review first
2. Keep PR small (< 400 lines)
3. Write clear description
4. Respond within 24 hours
5. Be open to feedback
```

### As Reviewer

```markdown
Use labels in comments:
🔴 BLOCKER: Must be fixed
⚠️ IMPORTANT: Should be addressed
💡 SUGGESTION: Nice-to-have
❓ QUESTION: Asking for clarification
🎉 PRAISE: Acknowledge good work

Response time:
- Small PR (< 50 lines): 4 hours
- Medium PR (50-200 lines): 24 hours
- Large PR (> 200 lines): 48 hours
```

---

## 🚨 Incident Response Quick Steps

### SEV 1 (Critical - Full Outage)

```
1. Acknowledge alert (< 5 min)
2. Create #incident-YYYY-MM-DD channel
3. Post initial status
4. Assess impact
5. Try quick fixes:
   - Revert last deployment
   - Scale up resources
   - Disable problematic feature
6. Update status every 30 min
7. Document everything
8. Schedule post-mortem within 48h
```

### SEV 2 (High - Major Degradation)

```
1. Respond within 30 min
2. Notify relevant teams
3. Update status page
4. Investigate and fix
5. Update stakeholders every 2 hours
```

---

## 🛠️ Local Development

### Environment Setup

```bash
# Copy example env file
cp .env.example .env

# Edit with your values
nano .env

# Start development server
npm run dev

# Run database migrations
npm run migrate

# Seed database with test data
npm run seed
```

### Common Issues

```bash
# Port already in use
Error: EADDRINUSE :::3000
Solution: lsof -ti:3000 | xargs kill

# Node modules corrupted
Solution: rm -rf node_modules package-lock.json && npm install

# Database connection failed
Solution: Check DATABASE_URL in .env, ensure DB is running

# Type errors after pulling
Solution: npm install (new packages may have been added)
```

---

## 📚 Documentation Quick Tips

### When to Document

- [ ] Complex logic that's not obvious
- [ ] Public APIs and functions
- [ ] Configuration options
- [ ] Setup/installation steps
- [ ] Architecture decisions

### Where to Document

```
Code Comments:
  - Why, not what
  - Complex algorithms
  - Non-obvious decisions

README:
  - Project overview
  - Setup instructions
  - Basic usage

API Docs:
  - OpenAPI/Swagger spec
  - Request/response examples

Wiki/Confluence:
  - Architecture diagrams
  - Process documentation
  - Runbooks
```

---

## 🎯 Definition of Done - Quick Check

Before marking task done:

- [ ] Code written and works
- [ ] Tests written and passing
- [ ] Code reviewed and approved
- [ ] Documentation updated
- [ ] Deployed to staging and verified
- [ ] Product team approved
- [ ] Ready for production

---

## 📞 Getting Help

```
Stuck on code?          → #engineering Slack
Security question?      → security@datacode.app
Production issue?       → Page on-call via PagerDuty
Process question?       → Your team lead
Tool/access issue?      → #it-support
```

---

## 🔗 Useful Links

```
Code Guidelines:     ./code-style-guide.md
Git Workflow:        ./git-workflow-guide.md
Testing Standards:   ./testing-standards.md
Security Practices:  ./security-best-practices.md
API Standards:       ./api-design-standards.md
Incident Response:   ./incident-response-guide.md
```

---

## 💡 Keyboard Shortcuts (VS Code)

```
Cmd/Ctrl + P          → Quick file open
Cmd/Ctrl + Shift + F  → Search in files
Cmd/Ctrl + `          → Toggle terminal
Cmd/Ctrl + /          → Toggle comment
Cmd/Ctrl + D          → Select next occurrence
Cmd/Ctrl + Shift + L  → Select all occurrences
Cmd/Ctrl + Shift + K  → Delete line
Alt + Up/Down         → Move line up/down
Cmd/Ctrl + Shift + P  → Command palette
```

---

## 📖 Reading Code Tips

When joining new codebase or reviewing:

1. **Start with README** - Understand project purpose
2. **Find main entry point** - index.ts, main.ts, app.ts
3. **Follow one feature** - Trace from UI → API → DB
4. **Read tests** - They show how code is used
5. **Check recent PRs** - See what's changing
6. **Ask questions** - No one knows everything

---

**Print this page and keep it handy!**

**Pro tip**: Bookmark this in your browser as "Dev Quick Ref" for instant access.

**Last Updated**: 2024-03-16
