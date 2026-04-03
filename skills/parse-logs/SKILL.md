---
name: parse-logs
description: "Ingests raw log files (JSON, syslog, nginx, app logs) and extracts structured events with severity, timestamps, and service attribution"
allowed-tools: Bash Read Write
---

# Parse Logs

## Purpose
Convert raw, noisy log input into a clean, structured event list that other skills
can reason over. This is always the first skill to run when an incident is reported.

## Instructions

1. **Accept input** as a file path, piped stdin, or raw pasted log text.

2. **Detect log format** automatically:
   - JSON logs: parse fields directly (`level`, `msg`, `timestamp`, `service`, `trace_id`)
   - Nginx/Apache: parse combined log format
   - Syslog: extract facility, severity, host, process, message
   - Plain app logs: extract timestamp patterns and severity keywords

3. **For each log line, extract:**
   - `timestamp` (normalize to ISO 8601 UTC)
   - `severity` (normalize to: ERROR, WARN, INFO, DEBUG)
   - `service` (if attributable)
   - `message` (cleaned)
   - `trace_id` or `request_id` (if present)
   - `error_code` or `http_status` (if present)

4. **Filter and surface:**
   - All ERROR and CRITICAL level events
   - Any anomalous patterns: repeated errors, sudden gaps in logs, burst events
   - First and last occurrence timestamps for recurring errors

5. **Output** a structured Markdown summary:
```
   ## Log Parse Summary
   - Timespan: <start> â†’ <end>
   - Total lines: N
   - Errors: N | Warnings: N
   
   ## Notable Events
   | Timestamp | Severity | Service | Message |
   |-----------|----------|---------|---------|
   
   ## Anomalies Detected
   - ...
```

6. Redact any secrets, tokens, passwords, or PII before outputting.