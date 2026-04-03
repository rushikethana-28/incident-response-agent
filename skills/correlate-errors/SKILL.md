---
name: correlate-errors
description: "Cross-references parsed events across multiple services to identify cascading failures, shared root causes, and blast radius"
allowed-tools: Bash Read Write
---

# Correlate Errors

## Purpose
Given structured events from one or more services (output of `parse-logs`), find the
connections â€” shared trace IDs, temporal clusters, dependency chains â€” that reveal
what triggered what.

## Instructions

1. **Inputs:** Accept one or more parsed log summaries or raw structured event lists.

2. **Group events by `trace_id` or `request_id`** where available to reconstruct
   request flows across services.

3. **Build a temporal cluster map:**
   - Find bursts of errors within the same 30-second window across services
   - Identify which service produced the first error in each cluster (likely origin)
   - Note downstream services that errored within 2 minutes of the first (likely cascade)

4. **Identify failure patterns:**
   - **Cascade failure:** Service A errors â†’ Service B errors shortly after
   - **Thundering herd:** Sudden spike in identical requests at same timestamp
   - **Bad deploy:** Error rate spikes immediately after a deployment event
   - **Resource exhaustion:** Gradual error increase â†’ total failure
   - **Dependency outage:** All errors trace to calls to the same external service

5. **Determine blast radius:**
   - Which services were affected?
   - Which endpoints or operations failed?
   - Estimated % of traffic impacted (if sample size is inferable)

6. **Output:**
```
   ## Correlation Summary

   ### Most Likely Origin
   - Service: <name>
   - First error: <timestamp>
   - Evidence: ...

   ### Cascade Chain
   <ServiceA> â†’ <ServiceB> â†’ <ServiceC>

   ### Failure Pattern
   Type: <pattern>
   Confidence: HIGH / MEDIUM / LOW
   Reasoning: ...

   ### Blast Radius
   - Affected services: ...
   - Affected operations: ...
   - Estimated user impact: ...
```