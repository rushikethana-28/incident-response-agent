---
name: suggest-remediation
description: "Generates prioritized, actionable remediation steps for the current incident when no runbook exists or runbook is incomplete"
allowed-tools: Bash Read Write
---

# Suggest Remediation

## Purpose
When runbooks are missing or incomplete, reason from first principles about the
failure pattern and suggest safe, prioritized remediation steps. Always flag
steps that require human approval before execution.

## Instructions

1. **Inputs:** Correlation summary and (if available) partial runbook match.

2. **Generate remediation steps based on detected failure pattern:**

   **Cascade failure:**
   - Identify and isolate the origin service
   - Implement circuit breaker or shed load from healthy services
   - Restart origin service after verifying dependency health
   - Gradually re-enable traffic

   **Bad deploy:**
   - Identify the deployment that correlates with error spike
   - Initiate rollback to last known good version
   - Verify error rate returns to baseline after rollback
   - Block re-deploy until root cause is fixed

   **Resource exhaustion (memory/CPU/disk):**
   - Identify the resource and process consuming it
   - Scale horizontally if possible (preferred, non-destructive)
   - Restart the offending process (flag as requiring confirmation)
   - Increase resource limits as temporary mitigation

   **Dependency/external outage:**
   - Enable fallback or degraded mode if available
   - Implement retry with exponential backoff
   - Surface status to users if SLA is breached
   - Monitor dependency status page

   **Thundering herd:**
   - Enable rate limiting at the API gateway
   - Add jitter to retry logic
   - Scale up backend capacity temporarily

3. **For each step, label:**
   - `[SAFE]` â€” can be executed immediately
   - `[CONFIRM]` â€” requires human confirmation before executing
   - `[MONITOR]` â€” observe for N minutes before proceeding

4. **Output:**
```
   ## Suggested Remediation
   Pattern: <type>

   ### Immediate Steps
   1. [SAFE] ...
   2. [CONFIRM] ...

   ### Follow-up Steps
   3. [MONITOR 5min] ...

   ### Prevention
   - Long-term fix: ...
   - Runbook to create: ...
```