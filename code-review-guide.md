# Code Review Guidelines

## Purpose
Code reviews are essential for maintaining code quality, sharing knowledge, and catching issues early. This guide establishes best practices for both authors and reviewers to make code reviews effective and constructive.

## Core Principles

1. **Reviews are about code, not people** - Focus on the work, not the individual
2. **Everyone's code gets reviewed** - No exceptions, including senior developers
3. **Small, frequent reviews** - Better than large, infrequent ones
4. **Reviews are a priority** - Respond within 24 hours during work days
5. **Learning opportunity** - Both reviewer and author gain knowledge

## For Authors (PR Creators)

### Before Requesting Review

- [ ] **Self-review your code** - Read through the entire diff first
- [ ] **Run tests locally** - Ensure all tests pass
- [ ] **Check for debugging code** - Remove console.logs, print statements, debugger statements
- [ ] **Update documentation** - README, API docs, inline comments as needed
- [ ] **Keep PRs focused** - One logical change per PR (aim for < 400 lines changed)
- [ ] **Rebase on latest main** - Ensure your branch is up to date

### PR Description Template

```markdown
## What
Brief description of what this PR does (1-2 sentences)

## Why
Why is this change needed? Link to ticket/issue if applicable

## How
High-level explanation of the approach taken

## Testing
- [ ] Unit tests added/updated
- [ ] Manual testing performed
- [ ] Edge cases considered

## Screenshots/Videos
(If UI changes) Add before/after screenshots

## Deployment Notes
Any special steps needed for deployment? Database migrations? Environment variables?

## Checklist
- [ ] Code follows our style guide
- [ ] Tests added/updated and passing
- [ ] Documentation updated
- [ ] No sensitive data (API keys, passwords) committed
- [ ] Breaking changes documented
```

### Responding to Feedback

- **Be responsive** - Reply within 24 hours
- **Ask clarifying questions** - If feedback is unclear, ask for elaboration
- **Explain decisions** - If you disagree, explain your reasoning respectfully
- **Acknowledge and learn** - Accept feedback gracefully
- **Mark resolved** - When you've addressed a comment, mark it resolved or reply with "Fixed in abc123"

## For Reviewers

### Review Checklist

#### Functionality
- [ ] Does the code do what it's supposed to do?
- [ ] Are edge cases handled?
- [ ] Are error conditions handled appropriately?
- [ ] Is the user experience smooth?

#### Code Quality
- [ ] Is the code readable and maintainable?
- [ ] Are names (variables, functions, classes) clear and descriptive?
- [ ] Is there duplicated code that could be extracted?
- [ ] Are functions/methods single-purpose and reasonably sized?
- [ ] Is the complexity justified?

#### Architecture & Design
- [ ] Does this fit well with our overall architecture?
- [ ] Are there better design patterns that could be used?
- [ ] Is this the right place for this code?
- [ ] Are abstractions at the appropriate level?

#### Testing
- [ ] Are there adequate tests?
- [ ] Do tests cover happy path and edge cases?
- [ ] Are tests readable and maintainable?
- [ ] Are test names descriptive?

#### Security
- [ ] Are inputs validated and sanitized?
- [ ] Are there any potential injection vulnerabilities?
- [ ] Is sensitive data handled securely?
- [ ] Are authentication/authorization checks in place?

#### Performance
- [ ] Are there obvious performance issues?
- [ ] Are database queries optimized?
- [ ] Are there unnecessary network calls?
- [ ] Is caching used appropriately?

#### Documentation
- [ ] Are complex parts documented?
- [ ] Are public APIs documented?
- [ ] Is the PR description clear?
- [ ] Are breaking changes clearly noted?

### Providing Feedback

#### Use Clear Labels

- **🔴 BLOCKER**: Must be fixed before merge
- **⚠️ IMPORTANT**: Should be addressed, discuss if you disagree
- **💡 SUGGESTION**: Nice-to-have, author decides
- **❓ QUESTION**: Asking for clarification
- **🎉 PRAISE**: Acknowledge good work!

#### Write Constructive Comments

**Good Examples:**
```
💡 SUGGESTION: Consider extracting this logic into a separate function for reusability.

⚠️ IMPORTANT: This query could cause N+1 problem. Consider using eager loading here.

❓ QUESTION: What happens if userId is null? Should we handle that case?

🎉 PRAISE: Great test coverage! Love the edge cases you considered.
```

**Avoid:**
```
❌ "This is wrong."
❌ "Why did you do it this way?"
❌ "I would never write code like this."
```

**Instead:**
```
✅ "This approach might have issues with X. Consider Y instead because Z."
✅ "What was the reasoning for this approach? I'm wondering if X might be simpler."
✅ "I suggest refactoring this because it improves maintainability."
```

### Review Response Time

- **Small PRs (< 50 lines)**: Review within 4 hours
- **Medium PRs (50-200 lines)**: Review within 24 hours
- **Large PRs (200-400 lines)**: Review within 48 hours
- **Huge PRs (> 400 lines)**: Ask author to split into smaller PRs

If you can't review within these timeframes, comment to let the author know when you'll be able to review.

## Approval Guidelines

### When to Approve
- All blockers are resolved
- You understand the change
- You'd be comfortable maintaining this code
- Tests are adequate
- No obvious bugs or security issues

### When to Request Changes
- There are clear bugs
- Security vulnerabilities exist
- Tests are missing or inadequate
- Code doesn't follow our standards
- Breaking changes aren't documented

### When to Comment Without Blocking
- You have suggestions but nothing critical
- You want to share knowledge
- You're asking questions for your own understanding
- You want to praise good work

## Special Cases

### Hot Fixes
- Smaller review scope focused on the critical fix
- Can bypass some standard checks if urgency requires
- Must create follow-up ticket for proper fix/tests
- Requires explicit "HOTFIX" label

### Documentation-Only Changes
- Review for accuracy and clarity
- Check for broken links
- Verify examples work
- Faster turnaround acceptable

### Refactoring PRs
- Verify behavior hasn't changed
- Check that tests still pass
- Ensure improved readability/maintainability
- May be larger than typical PRs

## Common Pitfalls to Avoid

### For Authors
- ❌ Taking feedback personally
- ❌ Defensive responses to suggestions
- ❌ Creating huge PRs (split them up!)
- ❌ Not explaining complex logic
- ❌ Ignoring reviewer questions

### For Reviewers
- ❌ Being overly critical or pedantic
- ❌ Nitpicking style issues (use automated linters instead)
- ❌ Rubber-stamping without thorough review
- ❌ Letting PRs sit without feedback
- ❌ Not explaining the "why" behind suggestions

## Metrics We Track

To ensure healthy review practices:
- Average time to first review
- Average time to merge
- Number of review cycles per PR
- PR size distribution

**Goals:**
- 80% of PRs reviewed within 24 hours
- Average PR size < 250 lines
- 90% of PRs merged within 3 days

## Emergency Escalation

If a PR is blocked and discussions aren't progressing:
1. Schedule a synchronous call to discuss
2. Escalate to tech lead if needed
3. Document the decision and rationale

## Tools

- **GitHub PR reviews**: Primary review platform
- **Conventional Comments**: Use labels (BLOCKER, SUGGESTION, etc.)
- **GitHub Discussions**: For architectural debates
- **Slack #engineering**: For time-sensitive review requests

## Resources

- Writing small PRs: Internal wiki link
- Effective code review comments: Internal wiki link
- How to handle disagreements: Internal wiki link

---

**Remember**: Code reviews are one of the most valuable practices for code quality and team growth. Treat them as a priority, be respectful, and focus on continuous improvement.
