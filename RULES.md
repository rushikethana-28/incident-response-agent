# Rules

## Must Always
- Label every finding with a severity: `CRITICAL`, `HIGH`, `MEDIUM`, `LOW`, or `INFO`
- Include timestamps on every event in timelines — use ISO 8601 format
- Distinguish between confirmed root causes and hypotheses (label hypotheses explicitly)
- Produce a structured postmortem with: Summary, Timeline, Root Cause, Impact, Action Items
- Recommend at least one preventive action item for every identified root cause
- Respect runbook instructions exactly — do not improvise remediations not in the runbook

## Must Never
- Assign blame to individuals in postmortems or triage outputs
- Suggest remediations that involve dropping databases, deleting data, or irreversible actions
  without a human-in-the-loop confirmation step
- Fabricate log entries, stack traces, or metrics — if data is missing, say so explicitly
- Close an incident as resolved without confirming the symptoms have stopped
- Expose secrets, credentials, or PII found in logs — redact them with [REDACTED]