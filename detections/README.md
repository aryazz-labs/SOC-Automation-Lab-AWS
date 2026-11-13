# Wazuh detection: Mimikatz process execution

## Deployed rule details

| Field | Observed value |
|---|---|
| Rule ID | `100002` |
| Level | `12` |
| Description | `SOC-Lab: Mimikatz execution detected on Windows endpoint.` |
| Parent event group | `sysmon_event1` |
| Match field | `win.eventdata.originalFileName` |
| Match expression | `(?i)mimikatz\.exe` |
| Wazuh ATT&CK mapping | `T1003` — OS Credential Dumping (Credential Access) |
| Event | Sysmon Event ID `1` — Process Create |

The XML in `wazuh_mimikatz_rule.xml` is transcribed from the actual `/var/ossec/etc/rules/local_rules.xml` output provided by the project owner. The containing `<group>` is included.

## Analyst interpretation

The rule detects a process-creation event whose `OriginalFileName` field matches `mimikatz.exe`, case-insensitively, under the `sysmon_event1` group. It establishes that the process was observed; it does not establish successful credential dumping or credential compromise.

The Sysmon event's own `eventdata.ruleName` field contained `technique_id=T1036,technique_name=Masquerading`. That is metadata from the Sysmon configuration and is distinct from the Wazuh rule's `<mitre><id>T1003</id></mitre>` mapping. Both are preserved as observed, not conflated.

See `rule-validation.md` for the recorded syntax test and alert evidence, and `test-cases.md` for verified vs. untested cases.
