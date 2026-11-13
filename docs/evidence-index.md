# Evidence index and provenance

Screenshots are point-in-time evidence. The two AWS security-group screenshots and the Wazuh dashboard screenshot are sanitized public copies; original source images are retained outside the public repository build.

| File | What it supports | Notes |
|---|---|---|
| `01-wazuh-agent-active.png` | Wazuh agent `001` shown Active | Existing recovered screenshot |
| `02-sysmon-events-in-wazuh.png` | Sysmon Operational events searchable in Wazuh; 46 hits shown | Existing recovered screenshot |
| `03-shuffle-sha256-extraction.png` | Regex capture returned `SUCCESS` / `found: true` | Existing recovered screenshot |
| `04-virustotal-http-200.png` | VirusTotal node returned HTTP `200` | Existing recovered screenshot |
| `05-shuffle-workflow.png` | Workflow topology; VirusTotal branches separately to Email and HTTP | Existing recovered screenshot |
| `06-thehive-alert-created.png` | TheHive UI displayed an alert with the project title and High/TLP/PAP labels | Earlier UI validation; not the exact same alert ID as the latest HTTP response |
| `07-shuffle-email-node-configuration.png` | Email action fields/mappings | Recipient redacted |
| `08-sysmon-event-id-1.png` | Sysmon Process Create event for `mimikatz.exe` | Windows username redacted |
| `09-wazuh-rule-100002-alert.png` | Wazuh Threat Hunting shows rule `100002`, Level `12`, T1003 | Dashboard address and manager identifiers redacted |
| `10-shuffle-run-finished.png` | Shuffle run status `FINISHED`, start and finish times | Debug view |
| `11-shuffle-sha256-extraction-success.png` | Shuffle Tools returned `success: true`, `found: true` | Debug node result |
| `12-shuffle-virustotal-success.png` | VirusTotal node returned `200`, `success: true` | Debug node result |
| `13-shuffle-thehive-and-email-success.png` | HTTP `201`, HTTP success, and Email successfully sent | TheHive host redacted |
| `14-aws-wazuh-security-group-review.png` | Inbound rules for Wazuh ports 1514/1515 and admin ports | AWS identifiers/admin IP redacted; `0.0.0.0/0` retained |
| `15-aws-thehive-security-group-review.png` | Inbound rules for TheHive port 9000 | AWS identifiers/admin IP redacted; `0.0.0.0/0` retained |
| `16-email-notification-delivered.png` | Gmail inbox showing successful delivery of the Wazuh alert notification | Direct email evidence |

## Structured evidence

- `../samples/wazuh-alert-100002.json`: sanitized Wazuh alert JSON supplied by the project owner.
- `../samples/virustotal-report-summary.json`: normalized subset of the actual Shuffle VirusTotal node output; not the full raw API JSON.
- `../samples/virustotal-node-output-capture.txt`: text capture of the VirusTotal node result.
- `../samples/thehive-alert-creation-response.json`: sanitized representation of the HTTP 201 TheHive response supplied by the project owner.
- `../samples/shuffle-execution-summary.json`: transcription of the successful Shuffle Debug run; not raw execution JSON.
- `../playbooks/SOC-Automation-Workflow.sanitized.json`: sanitized copy of the actual exported workflow; not directly importable because credentials/endpoints are placeholders.

The Gmail inbox screenshot is included as `16-email-notification-delivered.png` and provides direct evidence of email delivery.

## Not claimed as evidence

- Successful credential dumping or credential compromise.
- Automated endpoint containment or remediation.
- A TheHive case creation (the workflow created an alert).
- A completed negative/false-positive test for benign processes.
- A complete VirusTotal antivirus detection ratio (aggregate stats were not visible in the copied result).
