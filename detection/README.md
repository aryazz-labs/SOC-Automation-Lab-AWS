# Wazuh Detection

## Custom rule summary

| Field | Observed value |
|---|---|
| Rule ID | `100002` |
| Alert level | `12` |
| Description | `SOC-Lab: Mimikatz execution detected on Windows endpoint.` |
| Event source | Sysmon |
| Event ID | `1` (Process Create) |
| Process observed | `mimikatz.exe` |
| Parent process observed | Windows PowerShell |

The rule was validated during a controlled process-execution test in the isolated Windows lab VM. The test confirms detection of process execution; it does not prove successful credential dumping.

## ATT&CK mapping status

An earlier project note recorded T1036 (Masquerading), but the exact rule XML was not recovered and the mapping could not be independently verified. This repository therefore leaves the mapping unconfirmed. Check the actual `<mitre>` section in the deployed rule before adding a technique.

## Exact rule XML

The exact XML contents of rule `100002` were not present in the recovered files. The live manager export is still required. Do not recreate the rule from a generic example and label it as the deployed rule. When recovered, save the sanitized exact rule as `wazuh_mimikatz_rule.xml` in this directory.
