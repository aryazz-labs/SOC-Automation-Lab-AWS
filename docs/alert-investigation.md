# SOC Alert Investigation Record

## 1. Alert summary

| Field | Value |
|---|---|
| Alert title | `SOC-Lab: Mimikatz execution detected on Windows endpoint.` |
| Source | Wazuh |
| TheHive type | `wazuh-alert` |
| Wazuh rule ID | `100002` |
| Wazuh alert level | `12` |
| Endpoint agent | `wazah-agent-windows` (ID `001`) |
| Event source | Sysmon Event ID `1` — Process Create |
| Process | `mimikatz.exe` |
| Parent process | Windows PowerShell |
| File version | `2.2.0.0` |
| Integrity level | High |
| SHA256 | `61C0810A23580CF492A6BA4F7654566108331E7A4134C968C2D6A05261B2D8A1` |
| TheHive severity | High |
| TLP / PAP | Amber / Amber |
| TheHive organization | `Aryan-SOC-Automation` |

## 2. Detection context

The monitored Windows 11 Pro virtual machine generated a Sysmon process-creation event for `mimikatz.exe`. The Wazuh custom rule identified the event and generated a Level 12 alert. The alert was forwarded to Shuffle for automated enrichment and notification.

This was a controlled lab test. The evidence confirms process execution and alert processing; it does not prove that Mimikatz successfully extracted credentials or that any credentials were compromised.

## 3. Evidence collected

- Wazuh dashboard showing the Windows agent active.
- Wazuh Threat Hunting view filtered to `Microsoft-Windows-Sysmon/Operational` (46 hits shown in the recovered screenshot).
- Shuffle Tools output showing successful extraction of the SHA256 hash.
- VirusTotal action output showing HTTP `200` and a file-report object.
- TheHive alert record with source Wazuh, rule ID `100002`, process details, hashes, High severity, Amber TLP/PAP, and tags.

## 4. Threat intelligence enrichment

Shuffle submitted the extracted SHA256 to VirusTotal v3. The recovered action output shows HTTP `200`, indicating that the API returned a file report. The API status alone does not determine whether the file is malicious. The report's detection statistics and metadata should be reviewed in VirusTotal before assigning a reputation verdict.

## 5. Automation outcome

The project notes record the following outcomes:

- Wazuh alert received by the Shuffle webhook.
- SHA256 extracted successfully.
- VirusTotal returned HTTP `200`.
- TheHive alert creation returned HTTP `201`; the created alert is visible in the organization.
- Email notification delivery was reported by the project owner.

The recovered final Shuffle canvas shows HTTP and Email as separate branches from the VirusTotal node.

## 6. Analyst assessment

**Assessment:** High-priority suspicious process execution detected in a controlled lab environment. The detection and enrichment pipeline operated sufficiently to create a TheHive alert. The event should be treated as suspicious because of the executable observed and the custom detection that matched it.

**Confidence boundaries:** The current evidence does not establish successful credential dumping, credential compromise, lateral movement, persistence, or impact on other hosts. No endpoint containment or remediation action was performed by this workflow.

## 7. Recommended analyst follow-up

1. Confirm whether the execution was authorized as part of the lab test.
2. Review the full process command line, executable location, signer, and file reputation.
3. Correlate nearby process, logon, PowerShell, and network events on the endpoint.
4. Search for related activity on other monitored endpoints if this were a production alert.
5. Escalate and contain the endpoint only if the activity is unauthorized and the response is approved.

## 8. Disposition

**Lab disposition:** Controlled detection validation.  
**TheHive object created:** Alert. No case creation is evidenced.  
**Automated containment:** Not implemented.  
**ATT&CK mapping:** The project notes previously recorded T1036, but the exact Wazuh rule XML was not recovered; the mapping is therefore left unverified and is not asserted here.

