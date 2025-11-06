# Engineering Onboarding Guide

## Purpose
Welcome to the Datacode.app engineering team! This guide provides a structured path through your first 90 days, helping you navigate our engineering guidelines and become a productive, confident team member.

---

## 🎯 Onboarding Philosophy

**Learn by Doing**: We balance reading with hands-on work from day one.

**Progressive Disclosure**: Don't try to read everything at once. Focus on what you need when you need it.

**Buddy System**: You'll be paired with an experienced engineer who will guide you.

**Questions Are Expected**: We'd rather answer 100 questions than have you struggle in silence.

---

## 📅 Your First 90 Days

### Week 1: Foundation & Environment Setup

#### Day 1: Welcome & Setup
- [ ] **Morning: Admin & Setup**
  - Meet your team and manager
  - Get access to tools: GitHub, Slack, email, VPN
  - Set up your development environment
  - Clone main repositories

- [ ] **Afternoon: First Code**
  - Read [Quick Reference Guide](quick-reference.md) - Bookmark it!
  - Read [Engineering Principles](engineering-principles.md) - Understand our WHY
  - Make your first commit: Add yourself to team.md
  - Create your first PR (even if tiny!)

**Goal**: End of day 1, you've committed code and created a PR.

#### Day 2-3: Git & Code Style
- [ ] Read [Git Workflow Guide](git-workflow-guide.md)
  - Practice: Create a feature branch, commit, push, create PR
- [ ] Read [Code Style Guide](code-style-guide.md)
  - Set up ESLint and Prettier in your editor
  - Practice: Fix some linting issues in a practice repo
- [ ] Read [Code Review Guide](code-review-guide.md)
  - Shadow a code review with your buddy
  - Review a simple PR yourself

**Hands-On Task**: Fix a "good first issue" bug (your buddy will assign one)

#### Day 4-5: Testing & Quality
- [ ] Read [Testing Standards](testing-standards.md) - Focus on:
  - Test structure (AAA pattern)
  - How to run tests locally
  - Writing your first test
- [ ] Read [Definition of Done](definition-of-done.md)
  - Use this checklist for all your work

**Hands-On Task**: Add tests to your bug fix from days 2-3

**End of Week 1 Checkpoint**:
- ✅ Development environment set up
- ✅ First PR merged
- ✅ Understand basic Git workflow
- ✅ Can run and write tests
- ✅ Know how to get code reviewed

---

### Week 2: Security & APIs

#### Day 6-7: Security Fundamentals
- [ ] Read [Security Best Practices](security-best-practices.md) - Focus on:
  - Authentication & Authorization
  - Input Validation
  - Secrets Management
  - Common vulnerabilities (SQL injection, XSS)
- [ ] Complete security training module (if available)

**Hands-On Task**: Security audit a small feature (with your buddy)

#### Day 8-10: API Development
- [ ] Read [API Design Standards](api-design-standards.md) - Focus on:
  - REST fundamentals
  - Request/response format
  - Error handling
  - Authentication
- [ ] Study 2-3 existing API endpoints in our codebase
- [ ] Understand our API documentation (OpenAPI/Swagger)

**Hands-On Task**: Create or modify a simple API endpoint

**End of Week 2 Checkpoint**:
- ✅ Understand security basics
- ✅ Can identify common vulnerabilities
- ✅ Know our API conventions
- ✅ Can create/modify API endpoints

---

### Week 3-4: Real Work Begins

#### Week 3: First Feature
- [ ] Read [Technical Design Document Template](technical-design-doc-template.md)
  - Look at 1-2 recent design docs in the repo
- [ ] Get assigned your first feature (small but meaningful)
- [ ] Write a mini design doc (with guidance from your buddy)
- [ ] Implement the feature following all guidelines

**Daily Activities**:
- Stand-ups with your team
- Pair programming sessions
- Code reviews (give and receive feedback)

#### Week 4: Refinement & Learning
- [ ] Finish and deploy your first feature
- [ ] Read [Daily Report Guide](daily-report-guide.md)
  - Start sending daily updates to your manager
- [ ] Shadow an incident response (if one occurs)
- [ ] Quick skim of [Incident Response Guide](incident-response-guide.md)

**End of Month 1 Checkpoint**:
- ✅ First feature shipped to production
- ✅ Comfortable with core workflow
- ✅ Participating in code reviews
- ✅ Know who to ask for help

---

### Month 2: Depth & Breadth

#### Week 5-6: Expanding Scope
- [ ] Take on a medium-sized feature/project
- [ ] Lead a design discussion
- [ ] Review code from multiple team members
- [ ] Start exploring areas beyond your immediate team

**Learning Goals**:
- Understand our architecture (high-level)
- Know where different services/components live
- Understand deployment process
- Learn about monitoring and observability

