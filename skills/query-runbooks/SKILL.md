---
name: query-runbooks
description: "Searches the runbooks/ directory to find the most relevant remediation procedure for the detected failure pattern"
allowed-tools: Bash Read Write
---

# Query Runbooks

## Purpose
Match the current incident's failure pattern, affected services, and error signatures
to the best available runbook, and extract the relevant remediation steps.

## Instructions

1. **Inputs:** Correlation summary (from `correlate-errors`) including:
   - Failure pattern type
   - Affected service(s)
   - Error codes or messages

2. **Search `runbooks/` directory:**
   - List all `.md` and `.yaml` files under `runbooks/`
   - Match by: service name, failure pattern keyword, error code
   - Rank matches by specificity (service + pattern > service only > pattern only)

3. **If a matching runbook is found:**
   - Extract and present the relevant steps verbatim
   - Note any prerequisites (permissions, tools, access needed)
   - Flag any steps that are destructive or irreversible â€” these require human confirmation

4. **If no runbook matches:**
   - State clearly: "No matching runbook found for <pattern> on <service>"
   - Suggest the most generic applicable procedure if one exists
   - Recommend creating a runbook after the incident is resolved

5. **Output:**
```
   ## Runbook Match

   File: runbooks/<matched-file>
   Match confidence: HIGH / PARTIAL / NONE

   ## Recommended Steps
   1. ...
   2. ...

   ## âš ï¸ Human Confirmation Required
   Step N involves <destructive action> â€” do not proceed without confirmation.

   ## Missing Runbook
   (if applicable) Suggested template: ...
```