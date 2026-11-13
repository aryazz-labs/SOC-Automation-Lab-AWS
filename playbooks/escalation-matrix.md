# Escalation matrix

| Condition | Suggested handling | Automation in this lab |
|---|---|---|
| Wazuh rule `100002` fires in controlled lab test | Validate process evidence and record test result | Alert and enrichment automated |
| Same rule fires on an unapproved endpoint | Escalate to SOC/IR lead; preserve endpoint evidence | Email and TheHive alert automated |
| Corroborating LSASS access, credential dumping or suspicious logon behavior | High-priority incident review and approved containment | Not implemented |
| VirusTotal report unavailable or inconclusive | Continue local endpoint investigation; do not close solely on lookup failure | HTTP result recorded; no robust fallback branch evidenced |
| TheHive duplicate source reference / HTTP 400 | Check existing alert and avoid creating duplicate | Explicit idempotency logic not evidenced |
| Shuffle node failure | Review run output and handle manually | Production retry/error handling not implemented |
