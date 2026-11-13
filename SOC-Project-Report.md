# SOC Investigation Report — Controlled Mimikatz Execution Detection

**Project:** AWS-Based SOC Automation Lab  
**Analyst:** Aryan Baloria  
**Evidence window:** 12–13 November 2026  
**Environment:** Isolated Windows 11 Pro VM, Sysmon, Wazuh, Shuffle, VirusTotal, TheHive

## Executive summary

A controlled process-execution test on a Windows 11 Pro lab endpoint generated Sysmon Event ID 1 for `mimikatz.exe`. The Wazuh Manager matched custom rule `100002` at Level `12`. Shuffle extracted the file's SHA-256, retrieved a VirusTotal file report, created a TheHive alert through its HTTP API and sent an email notification.

The successful Shuffle debug execution was marked `FINISHED`. Its recorded outputs were: SHA256 extraction `success=true` / `found=true`, VirusTotal HTTP `200`, TheHive HTTP `201`, and Email `success=true`. The Wazuh alert and TheHive response share the same source reference, enabling correlation across the pipeline.

This report documents process execution and enrichment only. It does not claim successful credential dumping or credential compromise.

## 1. Scope and environment

- Endpoint: Windows 11 Pro virtual machine in VirtualBox.
- Sysmon: version `15.22` recorded in the installation output; uploaded configuration schema `4.91`.
- Wazuh Agent: `wazah-agent-windows`, ID `001`, version `4.14.10`.
- Wazuh Manager and Indexer: `4.14.10`, Ubuntu Server 24.04 on AWS EC2.
- Filebeat: `7.10.2`.
- SOAR: Shuffle Cloud.
- Threat intelligence: VirusTotal v3.
- Alert management: TheHive `5.8.1-1`.

## 2. Detection logic

The deployed Wazuh rule was recovered from `/var/ossec/etc/rules/local_rules.xml`:

```xml
<rule id="100002" level="12">
  <if_group>sysmon_event1</if_group>
  <field name="win.eventdata.originalFileName" type="pcre2">(?i)mimikatz\.exe</field>
  <description>SOC-Lab: Mimikatz execution detected on Windows endpoint.</description>
  <mitre>
    <id>T1003</id>
  </mitre>
  <group>sysmon,mimikatz,credential_access,</group>
</rule>
```

The surrounding group is retained in `detections/wazuh_mimikatz_rule.xml`. The manager syntax test `sudo /var/ossec/bin/wazuh-analysisd -t` returned exit code `0`.

The Wazuh rule maps to `T1003` (OS Credential Dumping). The Sysmon event's `RuleName` field contained `T1036` (Masquerading), originating from the Sysmon configuration metadata. These are distinct mappings from distinct sources.

## 3. Endpoint event and Wazuh alert

Sysmon recorded Process Create (Event ID `1`) at `2026-11-12 23:31:56.986 UTC`, process ID `2692`, image `mimikatz.exe`, file version `2.2.0.0`, and PowerShell as parent process. The matching Windows event record ID was `329512`.

Wazuh created alert ID `1794526316.3902037` at `2026-11-12T23:31:56.300+0000`. The alert contained rule `100002`, Level `12`, `firedtimes: 4`, agent `wazah-agent-windows`, and the same SHA-256 hash. The sanitized event is provided in `samples/wazuh-alert-100002.json`.

**SHA-256:**

```text
61C0810A23580CF492A6BA4F7654566108331E7A4134C968C2D6A05261B2D8A1
```

## 4. Threat-intelligence enrichment

Shuffle extracted the SHA-256 using:

```regex
SHA256=([0-9A-Fa-f]{64})
```

VirusTotal returned HTTP `200` and a file report. The captured summary identifies `mimikatz.exe` as a Win32 EXE, version `2.2.0.0`, size `1,355,264` bytes, reputation `-118`, and includes five sandbox verdicts categorized as malicious. The suggested threat label was `trojan.mimikatz/stealer`; the popular classifications included `trojan`, `hacktool`, and `pua`.

