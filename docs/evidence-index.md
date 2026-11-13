# Evidence index and provenance

Screenshots are point-in-time evidence. The two AWS security-group screenshots and the Wazuh dashboard screenshot are sanitized public copies; original source images are retained outside the public repository build.


| Screenshot | What it supports | Notes |
|---|---|---|
| <img src="../screenshots/01-wazuh-agent-active.png" width="180" alt="Wazuh agent active"> | Wazuh agent `001` shown Active | Existing recovered screenshot |
| <img src="../screenshots/02-sysmon-events-in-wazuh.png" width="180" alt="Sysmon events in Wazuh"> | Sysmon Operational events searchable in Wazuh; 46 hits shown | Existing recovered screenshot |
| <img src="../screenshots/03-shuffle-sha256-extraction.png" width="180" alt="SHA256 extraction"> | Regex capture returned `SUCCESS` / `found: true` | Existing recovered screenshot |
| <img src="../screenshots/04-virustotal-http-200.png" width="180" alt="VirusTotal HTTP 200"> | VirusTotal node returned HTTP `200` | Existing recovered screenshot |
| <img src="../screenshots/05-shuffle-workflow.png" width="180" alt="Shuffle workflow"> | Workflow topology; VirusTotal branches separately to Email and HTTP | Existing recovered screenshot |
| <img src="../screenshots/06-thehive-alert-created.png" width="180" alt="TheHive alert created"> | TheHive UI displayed an alert with the project title and High/TLP/PAP labels | Earlier UI validation; not the exact same alert ID as the latest HTTP response |
| <img src="../screenshots/07-shuffle-email-node-configuration.png" width="180" alt="Email node configuration"> | Email action fields/mappings | Recipient redacted |
| <img src="../screenshots/08-sysmon-event-id-1.png" width="180" alt="Sysmon Event ID 1"> | Sysmon Process Create event for `mimikatz.exe` | Windows username redacted |
| <img src="../screenshots/09-wazuh-rule-100002-alert.png" width="180" alt="Wazuh rule alert"> | Wazuh Threat Hunting shows rule `100002`, Level `12`, T1003 | Dashboard address and manager identifiers redacted |
| <img src="../screenshots/10-shuffle-run-finished.png" width="180" alt="Shuffle execution finished"> | Shuffle run status `FINISHED`, start and finish times | Debug view |
| <img src="../screenshots/11-shuffle-sha256-extraction-success.png" width="180" alt="SHA256 extraction success"> | Shuffle Tools returned `success: true`, `found: true` | Debug node result |
| <img src="../screenshots/12-shuffle-virustotal-success.png" width="180" alt="VirusTotal success"> | VirusTotal node returned `200`, `success: true` | Debug node result |
| <img src="../screenshots/13-shuffle-thehive-and-email-success.png" width="180" alt="TheHive and email success"> | HTTP `201`, HTTP success, and Email successfully sent | TheHive host redacted |
| <img src="../screenshots/14-aws-wazuh-security-group-review.png" width="180" alt="Wazuh security group"> | Inbound rules for Wazuh ports 1514/1515 and admin ports | AWS identifiers/admin IP redacted; `0.0.0.0/0` retained |
| <img src="../screenshots/15-aws-thehive-security-group-review.png" width="180" alt="TheHive security group"> | Inbound rules for TheHive port 9000 | AWS identifiers/admin IP redacted; `0.0.0.0/0` retained |
| <img src="../screenshots/16-email-notification-received.png" width="180" alt="Gmail email notification"> | Gmail inbox showing successful delivery of the Wazuh alert notification | Direct email evidence |


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
