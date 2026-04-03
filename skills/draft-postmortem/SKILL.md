---
name: draft-postmortem
description: "Produces a complete blameless postmortem document from triage findings, including timeline, root cause, impact, and action items"
allowed-tools: Bash Read Write
---

# Draft Postmortem

## Purpose
Turn the structured outputs of the triage process into a complete, human-ready
postmortem document. Blameless. Thorough. Ready to share with stakeholders.

## Instructions

1. **Inputs:** Collect all available triage artifacts:
   - Parsed log summary
   - Correlation summary
   - Runbook match and remediation steps taken
   - Any additional context provided (deploy times, alert timestamps, user reports)

2. **Write the postmortem with these required sections:**

   ### Incident Summary
   - One-paragraph executive summary: what broke, when, impact, how resolved

   ### Severity & Impact
   - Severity level: SEV1 / SEV2 / SEV3
   - Duration: detection time â†’ resolution time
   - Users/services affected
   - Estimated business impact (if inferable)

   ### Timeline
   - Chronological list of events in ISO 8601 timestamps
   - Include: first alert, detection, escalation, mitigation, resolution
   - Format: `HH:MM UTC â€” <what happened>`

   ### Root Cause
   - Confirmed root cause (clearly labeled as CONFIRMED)
   - Contributing factors
   - Any hypotheses that were ruled out (labeled as RULED OUT)

   ### What Went Well
   - Detection mechanisms that worked
   - Response actions that were effective

   ### What Went Wrong
   - Gaps in monitoring, alerting, or runbooks
   - Delays or missteps in the response

   ### Action Items
   | Priority | Action | Owner (TBD) | Due |
   |----------|--------|-------------|-----|
   | HIGH | ... | TBD | ... |

3. **Tone:** Blameless. Focus on systems, processes, and tooling â€” never individuals.

4. **Save output** to `postmortems/YYYY-MM-DD-<incident-slug>.md`