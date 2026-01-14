# Runbook: [Incident Name]

## Overview
**Severity:** [Critical/High/Medium/Low]
**Component:** [Service/Feature name]
**Last Updated:** [YYYY-MM-DD]

Brief description of what this incident is and its impact on users/services.

---

## Detection & Alerts

### Key Metrics to Monitor
- Metric name: [threshold] - What indicates this issue is happening
- Metric name: [threshold] - What to look for

### Alert Triggers
- Alert name with condition
- What the alert tells you about the problem

---

## Diagnosis

### 1. Confirm the Issue
- [ ] Check [monitoring dashboard/logs] for [specific metric/error pattern]
- [ ] Verify the issue affects [all users/specific regions/feature]
- [ ] Note the start time: _________________
- [ ] Check recent deployments or changes that might be related

### 2. Gather Information
**Run these commands/queries:**

```
[Command or log query example]
```

**Look for:**
- Error messages or stack traces
- Unusual latency spikes
- Resource exhaustion (CPU, memory, disk)
- Failed external service calls

### 3. Check Common Causes
- [ ] Service health: Check if [specific service] is running and responsive
- [ ] Database connectivity: Run `[command to verify DB connection]`
- [ ] External dependencies: Verify API status for [external services]
- [ ] Resource limits: Check [CPU/memory/disk] usage with `[command]`
- [ ] Recent code changes: Review commits in last [X hours]

### 4. Review Logs
**Key log locations:**
- Application logs: `[path]`
- System logs: `[path]`
- Error logs: `[path]`

**Search for:**
```
[Example log patterns to search for]
```

---

## Resolution Steps

### Quick Fixes (Try These First)
1. **Restart the service**
   ```
   [Command to restart]
   ```
   - Monitor [metric] for recovery
   - Expected recovery time: [X minutes]

2. **Clear cache/refresh state**
   ```
   [Command to clear cache]
   ```
   - Verify with: `[verification command]`

3. **Scale up resources**
   - Increase [resource type] from [current] to [recommended]
   - Command: `[scaling command]`
   - Monitor: Watch [metric] for improvement

### If Quick Fixes Don't Work

#### Step 1: Isolate the Issue
- [ ] Is it affecting [all/specific] users?
- [ ] Is it [all features/specific feature]?
- [ ] Reproduce locally with: [steps to reproduce]

#### Step 2: Deep Dive Debugging
- Enable debug logging: `[command]`
- Attach debugger to [component]: `[command]`
- Check [specific subsystem] with: `[diagnostic command]`

#### Step 3: Implement Workaround (if needed)
- [Describe temporary workaround]
- **Caution:** This is temporary; file ticket for permanent fix
- Monitor for [specific issues] while workaround is active

#### Step 4: Deploy Fix
1. Create hotfix branch: `git checkout -b hotfix/[issue-name]`
2. Apply fix: [Brief description of changes]
3. Test locally: `[test command]`
4. Deploy: `[deployment command]`
5. Monitor: Watch [key metrics] for [X minutes]

---

## Rollback Plan

**If the fix makes things worse:**

1. Identify rollback commit: `git log --oneline | head -20`
2. Deploy previous version: `[rollback command]`
3. Verify recovery: Monitor [metric] for normalization
4. Timeline: Should recover within [X minutes]

---

## Verification

After applying the fix, verify recovery with:
- [ ] Metric [X] returned to normal: `[verification query]`
- [ ] No error rate spike: `[verification query]`
- [ ] End users can [key action]: Manual testing
- [ ] Alert [X] is no longer firing

---

## Prevention & Post-Incident

### Root Cause
[What actually caused this issue?]

### Prevention Measures
- [ ] Add monitoring for [new metric]
- [ ] Add test case for [scenario that should catch this]
- [ ] Improve [documentation/code/architecture]
- [ ] Alert on [new threshold] to catch earlier

### Tickets to File
- [ ] [Ticket description]: Priority [High/Medium]
- [ ] [Ticket description]: Priority [High/Medium]

### Related Incidents
- [Link to previous similar incidents]

---

## Escalation

**On-call escalation path:**
1. Page [Team/Person] if service not recovered after [X minutes]
2. Page [Escalation contact] if still unresolved after [X more minutes]

**Contact:**
- Primary on-call: [Slack/Phone]
- Backup: [Slack/Phone]
- Team lead: [Slack/Phone]

---

## Additional Resources

- [Link to monitoring dashboard]
- [Link to relevant documentation]
- [Link to architecture diagram]
- [Link to deployment runbook]

---

## Appendix: Common Issues & Solutions

| Issue | Symptoms | Quick Fix |
|-------|----------|-----------|
| [Issue name] | [What to look for] | [How to fix] |
| [Issue name] | [What to look for] | [How to fix] |

---

**Last Updated By:** [Name]
**Review Frequency:** Quarterly or after each incident
