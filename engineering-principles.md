# Engineering Principles

## Purpose
These principles define WHO we are as engineers and WHY we work the way we do. They are the foundation beneath all our guidelines, the compass for our decisions, and the culture we build together.

---

## Our Core Belief

**Great software is built by great teams solving meaningful problems.**

We're not just writing code—we're building products that matter, creating value for users, and growing as engineers. Everything we do serves these goals.

---

## The Principles

### 1. 🎯 User Impact Over Technical Perfection

**What it means:**
The best code is code that solves real problems for real people. Technical elegance without user impact is just expensive art.

**In practice:**
- ✅ Ship working features over perfect architecture
- ✅ Iterate based on user feedback
- ✅ Choose boring, proven technology when it works
- ❌ Don't over-engineer for hypothetical future needs
- ❌ Don't let perfect be the enemy of good

**Example:**
```
Scenario: Should we rewrite this module with a cleaner architecture?

Questions to ask:
- Will users notice the difference?
- Is the current code causing user-facing bugs?
- Could we spend this time on new features instead?

If answers are "no, no, yes" → Don't rewrite.
```

**When to optimize:**
- Current code is causing user-facing issues
- Technical debt is slowing feature development
- Scalability problems affect user experience
- Security vulnerabilities exist

---

### 2. 🚀 Velocity Through Quality

**What it means:**
Going fast doesn't mean cutting corners. Tests, reviews, and good practices make us faster in the long run.

**In practice:**
- ✅ Write tests—they save time debugging later
- ✅ Review code carefully—catch bugs before production
- ✅ Refactor as you go—don't accumulate debt
- ✅ Automate repetitive tasks—free up time for creative work
- ❌ Don't skip quality to hit deadlines—you'll pay later

**Example:**
```
Apparent Speed:
  No tests, no reviews, ship quickly → 1 week to feature
  But: 2 weeks fixing production bugs
  Total: 3 weeks

Actual Speed:
  Write tests, code review, ship confidently → 1.5 weeks to feature
  But: 0.5 weeks fixing issues
  Total: 2 weeks
```

**The compound effect:**
Good practices slow you down 20% on each feature, but eliminate 80% of rework. Over time, quality makes you 2-3x faster.

---

### 3. 🤝 Team Success Over Individual Heroics

**What it means:**
We win as a team, not as individuals. The best engineer is the one who makes everyone around them better.

**In practice:**
- ✅ Share knowledge through docs and pair programming
- ✅ Review code thoughtfully and kindly
- ✅ Ask for help and offer help freely
- ✅ Celebrate team wins, learn from team losses
- ❌ Don't be a solo hero who doesn't collaborate
- ❌ Don't hoard knowledge or create silos

**Example:**
```
Individual Hero:
- Works alone on critical feature
- Only they understand the code
- Team depends on them
- They're overloaded, team is blocked
Result: Bus factor of 1, team can't scale

Team Player:
- Pairs with junior engineer on feature
- Documents decisions and architecture
- Shares knowledge in design reviews
- Others can maintain the code
Result: Knowledge shared, team grows stronger
```

**How we measure success:**
Not by individual commits, but by team velocity, quality, and growth.

---

### 4. 🔒 Security is Everyone's Job

**What it means:**
Security isn't just the security team's responsibility. Every engineer must think about security in everything they build.

**In practice:**
- ✅ Validate all inputs
- ✅ Use parameterized queries
- ✅ Never commit secrets
- ✅ Think like an attacker: "How could this be exploited?"
- ✅ Report vulnerabilities immediately
- ❌ Don't assume "someone else will catch it"

**Example:**
```
❌ Bad: "This is just internal, security doesn't matter"
✅ Good: "Even internal tools need auth and input validation"

❌ Bad: "I'll add security later"
✅ Good: "Security is built in from the start"

❌ Bad: "This probably won't be attacked"
✅ Good: "What if this were on the front page of HackerNews?"
```

**Security mindset:**
- Assume inputs are malicious
- Assume network is hostile
- Assume secrets will leak
- Build defenses in layers

---

### 5. 📊 Measure, Don't Guess

**What it means:**
Data beats opinions. Before optimizing, measure. After changing, measure. Always measure.

**In practice:**
- ✅ Add logging and metrics to new features
- ✅ Measure performance before optimizing
- ✅ A/B test significant changes
- ✅ Track the metrics that matter (user value, not vanity)
- ❌ Don't optimize prematurely
- ❌ Don't trust "it feels faster"

**Example:**
```
Without measurement:
"This function seems slow. Let me rewrite it."
→ Spend 2 days optimizing
→ Discover it runs 10 times per day
→ Saved 5ms × 10 = 50ms per day (irrelevant)

With measurement:
"Let me profile to find bottlenecks."
→ Discover a different function runs 10,000 times per day
→ Optimize that function instead
→ Save 100ms × 10,000 = 16 minutes per day (meaningful!)
```

**What to measure:**
- User-facing performance (page load, API response time)
- Business metrics (conversion, retention, revenue)
- Engineering metrics (deployment frequency, MTTR)
- NOT: Lines of code, commit count, hours worked

---

