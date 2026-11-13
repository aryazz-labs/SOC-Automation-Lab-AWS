# Rule validation record

## Configuration validation

Command executed on Wazuh Manager:

```bash
sudo /var/ossec/bin/wazuh-analysisd -t
echo "Exit code: $?"
```

Observed result: `Exit code: 0`. Wazuh printed an informational rule-tree memory threshold message and no configuration error in the captured output.

## Positive detection evidence

A rotated Wazuh alert record was recovered from the compressed alert archive. The sanitized sample is `../samples/wazuh-alert-100002.json`.

| Field | Recorded value |
|---|---|
| Alert timestamp | `2026-11-12T23:31:56.300+0000` |
| Alert ID | `1794526316.3902037` |
| Rule ID / level | `100002` / `12` |
| `firedtimes` | `4` |
| Agent | `wazah-agent-windows` (`001`) |
| Event | Sysmon Event ID `1` |
| Process ID | `2692` |
| File version | `2.2.0.0` |
| SHA-256 | `61C0810A23580CF492A6BA4F7654566108331E7A4134C968C2D6A05261B2D8A1` |
| Wazuh ATT&CK mapping | `T1003` |
| Sysmon RuleName metadata | `T1036` |

The corresponding Event Viewer screenshot is `../screenshots/08-sysmon-event-id-1.png`; the Wazuh dashboard view is `../screenshots/09-wazuh-rule-100002-alert.png`.

## Evidence caveats

- `firedtimes: 4` is the counter in the captured alert at that time; it is not a count of four unique malicious outcomes.
- The event confirms process creation, not successful credential dumping.
- The Wazuh dashboard screenshot's time display is local time; the JSON timestamp is UTC.