#### Week 7-8: Specialization Begins
- [ ] Choose an area to dive deeper:
  - Frontend architecture
  - Backend systems
  - Database optimization
  - DevOps/Infrastructure
  - Security
- [ ] Read relevant advanced documentation
- [ ] Pair with an expert in that area
- [ ] Contribute improvements to that area

**End of Month 2 Checkpoint**:
- ✅ Shipped 2+ features independently
- ✅ Providing valuable code reviews
- ✅ Have a developing specialty
- ✅ Contributing to team discussions

---

### Month 3: Independence & Impact

#### Week 9-10: Full Autonomy
- [ ] Own a feature from design to deployment
- [ ] Write your own design doc
- [ ] Present in team design review
- [ ] Monitor your feature in production

**Stretch Goals**:
- Mentor a newer engineer
- Improve our documentation
- Fix technical debt
- Optimize something slow

#### Week 11-12: Integration
- [ ] Participate in on-call rotation (shadowing first)
  - Read [Incident Response Guide](incident-response-guide.md) thoroughly
  - Learn our runbooks
  - Practice incident response scenarios
- [ ] Contribute to engineering initiatives:
  - Improve a guideline document
  - Add to our test suite
  - Improve CI/CD
  - Optimize performance

**End of Month 3 Checkpoint**:
- ✅ Fully autonomous contributor
- ✅ Owning features end-to-end
- ✅ Ready for on-call
- ✅ Contributing beyond your immediate work

---

## 📚 Progressive Reading Plan

### Priority 1: Read First (Days 1-5)
1. **[Quick Reference](quick-reference.md)** - Your daily cheat sheet
2. **[Engineering Principles](engineering-principles.md)** - Our WHY
3. **[Git Workflow Guide](git-workflow-guide.md)** - Essential for day-to-day
4. **[Code Style Guide](code-style-guide.md)** - Write consistent code
5. **[Code Review Guide](code-review-guide.md)** - Give and receive feedback

### Priority 2: Read Soon (Weeks 2-3)
6. **[Testing Standards](testing-standards.md)** - Quality assurance
7. **[Definition of Done](definition-of-done.md)** - What "done" means
8. **[Security Best Practices](security-best-practices.md)** - Build securely
9. **[API Design Standards](api-design-standards.md)** - API conventions

### Priority 3: Reference As Needed (Weeks 4+)
10. **[Technical Design Template](technical-design-doc-template.md)** - For larger features
11. **[Incident Response Guide](incident-response-guide.md)** - Before on-call
12. **[Daily Report Guide](daily-report-guide.md)** - Communication format

---

## 🎓 Learning Resources by Role

### Frontend Engineers
**Focus Areas**:
- Code Style Guide (React/JSX section)
- Component testing in Testing Standards
- Frontend security (XSS, CSRF)
- UI/UX sections in Definition of Done

**First Tasks**:
1. Build a small component
2. Add tests for the component
3. Review another frontend PR
4. Implement a design system component

### Backend Engineers
**Focus Areas**:
- API Design Standards
- Database sections in Code Style
- Integration testing in Testing Standards
- Security Best Practices (auth, validation)

**First Tasks**:
1. Create a simple CRUD endpoint
2. Write integration tests
3. Add input validation
4. Optimize a slow query

### Full-Stack Engineers
**Focus Areas**:
- All frontend + backend materials
- End-to-end testing
- Complete feature ownership

**First Tasks**:
1. Implement a feature touching UI + API + DB
2. Write unit, integration, and E2E tests
3. Deploy to production
4. Monitor and iterate

### DevOps/SRE Engineers
**Focus Areas**:
- Incident Response Guide
- Security Best Practices (infrastructure)
- CI/CD sections across guides
- Monitoring and observability

**First Tasks**:
1. Improve a CI/CD pipeline
2. Add monitoring/alerting
3. Create a runbook
4. Participate in incident response

---

## 🤝 Your Support Network

### Your Onboarding Buddy
**Role**: Day-to-day guide and first point of contact
- **When to Ask**: Any question, big or small
- **Cadence**: Daily check-ins for first 2 weeks, then as needed

### Your Manager
**Role**: Career development, bigger picture
- **When to Ask**: Career questions, feedback, roadblocks
- **Cadence**: Weekly 1:1s

### Team Lead
**Role**: Technical guidance, architecture decisions
- **When to Ask**: Complex technical questions, design reviews
- **Cadence**: As needed, plus design reviews

### The Team
**Role**: Collective knowledge and support
- **Where**: #engineering Slack channel
- **Don't Be Shy**: Everyone was new once!

---

## ✅ Self-Assessment Checklist

### After 30 Days, Can You...
- [ ] Create a feature branch and PR independently?
- [ ] Write code that passes our linters?
- [ ] Write basic unit tests?
- [ ] Navigate the codebase to find relevant code?
- [ ] Provide helpful code review feedback?
- [ ] Deploy code to staging?
- [ ] Explain what your team works on?