### 6. 🎓 Learn Continuously, Teach Generously

**What it means:**
Technology changes fast. We must learn constantly and share what we learn. Teaching others makes us better engineers.

**In practice:**
- ✅ Read code, docs, blogs, books
- ✅ Experiment with new technologies (on side projects)
- ✅ Write documentation and blog posts
- ✅ Mentor junior engineers
- ✅ Give tech talks and lead workshops
- ❌ Don't gate-keep knowledge
- ❌ Don't get comfortable and stop learning

**Example:**
```
Learning loop:
1. Encounter new problem/technology
2. Research and experiment
3. Apply to real work
4. Document what you learned
5. Share with team
6. Teach someone else

Result: You learn it deeply, team benefits, culture grows
```

**Growth mindset:**
- "I don't know yet" → Growth
- "That's not my job" → Fixed
- "Let me figure it out" → Growth
- "Someone else will do it" → Fixed

---

### 7. 🚨 Own Your Impact

**What it means:**
You're responsible for your code in production. If it breaks, you fix it. If users are confused, you improve it. Ownership means accountability.

**In practice:**
- ✅ Monitor your features in production
- ✅ Respond to alerts and incidents
- ✅ Fix bugs you introduced
- ✅ Improve based on user feedback
- ✅ Participate in on-call rotation
- ❌ Don't "throw code over the wall"
- ❌ Don't blame others for production issues

**Example:**
```
Without ownership:
"I deployed the feature. Now it's ops' problem."
→ Feature breaks in production
→ "Not my fault, works on my machine"
→ Ops scrambles to fix without context
Result: Slow resolution, blame culture

With ownership:
"I deployed and I'm monitoring it."
→ Feature breaks in production
→ "I see the alert, I'm on it"
→ Fix quickly with full context
Result: Fast resolution, learning, improvement
```

**You build it, you run it:**
- Participate in on-call
- Watch your metrics and logs
- Respond to user feedback
- Continuously improve

---

### 8. 💬 Communicate Early, Communicate Often

**What it means:**
Most problems stem from poor communication. Over-communicate on status, blockers, and decisions. Async-first, but synchronous when needed.

**In practice:**
- ✅ Send daily updates (even if "no progress, blocked on X")
- ✅ Raise blockers immediately
- ✅ Document decisions in PRs and design docs
- ✅ Ask questions in public channels (help future searchers)
- ❌ Don't work in silence
- ❌ Don't surprise people with big changes

**Example:**
```
Poor communication:
Day 1-10: "Working on feature" (no details)
Day 10: "Done!"
Team: "Wait, this isn't what we needed"
Result: Wasted 10 days

Good communication:
Day 1: "Starting feature, here's my plan"
Day 2: "Making progress, discovered X"
Day 3: "Blocked on Y, need help"
Day 5: "Approach changed based on Z"
Day 7: "Here's a demo, is this right?"
Day 10: "Done, exactly as expected"
Result: Team aligned, no surprises
```

