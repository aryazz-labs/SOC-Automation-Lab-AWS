# Detection test cases

| Test | Status | Evidence / note |
|---|---|---|
| Controlled `mimikatz.exe` Process Create event | **Executed** | Sysmon Event ID 1 and Wazuh rule `100002` Level `12` captured; see sanitized alert JSON and screenshots. |
| Wazuh rule syntax test (`wazuh-analysisd -t`) | **Passed** | Exit code `0`. |
| Benign unrelated process (for example, Notepad) | **Not recorded** | No negative-test output was supplied; do not claim a pass. |
| Similar process name / false-positive check | **Not recorded** | Needs a separate controlled test if detection tuning is extended. |
| Missing or malformed hash passed to Shuffle | **Not recorded** | Error-handling behavior is not asserted. |

Only tests explicitly marked Executed/Passed are reported as completed. Any future negative tests should record the event, expected result, observed result, and screenshot or JSON evidence.
