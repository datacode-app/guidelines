# Git Workflow Guide

## Purpose
This guide defines our Git branching strategy, commit conventions, and workflow practices to ensure smooth collaboration and maintainable version history.

## Branching Strategy

We use a **trunk-based development** approach with short-lived feature branches.

### Branch Types

#### `main` (or `master`)
- **Protected branch** - requires PR approval to merge
- Always deployable and stable
- Represents production code
- Never commit directly to main

#### Feature Branches
- Format: `feature/ticket-number-short-description` or `feature/short-description`
- Examples:
  - `feature/AUTH-123-oauth-integration`
  - `feature/user-dashboard`
- Created from: `main`
- Merged into: `main`
- Lifespan: 1-3 days (maximum 1 week)

#### Bugfix Branches
- Format: `bugfix/ticket-number-short-description`
- Example: `bugfix/PAY-456-payment-validation`
- Created from: `main`
- Merged into: `main`

#### Hotfix Branches
- Format: `hotfix/ticket-number-short-description`
- Example: `hotfix/PROD-789-login-crash`
- Created from: `main` (or production tag)
- Merged into: `main` immediately
- Used for critical production issues only

#### Release Branches (if applicable)
- Format: `release/v1.2.3`
- Created when preparing a release
- Only bugfixes allowed, no new features
- Merged into both `main` and tagged

### Branch Naming Rules

✅ **Good:**
```
feature/add-payment-gateway
bugfix/fix-email-validation
hotfix/critical-security-patch
feature/USER-123-profile-page
```

❌ **Bad:**
```
my-branch
test
fix
johns-work
branch1
```

## Commit Message Convention

We follow **Conventional Commits** specification for clear, parseable commit history.

### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, semicolons, etc.)
- **refactor**: Code refactoring (no functionality change)
- **perf**: Performance improvements
- **test**: Adding or updating tests
- **chore**: Maintenance tasks (dependencies, build config, etc.)
- **ci**: CI/CD changes
- **revert**: Reverting a previous commit

### Examples

#### Simple Commits
```
feat: add user authentication

fix: resolve null pointer in payment processing

docs: update API documentation for v2 endpoints

chore: upgrade React to v18.2
```

#### With Scope
```
feat(auth): implement OAuth2 login flow

fix(payments): handle declined card responses

test(api): add integration tests for user endpoints

refactor(dashboard): simplify chart rendering logic
```

#### With Body and Footer
```
feat(notifications): add push notification support

Implemented Firebase Cloud Messaging integration for
real-time push notifications on mobile devices.

Closes #234
```

#### Breaking Changes
```
feat(api): change authentication response format

BREAKING CHANGE: The /auth/login endpoint now returns
{ accessToken, refreshToken } instead of { token }.
Update client code accordingly.

Closes #456
```

### Commit Best Practices

✅ **Do:**
- Write in imperative mood ("add feature" not "added feature")
- Keep subject line under 50 characters
- Capitalize subject line
- No period at end of subject line
- Separate subject from body with blank line
- Wrap body at 72 characters
- Explain *what* and *why*, not *how*
- Reference ticket numbers
- Make atomic commits (one logical change)

❌ **Don't:**
- Write vague messages like "fix bug" or "update code"
- Commit unrelated changes together
- Commit work-in-progress code to main
- Include sensitive data in commit messages

## Daily Workflow

### Starting New Work

```bash
# 1. Ensure you're on main and up to date
git checkout main
git pull origin main

# 2. Create feature branch
git checkout -b feature/USER-123-add-export-feature

# 3. Make your changes and commit frequently
git add .
git commit -m "feat(export): add CSV export button to dashboard"

# 4. Push to remote regularly (at least daily)
git push -u origin feature/USER-123-add-export-feature
```

### During Development

```bash
# Commit frequently (atomic commits)
git add src/components/Export.tsx
git commit -m "feat(export): create Export component"

git add src/utils/csv.ts
git commit -m "feat(export): add CSV generation utility"

git add tests/export.test.ts
git commit -m "test(export): add Export component tests"

# Push regularly
git push
```

### Before Creating PR

```bash
# 1. Ensure you're up to date with main
git checkout main
git pull origin main

# 2. Return to your branch
git checkout feature/USER-123-add-export-feature

# 3. Rebase onto latest main
git rebase main

# 4. Resolve any conflicts if they arise
# (edit conflicted files, then)
git add .
git rebase --continue

# 5. Force push (safe because it's your branch)
git push --force-with-lease

# 6. Create PR via GitHub UI
```

### After PR Approval

```bash
# Use GitHub's "Squash and merge" or "Merge" button
# Then delete the branch via GitHub UI

# Locally, clean up
git checkout main
git pull origin main
git branch -d feature/USER-123-add-export-feature
```

## Advanced Workflows

### Updating Your Branch with Main

**Option 1: Rebase (Preferred)**
```bash
git checkout feature/my-feature
git fetch origin
git rebase origin/main
# Resolve conflicts if any
git push --force-with-lease
```

