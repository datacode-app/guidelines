# Guidelines Governance

## Purpose
Great guidelines become outdated without proper maintenance. This document establishes how we create, maintain, evolve, and enforce our engineering guidelines to ensure they remain relevant, useful, and adopted.

---

## Core Principles

1. **Living Documents** - Guidelines evolve with our practices
2. **Community Owned** - Everyone can contribute
3. **Data-Driven** - Changes based on evidence, not opinion
4. **Lightweight Process** - Governance shouldn't be bureaucratic
5. **Practical Value** - If nobody uses it, it doesn't matter

---

## Governance Model

### Roles & Responsibilities

#### Document Owners (DRIs)

Each guideline has a **Directly Responsible Individual** who:
- Ensures accuracy and relevance
- Reviews and merges proposed changes
- Quarterly review and updates
- Responds to questions and issues

**Current Ownership**:

| Document | Owner | Backup |
|----------|-------|--------|
| Engineering Principles | CTO | Engineering Manager |
| Code Style Guide | Frontend Lead | Backend Lead |
| API Design Standards | Backend Lead | API Team Lead |
| Git Workflow Guide | Senior Engineer | DevOps Lead |
| Testing Standards | QA Lead | Senior Engineer |
| Security Best Practices | Security Lead | CTO |
| Code Review Guide | Engineering Manager | Tech Lead |
| Definition of Done | Product Eng Lead | Engineering Manager |
| Incident Response | On-Call Lead | SRE Lead |
| Technical Design Template | CTO | Engineering Manager |
| Onboarding Guide | Engineering Manager | People Ops |
| Quick Reference | Community Maintained | Engineering Manager |
| Daily Report Guide | Engineering Manager | CTO |
| Guidelines Governance | CTO | Engineering Manager |

#### Engineering Leadership

**Responsibilities**:
- Approve major changes to guidelines
- Resolve disputes
- Ensure guidelines align with company strategy
- Allocate resources for maintenance
- Champion adoption

#### All Engineers

**Rights**:
- Propose changes to any guideline
- Request new guidelines
- Question existing practices
- Provide feedback

**Responsibilities**:
- Follow adopted guidelines
- Provide constructive feedback
- Participate in reviews
- Help onboard new team members

---

## Guideline Lifecycle

### 1. Creation

#### When to Create a New Guideline

✅ **Create when**:
- Pattern used repeatedly across teams
- Recurring questions or confusion
- Quality or security issue affecting multiple projects
- Onboarding gap identified
- Compliance requirement

❌ **Don't create when**:
- One-time occurrence
- Team-specific practice
- Personal preference
- Solution in search of problem

#### Creation Process

```
1. Identify Need
   ↓
2. Draft Proposal (1-2 pages)
   - Problem statement
   - Proposed solution
   - Alternatives considered
   - Expected impact
   ↓
3. Share in #engineering for feedback (1 week)
   ↓
4. Present in Engineering All-Hands
   ↓
5. Incorporate feedback
   ↓
6. Get Engineering Leadership approval
   ↓
7. Create full guideline
   ↓
8. Announce and educate team
```

**Timeline**: 2-4 weeks from proposal to adoption

### 2. Maintenance

#### Quarterly Review Process

Every guideline undergoes quarterly review:

**Month 1 (Jan, Apr, Jul, Oct)**:
- [ ] Document owner reviews guideline
- [ ] Checks for outdated information
- [ ] Reviews open issues/questions
- [ ] Gathers feedback from team
- [ ] Proposes updates if needed

**Month 2**:
- [ ] Changes reviewed by Engineering Leadership
- [ ] Discussion in Engineering All-Hands
- [ ] Finalize and merge updates
- [ ] Communicate changes

**Month 3**:
- [ ] Monitor adoption of changes
- [ ] Gather feedback
- [ ] Document lessons learned

#### Continuous Maintenance

Between quarterly reviews:
- **Minor updates** (typos, clarifications): Can be merged anytime by DRI
- **Additions** (new examples, tips): Reviewed and merged within 1 week
- **Changes** (modifying standards): Follow change process below

### 3. Evolution

#### Change Process

**Minor Changes** (examples, clarifications, formatting):
```
1. Open PR with changes
   ↓
2. DRI reviews within 3 days
   ↓
3. Merge and announce in #engineering
```

