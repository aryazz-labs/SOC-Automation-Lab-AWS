# Evidence Index and Provenance


| File | What it supports | What it does not prove |
|---|---|---|
|![Active Wazuh agent](https://github.com/aryazz-labs/SOC-Automation-Lab-AWS/blob/main/screenshots/01-wazuh-agent-active.png)  | Wazuh dashboard shows agent `001` active, Windows 11 Pro, agent version `4.14.10` | It is a point-in-time status screenshot, not continuous uptime |
| ![Sysmon-Events](https://github.com/aryazz-labs/SOC-Automation-Lab-AWS/blob/main/screenshots/02-sysmon-events-in-wazuh.png) | Wazuh Threat Hunting view filtered to Sysmon Operational channel; 46 hits shown | It is not a screenshot of the original Windows Event Viewer event or the custom Mimikatz rule alert |
| ![Shuffle-SHA256-extr](https://github.com/aryazz-labs/SOC-Automation-Lab-AWS/blob/main/screenshots/03-shuffle-sha256-extraction.png) | Shuffle Tools regex action succeeded and extracted the SHA256 | It does not prove downstream actions succeeded |
| ![VirusTotal-http-200](https://github.com/aryazz-labs/SOC-Automation-Lab-AWS/blob/main/screenshots/04-virustotal-http-200.png) | VirusTotal node returned HTTP `200` and a file object for the hash | HTTP `200` is not a malware verdict |
| ![Shuffle-Workflow](https://github.com/aryazz-labs/SOC-Automation-Lab-AWS/blob/main/screenshots/05-shuffle-workflow.png) | Final workflow canvas and branches from VirusTotal to HTTP and Email | A canvas alone does not prove each action succeeded in the same execution |
| ![TheHive-Alert](https://github.com/aryazz-labs/SOC-Automation-Lab-AWS/blob/main/screenshots/06-thehive-alert-created.png) | TheHive alert details, source, severity, tags, and title | Endpoint-specific description fields were redacted; it is an alert, not a case |
