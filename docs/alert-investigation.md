# Alert investigation

The complete analyst-style incident report is maintained in [SOC Investigation Report](soc-incident-report.md).

Key correlation artifacts:

- Sysmon Event ID 1 / Windows event record ID `329512`.
- Wazuh alert ID `1794526316.3902037`, rule `100002`, Level `12`.
- SHA-256 `61C0810A23580CF492A6BA4F7654566108331E7A4134C968C2D6A05261B2D8A1`.
- TheHive alert ID `~1192048`, source reference `1794526316.3902037`.
- Shuffle execution summary in `../samples/shuffle-execution-summary.json`.

The evidence confirms a controlled process-execution event and successful alert automation. It does not prove successful credential dumping or credential compromise.
