# Incident Response Guide

## Purpose
This guide provides a clear, actionable framework for responding to production incidents. Speed and clarity during incidents can make the difference between a minor disruption and a major outage.

## Core Principles

1. **Customer impact first** - Always prioritize restoring service
2. **Communicate early and often** - Keep stakeholders informed
3. **Document everything** - Record actions and decisions
4. **Blameless post-mortems** - Focus on systems, not people
5. **Learn and improve** - Every incident is a learning opportunity

---

## Incident Severity Levels

### SEV 1 (Critical) 🔴
**Response Time**: Immediate (< 5 minutes)

**Definition**: System is completely down or severely degraded for all/most users
- Complete service outage
- Data loss or corruption
- Security breach
- Payment processing down
- Critical functionality unavailable for all users

**Actions**:
- Page on-call engineer immediately
- Notify CTO/leadership within 15 minutes
- Create public status page incident
- Update customers every 30 minutes
- All hands on deck - pull in additional engineers

### SEV 2 (High) 🟠
**Response Time**: Within 30 minutes

**Definition**: Significant functionality degraded for many users
- Major feature unavailable
- Severe performance degradation
- Affecting a large segment of users (> 20%)
- Revenue-impacting but not complete outage
- Workaround exists but difficult

**Actions**:
- Notify on-call engineer
- Inform product/customer success teams
- Update status page
- Update stakeholders every 2 hours
- Plan mitigation strategy

### SEV 3 (Medium) 🟡
**Response Time**: Within 2 hours

**Definition**: Minor functionality issues affecting some users
- Non-critical feature broken
- Performance issues for subset of users
- Cosmetic issues on important pages
- Workaround is easy
- Affecting < 20% of users

**Actions**:
- Create ticket and assign
- Notify relevant team members
- Fix during business hours
- Update stakeholders daily

### SEV 4 (Low) 🟢
**Response Time**: Next business day

**Definition**: Minor issues with minimal impact
- Cosmetic bugs
- Documentation errors
- Non-critical logging issues
- Issues in non-production environments

**Actions**:
- Add to backlog
- Fix in normal workflow
- No urgent communication needed

---

## Incident Response Process

### Phase 1: Detection & Alert (0-5 minutes)

#### How Incidents are Detected
- Automated monitoring alerts (PagerDuty, Datadog, etc.)
- Customer reports (support tickets, social media)
- Internal team observation
- Third-party service status

#### Immediate Actions
```
1. Acknowledge the alert (stop paging)
2. Quickly assess severity (use definitions above)
3. Create incident channel: #incident-YYYY-MM-DD-description
4. Post initial message:
   "🚨 INCIDENT: [Brief description]
   Severity: SEV X
   Incident Commander: [Your name]
   Time Detected: [HH:MM UTC]
   Status: Investigating"
```

### Phase 2: Initial Assessment (5-15 minutes)

#### Gather Information
- [ ] What is the customer impact?
- [ ] When did it start?
- [ ] What changed recently? (deployments, config, infrastructure)
- [ ] What systems are affected?
- [ ] Is it getting worse?

#### Key Commands to Run
```bash
# Check application health
curl https://api.yourapp.com/health

# Recent deployments
git log --since="2 hours ago" --oneline

# Check error rates
# (Your monitoring dashboard link)

# Database connections
# (Your DB monitoring query)

# Traffic patterns
# (Your analytics/metrics query)
```

#### Update Stakeholders
```
Post in #incident channel:
"📊 ASSESSMENT UPDATE
Severity: [Confirmed SEV level]
Impact: [X users/% of traffic/specific feature]
Symptoms: [What users are experiencing]
Started: [Best estimate of start time]
Recent Changes: [Last deployment/change]
Next Steps: [Investigation plan]
Updated: [HH:MM UTC]"
```

### Phase 3: Mitigation (15 minutes - X hours)

#### Mitigation Strategies (in order of preference)

1. **Revert Recent Changes** (Fastest)
   ```bash
   # Revert last deployment
   git revert HEAD
   git push origin main
   # Trigger deployment

   # Or rollback via deployment tool
   kubectl rollout undo deployment/your-app
   ```

2. **Disable Problematic Feature**
   ```bash
   # Toggle feature flag
   # Update config to disable feature
   # Remove problematic code path
   ```

3. **Scale Resources**
   ```bash
   # Add more instances
   kubectl scale deployment/your-app --replicas=10

   # Increase resource limits
   # Scale database
   ```

4. **Failover to Backup**
   ```bash
   # Switch to backup database
   # Redirect to backup region
   # Enable read-only mode
   ```