**Communication guidelines:**
- **Status**: Daily (use daily report format)
- **Blockers**: Immediately (don't wait)
- **Decisions**: As they happen (in PRs/docs)
- **Questions**: When stuck for >30 min

---

### 9. 🔄 Iterate Fast, Fail Forward

**What it means:**
We learn fastest by shipping and iterating. Failure is data. Build, measure, learn, repeat.

**In practice:**
- ✅ Ship MVPs and iterate based on feedback
- ✅ Use feature flags for gradual rollouts
- ✅ Learn from failures and improve systems
- ✅ Experiment with new approaches
- ❌ Don't wait for perfection
- ❌ Don't fear failure—fear not learning from it

**Example:**
```
Waterfall approach:
- 6 months building perfect product
- Launch day: Users hate it
- 6 months wasted
Result: Slow feedback, big failure

Iterative approach:
- 2 weeks building MVP
- Launch to 10% of users
- Learn what works, what doesn't
- Iterate based on data
- Repeat every 2 weeks
Result: Fast learning, continuous improvement
```

**Fail forward:**
- Blameless post-mortems
- Document lessons learned
- Improve systems, not just fix symptoms
- Share learnings with team

---

### 10. 🎨 Simple > Clever

**What it means:**
Clever code is fun to write but painful to maintain. Simple, boring code is a gift to your future self and your teammates.

**In practice:**
- ✅ Choose readable over clever
- ✅ Prefer standard patterns over custom abstractions
- ✅ Write code that a junior engineer can understand
- ✅ Delete code whenever possible
- ❌ Don't show off with complex one-liners
- ❌ Don't create abstractions for one use case

**Example:**
```javascript
// ❌ Clever (hard to understand)
const t = d.filter(x=>x.a&&!x.d).map(x=>({...x,s:'active'}));

// ✅ Simple (easy to understand)
const activeUsers = users
  .filter(user => user.isActive && !user.isDeleted)
  .map(user => ({ ...user, status: 'active' }));
```

**Simplicity guidelines:**
- If you have to explain it, it's too complex
- If a junior engineer can't understand it, simplify it
- If you'll forget how it works in 6 months, simplify it
- "Any fool can write code that a computer can understand. Good programmers write code that humans can understand." - Martin Fowler

---

## Living These Principles

### In Daily Work

**Before starting a task:**
- [ ] How does this impact users? (Principle 1)
- [ ] What's the simplest approach? (Principle 10)
- [ ] Who needs to know about this? (Principle 8)

**While coding:**
- [ ] Am I writing tests? (Principle 2)
- [ ] Is this secure? (Principle 4)
- [ ] Will teammates understand this? (Principles 3, 10)

**Before shipping:**
- [ ] How will I measure success? (Principle 5)
- [ ] How will I monitor this? (Principle 7)
- [ ] Is this the MVP or am I over-building? (Principle 9)

### In Code Review

Look for:
- User impact and value (Principle 1)
- Tests and quality (Principle 2)
- Knowledge sharing and clarity (Principle 3)
- Security concerns (Principle 4)
- Unnecessary complexity (Principle 10)

### In Design Discussions

Ask:
- How will users benefit? (Principle 1)
- How will we measure success? (Principle 5)
- Can we start simpler and iterate? (Principles 9, 10)
- Who's the target user? (Principle 1)

### In Incidents

Remember:
- Own your impact (Principle 7)
- Communicate often (Principle 8)
- Learn and improve systems (Principle 9)
- Team success over blame (Principle 3)

---

## Cultural Expectations

### We Value

✅ **Collaboration** over competition
✅ **Learning** over knowing
✅ **Impact** over activity
✅ **Simplicity** over cleverness
✅ **Quality** over speed
✅ **Transparency** over secrecy

### We Don't Tolerate

❌ **Lone wolves** who don't share knowledge
❌ **Ego** that puts personal glory over team success
❌ **Blame** when things go wrong
❌ **Shortcuts** that compromise security
❌ **Silence** when blockers arise
❌ **Complexity** for its own sake

---

## When Principles Conflict

Sometimes principles seem to conflict. Here's how to navigate:

**Example: Velocity (2) vs. Simplicity (10)**
```
Situation: Should we use a complex library to ship faster?

Resolution:
- If it's a solved problem → Use the library (velocity wins)
- If it's core logic → Keep it simple (simplicity wins)
- If unsure → Start simple, add library if proven needed
```

**Example: User Impact (1) vs. Quality (2)**
```
Situation: Should we skip tests to ship faster?

Resolution:
- If emergency hotfix → Ship first, add tests immediately after
- If normal feature → Tests first (quality enables velocity)
- Remember: Quality is how we go fast sustainably
```

**When in doubt:**
1. Which choice serves users better long-term?
2. Which choice helps the team more?
3. Which choice is more reversible?

---

## Recognition & Rewards

We recognize engineers who:
- 🏆 Ship high-impact features
- 🏆 Improve team velocity through better practices
- 🏆 Mentor and grow other engineers
- 🏆 Catch critical bugs/security issues
- 🏆 Improve our processes and guidelines
- 🏆 Go above and beyond in incidents
- 🏆 Make tough problems look easy

---

## Questions to Challenge Yourself

### Daily
- Did my work today create user value?
- Did I help a teammate?
- Did I learn something new?

### Weekly
- Am I building the right thing?
- Is my code getting better or worse?
- Am I growing as an engineer?

### Quarterly
- What impact have I had?
- What have I learned?
- Who have I helped grow?
- What would I do differently?

---

## For New Engineers

You might feel overwhelmed by these principles. That's okay. Focus on:

**Month 1**: Principles 3, 8, 10
- Be a good teammate
- Communicate often
- Keep things simple

**Month 2-3**: Principles 2, 4, 7
- Invest in quality
- Think about security
- Own your work

**Month 3+**: Principles 1, 5, 6, 9
- Focus on user impact
- Measure and learn
- Grow continuously
- Iterate fast

**You'll grow into these principles. We're here to help.**

---

## For Leaders

As engineering leaders, we must:
- **Model** these principles in our own work
- **Recognize** engineers who embody them
- **Coach** engineers who struggle with them
- **Protect** the team from forces that undermine them
- **Evolve** them as our needs change

**Culture flows from the top. Be the engineer you want your team to be.**

---

## Evolution

These principles aren't set in stone. As we grow and learn, they'll evolve.

**How to propose changes:**
1. Discuss in #engineering
2. Present in Engineering All-Hands
3. Get broad consensus
4. Update this document
5. Communicate widely

**Last major update**: 2024-03-16
**Next review**: Annually

---

## Summary Card

Print this, keep it visible:

```
DATACODE.APP ENGINEERING PRINCIPLES

1. User Impact > Technical Perfection
2. Velocity Through Quality
3. Team Success > Individual Heroics
4. Security is Everyone's Job
5. Measure, Don't Guess
6. Learn Continuously, Teach Generously
7. Own Your Impact
8. Communicate Early, Communicate Often
9. Iterate Fast, Fail Forward
10. Simple > Clever

We build great software by being great teammates.
```

---

**These principles guide everything we do. When in doubt, come back to these.**

**Questions?** Discuss in #engineering or Engineering All-Hands

**Last Updated**: 2024-03-16
**Owner**: CTO
**Contributors**: Entire Engineering Team