**Major Changes** (standards, requirements, processes):
```
1. Open RFC (Request for Comments) as GitHub Issue
   ↓
2. Discuss in #engineering (2 weeks minimum)
   ↓
3. Present in Engineering All-Hands
   ↓
4. Engineering Leadership approval required
   ↓
5. Open PR with changes
   ↓
6. Announce with migration guide
   ↓
7. 30-day transition period
```

#### Versioning

Guidelines use **semantic versioning**:

```
v2.1.3
│ │ └─ Patch: Typos, clarifications, examples
│ └─── Minor: Additions, non-breaking changes
└───── Major: Breaking changes, new requirements
```

**Example**:
- `v1.0.0` → Initial release
- `v1.1.0` → Added React hooks examples (minor)
- `v1.1.1` → Fixed typo in example (patch)
- `v2.0.0` → Changed required test coverage from 70% to 80% (major)

### 4. Deprecation

#### When to Deprecate

- Guideline no longer relevant
- Superseded by better approach
- Tool/technology retired
- Proven ineffective

#### Deprecation Process

```
1. Mark guideline as [DEPRECATED] in title
   ↓
2. Add deprecation notice at top:
   - Why deprecated
   - What replaces it
   - Sunset date (minimum 6 months)
   ↓
3. Announce in #engineering and All-Hands
   ↓
4. Monitor usage (6 months)
   ↓
5. After sunset date, move to archive/
   ↓
6. Keep archived for historical reference
```

---

## Enforcement

### Levels of Enforcement

#### Level 1: Recommendations (Soft)
- Best practices and suggestions
- "Should" language
- No blocking, but discussed in code review
- **Example**: "Code should be DRY"

#### Level 2: Standards (Medium)
- Team-wide conventions
- "Must" language for new code
- Existing code can be updated gradually
- **Example**: "API endpoints must be versioned"

#### Level 3: Requirements (Hard)
- Critical quality/security gates
- "Must" language, no exceptions
- Enforced by automated tools
- Blocks merge if violated
- **Example**: "All code must pass security scan"

### Automated Enforcement

Where possible, use automation:

```javascript
// .github/workflows/guidelines-check.yml
name: Guidelines Check

on: [pull_request]

jobs:
  enforce:
    runs-on: ubuntu-latest
    steps:
      # Lint (Code Style)
      - name: ESLint
        run: npm run lint

      # Tests (Testing Standards)
      - name: Test Coverage
        run: npm run test:coverage
        if: coverage < 80%, fail

      # Security (Security Best Practices)
      - name: Security Audit
        run: npm audit --audit-level=high

      # Commit Messages (Git Workflow)
      - name: Validate Commit Messages
        run: npx commitlint --from HEAD~1
```

### Manual Enforcement

#### In Code Review
Reviewers check:
- [ ] Follows Code Style Guide
- [ ] Meets Definition of Done
- [ ] Has adequate tests per Testing Standards
- [ ] Follows API Design Standards (if API change)
- [ ] No security issues per Security Best Practices

#### Exceptions

Exceptions to guidelines require:
1. Clear justification in PR description
2. Approval from guideline DRI or Engineering Leadership
3. TODO ticket created to address later (if technical debt)

**Example**:
```markdown
## Exception Request

**Guideline**: Testing Standards require 80% coverage
**Exception**: This PR has 65% coverage
**Justification**: Legacy code being refactored; adding tests would require
rewriting entire module. Follow-up ticket TECH-456 created to add
comprehensive tests in Q2.
**Approved by**: @qa-lead
```

---

## Measuring Success

### Guideline Metrics

We track:

| Metric | Target | How Measured |
|--------|--------|--------------|
| **Adoption Rate** | >90% of PRs comply | Automated checks + random audits |
| **Time Saved** | Reduced review time | PR review duration metrics |
| **Onboarding Time** | 30% faster | Time to first productive PR |
| **Question Volume** | Decreasing | #engineering question count |
| **Doc Usefulness** | >4/5 rating | Quarterly survey |
| **Update Frequency** | Quarterly minimum | Commits to guidelines repo |

### Quarterly Survey

We ask the team:

1. **Usefulness**: How useful are our guidelines? (1-5)
2. **Clarity**: How clear are they? (1-5)
3. **Adoption**: Do you follow them? (Yes/Mostly/Sometimes/No)
4. **Gaps**: What's missing?
5. **Improvements**: What should change?