5. **Apply Hotfix**
   ```bash
   # Create hotfix branch
   git checkout -b hotfix/issue-description
   # Make minimal fix
   # Fast-track review and deploy
   ```

#### During Mitigation
- [ ] Document every action taken in incident channel
- [ ] Update status page every 30 min (SEV1), 2 hours (SEV2)
- [ ] If not improving in 30 min, escalate or try different approach
- [ ] Keep stakeholders updated on progress

### Phase 4: Recovery Verification (X hours + 30 minutes)

#### Confirm Resolution
- [ ] Monitoring shows normal levels
- [ ] Error rates back to baseline
- [ ] Manual testing of affected functionality
- [ ] Customer reports declining
- [ ] No new alerts firing

#### Validation Checklist
```markdown
- [ ] Primary metrics returned to normal
- [ ] Error logs clear
- [ ] Customer success confirms user reports stopped
- [ ] All systems green in monitoring dashboard
- [ ] Sustained stability for 30+ minutes
```

#### Communicate Resolution
```
Post in #incident channel:
"✅ INCIDENT RESOLVED
Duration: [Start time - End time] ([X hours Y minutes])
Resolution: [What fixed it]
Impact Summary: [Final count of affected users/transactions]
Next Steps: Post-mortem scheduled for [Date/Time]
Incident closed at: [HH:MM UTC]"
```

### Phase 5: Post-Incident (24-48 hours after)

#### Immediate Follow-up (Within 4 hours)
- [ ] Update all stakeholders with final summary
- [ ] Close status page incident
- [ ] Thank everyone involved
- [ ] Schedule post-mortem meeting (within 48 hours)
- [ ] Create post-mortem document

#### Post-Mortem Meeting
**Attendees**: All involved engineers, product, leadership

**Agenda** (60 minutes):
1. Timeline review (10 min)
2. What went well (10 min)
3. What went poorly (15 min)
4. Root cause analysis (15 min)
5. Action items (10 min)

**Rules**:
- **Blameless**: Focus on systems, not people
- **Fact-based**: Use data and logs
- **Forward-looking**: How do we prevent this?

---

## Roles and Responsibilities

### Incident Commander (IC)
**Who**: First engineer to respond becomes IC (can delegate)

**Responsibilities**:
- Declare incident and severity
- Create and run incident channel
- Coordinate response efforts
- Make final decisions
- Communicate with stakeholders
- Run post-mortem

### On-Call Engineer
**Who**: Rotates weekly (see PagerDuty schedule)

**Responsibilities**:
- Respond to pages within 5 minutes
- Assess severity and escalate if needed
- Begin investigation and mitigation
- Call in backup if needed

### Engineering Support
**Who**: Additional engineers called in for SEV1/SEV2

**Responsibilities**:
- Follow IC direction
- Investigate specific areas
- Implement fixes
- Document findings

### Communications Lead
**Who**: Product Manager or Customer Success Lead

**Responsibilities**:
- Update status page
- Draft customer communications
- Monitor social media / support tickets
- Interface with customers

---

## Communication Templates

### Internal Slack Update (Every 30 min for SEV1, 2 hours for SEV2)
```
⏱️ INCIDENT UPDATE [HH:MM UTC]
Status: [Investigating / Mitigating / Resolved]
Progress: [What we've done]
Current Theory: [What we think is wrong]
Next Steps: [What we're trying next]
ETA: [Best guess or "unknown"]
```

### Status Page Update (External)
```
Investigating:
We are currently investigating reports of [issue description].
Users may experience [specific impact]. Our team is working to
identify the root cause. Updates will be provided every 30 minutes.

Identified:
We have identified the issue as [cause]. Our team is working on
implementing a fix. [Optional: Workaround if available]

Monitoring:
A fix has been applied and we are monitoring the results.
We expect full resolution shortly.

Resolved:
This incident has been resolved. [Brief explanation of what happened
and what was done]. We apologize for any inconvenience.
```

### Customer Email (If major impact)
```
Subject: [Resolved] Service Disruption on [Date]

Dear [Customer],

We want to inform you about a service disruption that occurred on
[Date] from [Time] to [Time] UTC.

What Happened:
[Brief, non-technical explanation]

Impact:
[What users experienced]

Resolution:
[What we did to fix it]

Prevention:
[Steps we're taking to prevent recurrence]

We sincerely apologize for any inconvenience this may have caused.
If you have questions, please contact support@datacode.app.

Best regards,
The Datacode.app Team
```

---

## On-Call Rotation

### Schedule
- View current schedule: PagerDuty dashboard
- Rotates: Weekly (Monday 9 AM UTC)
- Backup: Previous week's on-call