**If No to any**: Talk to your buddy or manager. We'll help!

### After 60 Days, Can You...
- [ ] Design a small feature independently?
- [ ] Write comprehensive tests (unit, integration)?
- [ ] Review PRs from any team member?
- [ ] Debug production issues?
- [ ] Identify security vulnerabilities?
- [ ] Estimate how long tasks will take?
- [ ] Explain our architecture to someone new?

### After 90 Days, Can You...
- [ ] Own a feature from design to deployment?
- [ ] Write a technical design document?
- [ ] Mentor a newer engineer?
- [ ] Participate in on-call rotation?
- [ ] Contribute to engineering initiatives?
- [ ] Advocate for technical improvements?
- [ ] Feel like a full member of the team?

**Goal**: Yes to most of these by day 90!

---

## 🎯 Onboarding Tasks Tracker

### Required Tasks (Everyone)
- [ ] Set up development environment
- [ ] Merge first PR (day 1)
- [ ] Ship first feature (week 3-4)
- [ ] Complete security training
- [ ] Shadow 5+ code reviews
- [ ] Conduct 5+ code reviews
- [ ] Write a design doc (even if small)
- [ ] Deploy to production
- [ ] Shadow incident response
- [ ] Complete 30/60/90 self-assessments

### Bonus Tasks (Optional)
- [ ] Fix technical debt
- [ ] Improve documentation
- [ ] Add to test coverage
- [ ] Optimize something
- [ ] Give a team presentation
- [ ] Contribute to open source tools we use
- [ ] Write a blog post about what you learned

---

## 💬 Common Questions from New Engineers

### "There's so much to read. Where do I start?"
Start with [Quick Reference](quick-reference.md). Then follow the Priority 1 list above. Don't try to read everything at once.

### "I don't understand something in the codebase."
Ask! Use #engineering Slack or ask your buddy. Document what you learn.

### "I'm stuck on a problem."
Try for 30 minutes, then ask. We value collaboration over struggling in silence.

### "Am I asking too many questions?"
No. Seriously, no. We'd rather answer questions than have you make assumptions.

### "I made a mistake."
Everyone does. Be honest about it, learn from it, help us prevent it systematically.

### "I have an idea to improve something."
Great! Share it. Write a proposal. We love when new people bring fresh perspectives.

### "I feel overwhelmed."
Normal! Talk to your manager. We can adjust the pace. Your wellbeing matters.

### "How am I doing?"
Ask! Request feedback from your buddy, manager, and team members regularly.

---

## 🚀 Beyond Day 90

### Continuing Your Growth
- **Specialize**: Deep expertise in one area
- **Broaden**: Basic competence across many areas
- **Lead**: Mentor others, lead projects
- **Innovate**: Improve our processes and tools
- **Share**: Write, present, teach

### Career Development
- Regular 1:1s with manager
- Quarterly goal setting
- Annual performance reviews
- Engineering levels and promotion criteria
- Conference attendance and speaking
- Internal tech talks

---

## 📊 Success Metrics

We measure onboarding success by:

**Quantitative**:
- Time to first PR (target: Day 1)
- Time to first feature deployed (target: 30 days)
- Number of PRs reviewed (target: 10+ by day 90)
- Code review quality scores
- Test coverage contributions

**Qualitative**:
- Self-assessment scores
- Peer feedback
- Manager assessment
- Buddy feedback
- Cultural fit and team integration

---

## 🔄 Feedback Loop

### Help Us Improve Onboarding
At 30, 60, and 90 days, you'll receive a survey:
- What was helpful?
- What was confusing?
- What was missing?
- What should we change?

**Your feedback shapes the experience for future engineers.**

---

## 🎉 Welcome to the Team!

Remember:
- **Everyone starts somewhere** - Even our most senior engineers were new once
- **Questions are encouraged** - They make you smarter and our docs better
- **Mistakes are learning** - Fail fast, learn faster
- **You belong here** - We hired you because we believe in you
- **Have fun** - Build cool things with great people

**We're excited to have you on the team!**

---

## 📞 Quick Reference Contacts

```
General Questions:        #engineering Slack
Your Buddy:              [Assigned on day 1]
Your Manager:            [Assigned before day 1]
Tech Lead:               [Team-specific]
HR/People Ops:           people@datacode.app
IT Support:              #it-support Slack
Security Questions:      security@datacode.app
Anonymous Feedback:      [Feedback form link]
```

---

**This guide is a living document. If you find gaps or have suggestions, please open a PR!**

**Last Updated**: 2024-03-16
**Maintained by**: Engineering Leadership
**Questions?** Ask in #engineering or email hooshyar@datacode.app
