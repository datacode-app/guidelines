# Definition of Done

## Purpose
The Definition of Done (DoD) is a shared agreement on what it means for work to be "complete". This ensures quality, consistency, and prevents technical debt from accumulating.

## Core Principle
**A task is only done when it's ready for production deployment without any additional work.**

---

## General Definition of Done

All work items must meet these criteria before being marked as complete:

### Code Quality
- [ ] **Code is written and readable**
  - Follows our coding style guide
  - Variable/function names are clear and descriptive
  - Complex logic is commented
  - No unnecessary comments (code should be self-documenting where possible)

- [ ] **Code is DRY (Don't Repeat Yourself)**
  - No obvious duplication
  - Shared logic extracted into reusable functions/components
  - Constants defined and reused

- [ ] **Code is reviewed and approved**
  - PR created with clear description
  - At least one approval from team member
  - All review comments addressed or discussed
  - CI checks passing

### Testing
- [ ] **Unit tests written and passing**
  - New code has corresponding unit tests
  - Test coverage meets minimum threshold (80%+)
  - Tests are meaningful (not just for coverage)
  - Edge cases considered and tested

- [ ] **Integration tests added (if applicable)**
  - API endpoints tested
  - Component integration tested
  - Database queries tested

- [ ] **Manual testing completed**
  - Feature works as expected in dev environment
  - Edge cases manually verified
  - Different user roles tested (if applicable)
  - Mobile/responsive tested (if UI change)

- [ ] **Regression testing considered**
  - Related features still work
  - No unintended side effects

### Documentation
- [ ] **Code is documented**
  - Complex functions have JSDoc/docstrings
  - Public APIs documented
  - Non-obvious decisions explained in comments

- [ ] **User-facing documentation updated**
  - README updated if needed
  - API documentation updated
  - Changelog entry added
  - User guides updated (if applicable)

- [ ] **Technical documentation updated**
  - Architecture diagrams updated (if changed)
  - Confluence/Wiki updated (if applicable)
  - Technical decisions documented (ADR if significant)

### Security
- [ ] **Security considerations addressed**
  - No secrets/credentials in code
  - Input validation implemented
  - Authorization checks in place
  - SQL injection prevention (parameterized queries)
  - XSS prevention (sanitized inputs/outputs)
  - CORS configured properly

- [ ] **Dependencies reviewed**
  - No known security vulnerabilities in dependencies
  - `npm audit` or equivalent run and clear

### Performance
- [ ] **Performance impact considered**
  - No obvious performance bottlenecks
  - Database queries optimized (indexed, no N+1)
  - Large lists paginated
  - Heavy operations run async
  - Caching considered for expensive operations

### Deployment
- [ ] **Deployment requirements documented**
  - Environment variables documented
  - Database migrations included (if needed)
  - Deployment steps in PR description
  - Rollback plan considered for risky changes

- [ ] **Feature flags implemented (if appropriate)**
  - Large features behind feature flags
  - Gradual rollout possible

- [ ] **Monitoring and logging added**
  - Critical paths logged
  - Error tracking in place
  - Metrics/analytics added (if needed)

### Product Requirements
- [ ] **Acceptance criteria met**
  - All requirements from ticket/story satisfied
  - Product owner/stakeholder can verify
  - Demo-ready

- [ ] **UX/UI matches design**
  - Design mockups implemented accurately
  - Responsive behavior correct
  - Accessibility considerations met

---

## Feature-Specific Definitions of Done

### For New Features

- [ ] Feature flag implemented
- [ ] Analytics tracking added
- [ ] User documentation/help text written
- [ ] Onboarding/tooltips added (if needed)
- [ ] Product team has reviewed and signed off
- [ ] Demo prepared for team showcase

### For Bug Fixes

- [ ] Root cause identified and documented
- [ ] Fix verified in environment where bug occurred
- [ ] Test added to prevent regression
- [ ] Related bugs checked (same root cause?)
- [ ] Issue tracking ticket updated with resolution

### For Refactoring

- [ ] Behavior unchanged (proven by tests)
- [ ] All existing tests still pass
- [ ] Code complexity reduced (measurable)
- [ ] Team understands the new structure
- [ ] Migration path documented (if breaking change)

### For API Changes

- [ ] API versioning considered
- [ ] Backward compatibility maintained (or breaking change documented)
- [ ] API documentation updated (OpenAPI/Swagger)
- [ ] Client examples updated
- [ ] Rate limiting considered
- [ ] Error responses documented

### For Database Changes

- [ ] Migration script tested locally
- [ ] Migration is reversible (down migration)
- [ ] Performance impact on large datasets considered
- [ ] Indexes added for new queries
- [ ] Backup plan established
- [ ] Database documentation updated

### For UI/UX Changes

- [ ] Responsive design tested (mobile, tablet, desktop)
- [ ] Cross-browser tested (Chrome, Firefox, Safari)
- [ ] Accessibility tested (keyboard navigation, screen readers)
- [ ] Loading states implemented
- [ ] Error states implemented
- [ ] Empty states implemented
- [ ] Design team approved

### For Third-Party Integrations

- [ ] Error handling for API failures
- [ ] Timeouts configured
- [ ] Retry logic implemented
- [ ] Rate limiting respected
- [ ] Credentials stored securely
- [ ] Fallback behavior defined
- [ ] Monitoring/alerting configured

---

## Before Moving to "Done"

### Developer Checklist
```markdown
- [ ] All automated tests pass
- [ ] Code review approved
- [ ] Branch rebased on main
- [ ] All DoD items checked
- [ ] Tested in dev environment
- [ ] PR description complete
- [ ] Breaking changes documented
- [ ] No console errors/warnings
```

### Before Deployment Checklist
```markdown
- [ ] Staging environment tested
- [ ] Product team sign-off received
- [ ] Database migrations ready
- [ ] Environment variables configured
- [ ] Monitoring/alerts configured
- [ ] Rollback plan documented
- [ ] Team notified of deployment
```

---

## Quality Gates

Work cannot progress to the next stage without meeting DoD:

| Stage | Gate | Criteria |
|-------|------|----------|
| **Development → Code Review** | Developer checklist | All code written, self-reviewed, tests passing |
| **Code Review → QA** | Approval + CI green | At least 1 approval, all CI checks pass |
| **QA → Staging** | Testing complete | Manual testing done, bugs fixed |
| **Staging → Production** | Product sign-off | Product team approved, monitoring ready |

---

## Common Pitfalls

### ❌ What "Done" is NOT:
- "Code written but not tested"
- "Works on my machine"
- "Tests failing but I'll fix them later"
- "Documentation can wait"
- "I'll write tests in a follow-up PR"
- "Just needs a quick review"
- "95% done" (there's no such thing)

### ✅ What "Done" IS:
- Deployed to production and working
- Tested thoroughly
- Documented completely
- Reviewed and approved
- Monitored and measurable
- Meets all acceptance criteria

---

## Exceptions

Exceptions to DoD should be **rare** and **explicitly documented**.

### Valid Exceptions:
- **Spike/Proof of Concept**: Research work with no production intent
- **Hotfix**: Critical production issue (but must create follow-up ticket for full DoD)
- **Documentation-only**: Changes that don't affect code

### How to Handle Exceptions:
1. Get tech lead approval
2. Document reason in PR
3. Create follow-up tickets for skipped items
4. Track tech debt in dedicated backlog

---

## DoD for Different Work Types

### Documentation Updates
- [ ] Technical accuracy verified
- [ ] Examples tested
- [ ] Links work
- [ ] Spelling/grammar checked
- [ ] Reviewed by another team member

### Configuration Changes
- [ ] Tested in dev/staging
- [ ] Impact on production assessed
- [ ] Rollback procedure documented
- [ ] Team notified

### Dependency Updates
- [ ] Release notes reviewed
- [ ] Breaking changes identified
- [ ] All tests pass with new version
- [ ] No new security vulnerabilities

---

## Measuring DoD Compliance

We track these metrics:
- **Defect escape rate**: Bugs found in production (target: < 2%)
- **Rework rate**: PRs needing significant changes post-review (target: < 10%)
- **Test coverage**: Code covered by tests (target: > 80%)
- **Time to production**: Days from dev complete to deployed (target: < 2 days)

---

## Team Agreement

This Definition of Done is a **living document**. We review and update it quarterly based on:
- Lessons learned from production issues
- Team retrospective feedback
- Changing product requirements
- New tools and practices

**Last Updated**: [Date will be updated in commit]
**Next Review**: [Quarterly]

---

## Quick Reference Card

### The 5 Essential Checks
Before marking anything done, ask:

1. ✅ **Can I deploy this to production right now?**
2. ✅ **Would I be comfortable if someone else had to maintain this?**
3. ✅ **Have I tested all the ways this could break?**
4. ✅ **Can the next person understand what I did and why?**
5. ✅ **Does this meet all the acceptance criteria?**

If the answer to any is "no", it's not done.

---

**Remember**: Done means **production-ready**, not "ready for the next person to finish". Quality is everyone's responsibility.