The Shuffle output exposed 75 antivirus engine results, but the aggregate `last_analysis_stats` values were collapsed in the copied output. Therefore, this report does not state an antivirus detection ratio. VirusTotal HTTP `200` means the report was retrieved; the reputation and sandbox classifications are separate enrichment fields.

See `samples/virustotal-report-summary.json` and the original text capture at `samples/virustotal-node-output-capture.txt`.

## 5. TheHive alert creation

The generic Shuffle HTTP action issued a POST to the TheHive alert endpoint. The observed response was HTTP `201 Created`. The returned alert had ID `~1192048`, type `wazuh-alert`, source `Wazuh`, source reference `1794526316.3902037`, severity label `HIGH`, TLP `AMBER`, PAP `AMBER`, and status `New`.

The response reported `observableCount: 0`; no observables were attached to this alert. The workflow created a TheHive **alert**, not a TheHive case.

## 6. Email notification and workflow execution

The final saved graph shows VirusTotal branching independently to the HTTP action and Email action. The recorded Shuffle Debug run was `FINISHED` from `05:01:57` to `05:02:01` local time on 13 November 2026. The Email node returned `success: true` with reason `Email was successfully sent`.

The email body contained the Wazuh title, agent, rule ID, severity field, process details, file hashes and VirusTotal link. Downstream severity values are recorded as emitted by their respective systems; this report does not remap or reconcile severity scales.

## 7. Correlation timeline

| Time | System | Observation |
|---|---|---|
| 2026-11-12 23:31:56.986 UTC | Sysmon | Event ID 1 Process Create for `mimikatz.exe`; record ID `329512` |
| 2026-11-12 23:31:56.300 UTC | Wazuh | Rule `100002`, Level `12`; alert ID `1794526316.3902037` |
| 2026-11-12 23:31:59 UTC | VirusTotal | File-hash report returned HTTP `200` |
| 2026-11-12 23:32:00.753 UTC | TheHive | Alert object date created |
| 2026-11-12 23:32:00.778 UTC | TheHive | Alert object `_createdAt`; HTTP response `201` |
| 2026-11-12 23:32:01 UTC | Shuffle | Debug run finished; Email action reported success |

The event/Wazuh timestamps are from separate ingestion/recording layers and differ by fractions of a second. The matching event record ID, process ID, hash and source reference are the main correlation keys.

## 8. Security review and limitations

- TheHive security group showed TCP `9000` open to `0.0.0.0/0` in the 13 November 2026 screenshot.
- The Wazuh security group showed TCP `1514` and `1515` open to `0.0.0.0/0` in the same review.
- SSH rules were restricted to specific `/32` source addresses; HTTPS `443` on the Wazuh group was similarly restricted.
- TheTheHive API URL in the workflow used HTTP, not HTTPS.
- The saved Shuffle webhook trigger was configured as an unauthenticated POST endpoint.
- The raw Shuffle export contained an API credential, webhook URL, email address and public endpoint. These values are redacted in the public export. Sanitization does not invalidate any credential that may already have been exposed.
- The workflow created an alert but attached no observables and did not create a case.
- Automated endpoint isolation, process termination and remediation were not implemented.
- The uploaded workflow export's later validation metadata showed `valid: false` and an HTTP `400` issue for HTTP 1. This is distinct from the earlier successful execution returning HTTP `201`; the exact cause of the later validation error is not established by the export metadata alone.
- Production-grade retries, idempotency and explicit error handling were not implemented or fully validated.

## 9. Final assessment

The lab demonstrated an end-to-end SOC pipeline from endpoint telemetry through SIEM detection, hash extraction, threat-intelligence enrichment, TheHive alert creation and email notification. The alert is well correlated across Sysmon, Wazuh, Shuffle and TheHive using the process details, SHA-256 and source reference.

The project is suitable as a controlled SOC engineering case study. The main improvement areas are exposure reduction, transport encryption, webhook authentication, deduplication/error handling, and adding analyst-approved incident response actions. No claim of credential theft or production compromise is made.