### Success Indicators

✅ **Guidelines are working when**:
- PRs move faster through review
- Fewer bugs reach production
- New engineers onboard quicker
- Code becomes more consistent
- Team reports less confusion

❌ **Guidelines are failing when**:
- Frequently ignored or bypassed
- Require constant exceptions
- Source of frustration
- Out of sync with reality
- Nobody reads them

---

## Communication & Training

### Announcing Changes

**Major Changes** (v2.0.0):
- Engineering All-Hands presentation
- Email to entire engineering team
- #engineering Slack announcement
- Update in weekly team meetings
- Migration guide published
- 30-day transition period

**Minor Changes** (v1.1.0):
- #engineering Slack announcement
- Added to weekly engineering newsletter
- Mentioned in relevant PRs

**Patches** (v1.0.1):
- Git commit message
- Mentioned in next All-Hands

### Training

#### For New Guidelines
- Engineering All-Hands presentation
- Office hours for Q&A
- Example PRs demonstrating new standard
- Update onboarding materials

#### For Existing Guidelines
- Quarterly refresher in All-Hands
- Included in onboarding
- "Lunch & Learn" sessions
- Pair programming to reinforce

#### For New Engineers
- Follow [Onboarding Guide](onboarding-guide.md)
- Buddy reviews guidelines with them
- Quiz/assessment at 30 days (optional)

---

## Feedback Mechanisms

### How to Provide Feedback

#### Quick Questions
- Ask in #engineering Slack
- Tag the guideline DRI
- Response within 1 business day

#### Suggestions
- Open GitHub Issue
- Use template: "Guideline Improvement Suggestion"
- Discuss in issue comments
- May become RFC for major changes

#### Formal Proposals
- Open RFC (Request for Comments)
- Present in Engineering All-Hands
- Gather feedback for 2 weeks
- Engineering Leadership decides

### Feedback Channels

| Channel | Use For | Response Time |
|---------|---------|---------------|
| #engineering Slack | Questions, quick feedback | 1 business day |
| GitHub Issues | Suggestions, bug reports | 1 week |
| Engineering All-Hands | Major proposals, discussions | Discussed at next meeting |
| Quarterly Survey | General feedback | Reviewed quarterly |
| 1:1s with Manager | Concerns, frustrations | Immediate |

---

## Guideline Health Checklist

Each quarter, ask:

**Relevance**:
- [ ] Is this still relevant to our work?
- [ ] Does it reflect current practices?
- [ ] Are the examples up to date?

**Clarity**:
- [ ] Can a new engineer understand this?
- [ ] Are there confusing sections?
- [ ] Do we get repeated questions about this?

**Adoption**:
- [ ] Are people following this?
- [ ] If not, why not?
- [ ] Does it need better enforcement?

**Completeness**:
- [ ] Are there gaps or missing sections?
- [ ] Have new patterns emerged?
- [ ] Do we need new examples?

**Impact**:
- [ ] Is this making us better?
- [ ] Can we measure the improvement?
- [ ] Is the effort worth the benefit?

---

## Tools & Infrastructure

### Guidelines Repository

```
guidelines/
├── README.md                      # Index
├── *.md                           # Individual guidelines
├── .github/
│   └── ISSUE_TEMPLATE/
│       ├── guideline-question.md
│       ├── guideline-suggestion.md
│       └── guideline-rfc.md
├── archive/                       # Deprecated guidelines
├── examples/                      # Code examples
│   ├── good/
│   └── bad/
└── CHANGELOG.md                   # Version history
```

### Automation

- **Linting**: ESLint, Prettier (Code Style)
- **Testing**: Coverage checks (Testing Standards)
- **Security**: npm audit, Snyk (Security Best Practices)
- **Commit Messages**: commitlint (Git Workflow)
- **PR Checks**: GitHub Actions enforce standards
- **Metrics**: Track compliance automatically

### Documentation Platform

- **Primary**: GitHub (source of truth)
- **Internal Wiki**: Links to GitHub docs
- **Search**: GitHub search + internal search
- **Accessibility**: Public repo, anyone can read

---

## Decision-Making Framework

### When to Update a Guideline

Use this decision tree:

