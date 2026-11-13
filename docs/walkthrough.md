# Implementation Walkthrough

This walkthrough records the implemented lab flow using the project notes and screenshots recovered from the earlier work.

## 1. Windows endpoint and Sysmon

- Created a Windows 11 Pro VM in VirtualBox.
- Installed Sysmon with an advanced configuration based on the Olaf Hartong Sysmon configuration.
- Confirmed Sysmon events were generated in `Microsoft-Windows-Sysmon/Operational`.
- The recovered Wazuh Threat Hunting screenshot shows events filtered to the Sysmon Operational channel, with 46 matching events at the time of capture.

The installation command recorded in the lab was:

```powershell
.\Sysmon64.exe -accepteula -i .\sysmonconfig.xml
```

## 2. Wazuh agent collection

The Windows Wazuh agent was configured to collect the Sysmon Operational event channel. The relevant `ossec.conf` block was:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

The agent service was restarted after the change. A recovered Wazuh dashboard screenshot shows one active Windows 11 Pro agent:

- Agent name: `wazah-agent-windows`
- Agent ID: `001`
- Agent version: `4.14.10`
- Status: Active

## 3. Custom detection validation

The project report records a custom Wazuh rule with ID `100002`, Level `12`, and description:

```text
SOC-Lab: Mimikatz execution detected on Windows endpoint.
```

The controlled test observed `mimikatz.exe` version `2.2.0.0`, launched by Windows PowerShell, with High integrity. The event was Sysmon Event ID `1` (Process Create).

SHA256 observed in the event:

```text
61C0810A23580CF492A6BA4F7654566108331E7A4134C968C2D6A05261B2D8A1
```

The evidence supports detection of process execution. It does not establish successful credential dumping. The raw rule XML and a standalone Wazuh alert screenshot were not recovered.

## 4. Shuffle workflow

The workflow name was `SOC-Automation-Workflow`. The recovered final canvas shows these nodes:

| Node | Role |
|---|---|
| Webhook 1 | Receives the Wazuh alert |
| Shuffle Tools 1 | Extracts the SHA256 value |
| VirusTotal v3 1 | Queries the VirusTotal file report |
| HTTP 1 | Sends a structured alert to TheHive |
| Email 1 | Sends the notification |

The hash extraction regular expression was:

```regex
SHA256=([0-9A-Fa-f]{64})
```

A recovered Shuffle Tools result shows `SUCCESS`, `found: true`, and the captured 64-character SHA256 value.

The final recovered canvas shows `HTTP 1` and `Email 1` as separate branches from VirusTotal.

## 5. VirusTotal enrichment

A recovered VirusTotal node result shows HTTP status `200` and a file object returned for the SHA256 value. Earlier attempts produced HTTP `404` responses because the request URL was malformed / the wrong value was passed. The extraction and mapping were corrected before the successful result. A `200` response confirms the API returned a file report; it is not itself a malware verdict.

## 6. TheHive alert creation

The workflow used a generic HTTP POST action to TheHive's alert endpoint:

```text
POST http://<THEHIVE_HOST>:9000/api/v1/alert
```

The project report records HTTP `201` on successful creation. A recovered TheHive screenshot shows an alert created by `Soar` in the `Aryan-SOC-Automation` organization, with:

- Title: `SOC-Lab: Mimikatz execution detected on Windows endpoint.`
- Source: `Wazuh`
- Type: `wazuh-alert`
- Severity: High
- TLP: Amber
- PAP: Amber
- Tags including `malware`, `sysmon`, `virustotal`, `wazuh`, and `windows`
- Description containing agent, rule ID, process, parent process, hashes, and VirusTotal report link

The native Shuffle TheHive action encountered a connectivity/compatibility issue. The generic HTTP action was used instead. The workflow created a TheHive **alert**, not a case.

## 7. Email notification

The project record says the email notification was received. The recovered final workflow canvas shows Email 1 connected as a separate branch from VirusTotal. The actual inbox/received-message screenshot was not present in the recovered files, so the README identifies email delivery as user-reported validation rather than screenshot-backed evidence.

## 8. End-to-end status

| Stage | Recorded outcome | Evidence available in this package |
|---|---|---|
| Wazuh agent | Active | Screenshot |
| Sysmon event collection | 46 Sysmon-channel hits in Wazuh search | Screenshot |
| Custom rule `100002` | Level 12 alert reported | TheHive alert details include rule ID; standalone Wazuh alert screenshot missing |
| SHA256 extraction | Successful | Screenshot |
| VirusTotal | HTTP 200 | Screenshot |
| TheHive | Alert created; HTTP 201 reported | Screenshot of created alert |
| Email | Delivery reported | No inbox screenshot recovered |