### On-Call Expectations
- **Response time**: < 5 minutes for pages
- **Availability**: Able to access laptop and internet
- **Sobriety**: Able to respond effectively
- **Handoff**: Brief next on-call on any ongoing issues

### On-Call Compensation
- On-call stipend: [Amount] per week
- Incident response: Compensatory time off
- Weekend pages: Additional compensation

### Escalation Path
```
On-Call Engineer
    ↓ (if no response in 10 min)
Backup On-Call
    ↓ (if SEV1 or no response)
Engineering Lead
    ↓ (if continues or major)
CTO
```

---

## Incident Response Playbooks

### Database Issues
```
1. Check connection pool status
2. Review slow query log
3. Check for locks: SELECT * FROM pg_locks;
4. Check disk space: df -h
5. Check replication lag
6. Consider: Kill long-running queries
7. Consider: Scale read replicas
8. Consider: Failover to backup
```

### High Error Rate
```
1. Check error logs for patterns
2. Review recent deployments
3. Check third-party service status
4. Look for traffic spike (DDoS?)
5. Check resource utilization (CPU, memory)
6. Consider: Rollback recent deployment
7. Consider: Rate limiting
8. Consider: Scale up resources
```

### Performance Degradation
```
1. Check response time metrics
2. Check database query performance
3. Check cache hit rates
4. Review recent code changes
5. Check external API latency
6. Look for memory leaks
7. Consider: Clear cache
8. Consider: Scale horizontally
9. Consider: Enable read-only mode
```

### Security Incident
```
1. IMMEDIATELY isolate affected systems
2. Preserve logs and evidence
3. Notify CTO/CEO immediately
4. Do NOT communicate publicly until assessed
5. Contact security team/consultant
6. Follow security incident response plan
7. Consider: Legal/PR consultation
8. Document everything meticulously
```

---

## Tools and Resources

### Monitoring & Alerting
- **Application Monitoring**: [Datadog/New Relic link]
- **Infrastructure**: [AWS CloudWatch/GCP link]
- **Logging**: [ELK/Splunk link]
- **Status Page**: [StatusPage.io link]
- **On-Call**: [PagerDuty link]

### Quick Links
- Production dashboard: [Link]
- Deployment logs: [Link]
- Customer impact calculator: [Link]
- Incident post-mortem template: [Link]
- War room video call: [Zoom/Meet link]

### Contact Information
- On-call hotline: [Phone number]
- CTO mobile: [Phone number]
- Security team: security@datacode.app
- Infrastructure team lead: [Phone]

---

## Post-Mortem Template

```markdown
# Post-Mortem: [Brief Incident Description]

**Date**: [YYYY-MM-DD]
**Authors**: [Names]
**Status**: Draft | Reviewed | Final
**Severity**: SEV X

## Executive Summary
[2-3 sentence summary of what happened, impact, and resolution]

## Impact
- **Duration**: [X hours Y minutes]
- **Users Affected**: [Number or percentage]
- **Revenue Impact**: [$X or N/A]
- **Services Affected**: [List]

## Timeline (All times UTC)
- **HH:MM** - [First sign of issue]
- **HH:MM** - [Alert fired]
- **HH:MM** - [Engineer responded]
- **HH:MM** - [Root cause identified]
- **HH:MM** - [Fix applied]
- **HH:MM** - [Service restored]
- **HH:MM** - [Incident closed]

## Root Cause
[Detailed technical explanation of what caused the incident]

## Resolution
[What was done to fix it]

## What Went Well
- [Thing 1]
- [Thing 2]

## What Went Poorly
- [Thing 1]
- [Thing 2]

## Action Items
| Action | Owner | Due Date | Status |
|--------|-------|----------|--------|
| [Specific action] | [Name] | [Date] | Open |

## Lessons Learned
[Key takeaways for the team]

---
*Post-mortem completed on [Date]*
*Next review: [Date or N/A]*
```

---

## Metrics We Track

- **MTTD** (Mean Time To Detect): Alert to acknowledgment
- **MTTR** (Mean Time To Resolve): Detection to resolution
- **Incident frequency**: Number per month
- **Severity distribution**: SEV1/2/3/4 breakdown
- **Repeat incidents**: Same root cause

**Goals**:
- MTTD < 5 minutes (95th percentile)
- MTTR < 2 hours for SEV2
- Zero repeat SEV1 incidents
- 100% post-mortems completed within 1 week

---

## Remember

🚨 **During an incident:**
- Stay calm
- Communicate clearly
- Document everything
- Fix first, investigate later
- Ask for help if needed

🎓 **After an incident:**
- Be blameless
- Be thorough
- Be honest
- Create action items
- Follow through

**The best incident response is incident prevention. Every post-mortem should result in concrete improvements to our systems and processes.**