**Option 2: Merge (When rebase is too complex)**
```bash
git checkout feature/my-feature
git fetch origin
git merge origin/main
# Resolve conflicts if any
git push
```

### Fixing a Commit Message

```bash
# Last commit only
git commit --amend -m "fix(auth): correct validation logic"
git push --force-with-lease

# Older commits (interactive rebase)
git rebase -i HEAD~3  # Edit last 3 commits
# Mark commits to edit, then amend and continue
```

### Splitting a Large Commit

```bash
# Undo last commit but keep changes
git reset HEAD~1

# Stage and commit in smaller chunks
git add src/feature1.ts
git commit -m "feat: add feature 1"

git add src/feature2.ts
git commit -m "feat: add feature 2"
```

### Cherry-Picking a Commit

```bash
# Apply specific commit from another branch
git cherry-pick abc123de

# Cherry-pick without committing (to modify)
git cherry-pick -n abc123de
```

### Stashing Work

```bash
# Save work-in-progress temporarily
git stash save "WIP: working on dashboard redesign"

# List stashes
git stash list

# Apply most recent stash
git stash pop

# Apply specific stash
git stash apply stash@{1}
```

## Merge Strategies

### Squash and Merge (Default)
- **Use for**: Most feature branches
- **Result**: All commits squashed into one on main
- **Pros**: Clean history on main
- **Cons**: Loses individual commit history

### Merge Commit
- **Use for**: Release branches, important feature work
- **Result**: All commits preserved, creates merge commit
- **Pros**: Full history preserved
- **Cons**: Can clutter main history

### Rebase and Merge
- **Use for**: Very clean, well-structured commits
- **Result**: Linear history without merge commits
- **Pros**: Cleanest history
- **Cons**: Requires disciplined commits

## Git Hooks

We use automated hooks for quality control:

### Pre-commit
- Runs linters (ESLint, Prettier)
- Checks for debugging statements
- Validates commit message format
- Runs quick unit tests

### Pre-push
- Runs full test suite
- Checks for secrets/credentials
- Validates branch naming

### Commit-msg
- Validates commit message follows Conventional Commits
- Checks for issue/ticket references

## Common Scenarios

### Accidentally Committed to Main

```bash
# 1. Create a branch from current state
git branch feature/my-work

# 2. Reset main to origin
git checkout main
git reset --hard origin/main

# 3. Switch to your branch
git checkout feature/my-work
```

### Need to Undo Last Commit

```bash
# Keep changes (undo commit only)
git reset HEAD~1

# Discard changes (dangerous!)
git reset --hard HEAD~1
```

### Resolve Merge Conflicts

```bash
# 1. Start merge/rebase (conflict occurs)
git rebase main

# 2. View conflicts
git status

# 3. Edit conflicted files (look for <<<<<<, ======, >>>>>>)

# 4. Mark as resolved
git add <file>

# 5. Continue
git rebase --continue

# If stuck, abort
git rebase --abort
```

### Lost Commit Recovery

```bash
# View reflog
git reflog

# Find lost commit hash
# Checkout or cherry-pick
git checkout abc123
```

## Git Configuration

### Recommended Global Config

```bash
# Your identity
git config --global user.name "Your Name"
git config --global user.email "you@datacode.app"

# Default branch name
git config --global init.defaultBranch main

# Better diffs
git config --global diff.algorithm histogram

# Reuse recorded conflict resolutions
git config --global rerere.enabled true

# Prune deleted remote branches on fetch
git config --global fetch.prune true

# Use force-with-lease by default
git config --global alias.pushf "push --force-with-lease"

# Better log
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## Don'ts - Critical Rules

❌ **NEVER:**
- Force push to `main` or shared branches
- Commit secrets, API keys, passwords, or tokens
- Rewrite public history (commits others have pulled)
- Commit large binary files without Git LFS
- Use `git push --force` (use `--force-with-lease` instead)
- Commit commented-out code (delete it, Git remembers)
- Create branches that live longer than a week

## Metrics We Track

- Average branch lifespan (target: < 3 days)
- PR size (target: < 400 lines)
- Time from PR creation to merge (target: < 24 hours)
- Commit message compliance (target: > 95%)

## Troubleshooting

### Branch is Behind Main by Many Commits
```bash
git checkout feature/my-feature
git rebase origin/main
git push --force-with-lease
```

### Accidentally Deleted Branch
```bash
git reflog
git checkout -b feature/recovered-branch abc123
```

### Want to Undo a Pushed Commit
```bash
git revert abc123
git push
# Never use reset on public branches
```

## Resources

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Git Flight Rules](https://github.com/k88hudson/git-flight-rules)
- Internal Git workshop recordings (Wiki link)

---

**Remember**: Git is a powerful tool. Use it to enable collaboration, not hinder it. When in doubt, ask for help in #engineering Slack channel.