```
Is this a recurring pattern/issue?
├─ No → Don't update guideline
└─ Yes → Does this affect multiple teams?
    ├─ No → Make team-specific practice
    └─ Yes → Is there team consensus?
        ├─ No → Discuss in All-Hands, build consensus
        └─ Yes → Is it measurably better?
            ├─ Unclear → Run experiment, measure
            └─ Yes → Update guideline
```

### Resolving Disputes

If team disagrees on guideline change:

1. **Data First**: What does evidence show?
2. **Experiment**: Try both approaches, measure outcomes
3. **External Research**: What do industry leaders do?
4. **Escalate**: Engineering Leadership decides
5. **Document**: Record decision and rationale

**Decision is Final**: Once decided, team aligns and implements.

---

## Continuous Improvement

### Retrospectives

After major guideline rollouts:
- What went well?
- What could improve?
- What did we learn?
- Update process based on learnings

### Experiments

To test new practices:
1. One team pilots for 1 sprint
2. Measure outcomes (velocity, quality, satisfaction)
3. Gather feedback
4. Decide: adopt, modify, or abandon

### External Inspiration

We monitor:
- Industry best practices
- Conference talks
- Engineering blogs
- Open source projects
- Competitor practices

**But**: We adapt, not copy. What works elsewhere may not work here.

---

## Getting Started with Governance

### For Guideline Owners

1. Read this governance doc
2. Review your assigned guideline
3. Set calendar reminder for quarterly review
4. Join #guidelines-owners Slack channel
5. Respond to issues/questions about your guideline

### For All Engineers

1. Bookmark the [guidelines repo](link)
2. Follow guidelines in your daily work
3. Provide feedback when something's unclear
4. Propose improvements
5. Help onboard new engineers

### For Leadership

1. Champion guidelines adoption
2. Review and approve major changes
3. Allocate time for guideline maintenance
4. Model adherence to guidelines
5. Celebrate improvements

---

## FAQ

### Why do we need governance for documentation?

Without governance, guidelines become:
- Outdated → People stop trusting them
- Inconsistent → Confusion and frustration
- Ignored → Waste of effort to create them
- Stale → No one maintains them

**Governance keeps guidelines valuable.**

### Isn't this bureaucratic?

It can be if done wrong. Our governance is **lightweight**:
- Minor changes: Just open a PR
- Major changes: RFC + discussion
- Quarterly reviews: Scheduled, predictable
- Clear ownership: Know who to ask

### What if I disagree with a guideline?

Great! We want healthy debate:
1. Open an issue explaining why
2. Propose alternative with evidence
3. Discuss with team
4. We'll experiment and measure
5. Update based on data

### Can I bypass a guideline?

For good reason, yes:
- Document why in PR
- Get approval from DRI
- Create follow-up ticket if needed

Without good reason, no:
- Reviewers will ask for compliance
- Automated checks may block merge

### How much time does this take?

**For DRIs**: ~2 hours per quarter
**For engineers**: ~30 min per month (reading updates)
**For leadership**: ~1 hour per quarter

**ROI**: Huge. Time saved in code review, onboarding, and bug fixes far exceeds this investment.

---

## Success Stories

(To be filled in as we adopt this governance model)

- **Example**: After adopting API Design Standards, API review time decreased by 40%
- **Example**: Security Best Practices caught 15 vulnerabilities before production
- **Example**: New engineer onboarding time decreased from 60 to 45 days

---

## Appendix: Templates

### RFC Template

```markdown
# RFC: [Title]

**Author**: [Your Name]
**Date**: YYYY-MM-DD
**Status**: Draft | In Discussion | Accepted | Rejected

## Problem Statement
[What problem are we solving?]

## Proposed Solution
[What's the proposed guideline or change?]

## Alternatives Considered
[What other options did we consider?]

## Impact
[Who does this affect? How?]

## Implementation
[How will we roll this out?]

## Metrics
[How will we measure success?]

## Open Questions
[What needs to be resolved?]

## Discussion
[Link to GitHub Issue or Slack thread]
```

---

**This governance model ensures our guidelines remain living, useful, and adopted.**

**Questions?** Ask in #engineering or contact the CTO.

**Last Updated**: 2024-03-16
**Next Review**: Q2 2024
**Owner**: CTO
**Contributors**: Engineering Leadership Team
