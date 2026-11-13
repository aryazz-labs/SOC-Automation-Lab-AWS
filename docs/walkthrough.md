# Implementation walkthrough

## 1. Endpoint and Sysmon

A Windows 11 Pro VM ran in VirtualBox. The project history records Sysmon version `15.22` installed from an elevated PowerShell session using:

```powershell
.\Sysmon64.exe -accepteula -i .\sysmonconfig.xml
```

The uploaded `configurations/sysmonconfig.xml` has XML schema version `4.91`. Its header identifies it as a modified Sysmon Modular configuration. No later `Sysmon64.exe -c` command was found in the project chat history; byte-for-byte equality with the historical file cannot be independently verified because no installation-time checksum was recorded.

## 2. Wazuh Agent collection

The Windows Wazuh agent was named `wazah-agent-windows`, ID `001`, version `4.14.10`. Its deployed configuration collected Application, Security, System and Sysmon Operational event channels. The Sysmon collection block was:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

The sanitized excerpt is in `configurations/windows-agent-ossec.conf.example`.

## 3. Wazuh Manager and detection

The AWS-hosted Wazuh Manager and Indexer were version `4.14.10` on Ubuntu Server 24.04; Filebeat was `7.10.2`. The custom rule `100002` matched `win.eventdata.originalFileName` against `(?i)mimikatz\.exe` under `sysmon_event1`, at Level `12`, with Wazuh ATT&CK mapping `T1003`.

`wazuh-analysisd -t` returned exit code `0`. The actual rule is included as `detections/wazuh_mimikatz_rule.xml`.

## 4. Controlled process event

The captured Sysmon Event ID 1 described `mimikatz.exe`, file version `2.2.0.0`, with PowerShell as its parent. Its `eventdata.ruleName` metadata identified `T1036` (Masquerading), which is distinct from the Wazuh rule's `T1003` mapping.

The Wazuh alert JSON sample links this process event to alert ID `1794526316.3902037` and shows rule `100002`, Level `12`, and `firedtimes: 4`.

## 5. Shuffle workflow

The saved workflow `SOC-Automation-Workflow` contains a Webhook trigger and four actions:

1. Shuffle Tools: regex `SHA256=([0-9A-Fa-f]{64})`.
2. VirusTotal v3: file hash lookup.
3. HTTP: POST to TheHive alert API.
4. Email: send notification.

The actual workflow export is included only as a sanitized, non-importable reference at `playbooks/SOC-Automation-Workflow.sanitized.json`. The raw export is not part of the public package because it contained secrets and private endpoints.

## 6. Recorded successful execution

A Shuffle Debug run was recorded as `FINISHED` from `05:01:57` to `05:02:01` local time on 13 November 2026. The node outputs showed regex extraction success, VirusTotal HTTP `200`, TheHive HTTP `201`, and Email success. A transcription is in `samples/shuffle-execution-summary.json` and screenshot evidence is in `screenshots/10` through `screenshots/13`.

The workflow export's later validation metadata separately recorded `valid: false` with a status `400` issue on HTTP 1. This later validation record is preserved in `samples/shuffle-export-validation.json` and is not presented as the same event as the earlier successful run.
