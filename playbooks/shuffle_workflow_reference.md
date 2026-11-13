# Shuffle workflow reference

**Workflow:** `SOC-Automation-Workflow`  
**Platform:** Shuffle Cloud

## Nodes and branch layout

| Node | Function | Recorded result in the captured run |
|---|---|---|
| Webhook 1 | Receives Wazuh alert via POST | Run source shown as `webhook` |
| Shuffle Tools 1 | Extracts SHA-256 from hash string | `success: true`, `found: true` |
| VirusTotal v3 1 | Retrieves VirusTotal file report | HTTP `200`, `success: true` |
| HTTP 1 | POSTs alert to TheHive API | HTTP `201`, `success: true` |
| Email 1 | Sends analyst email | `success: true`; reason `Email was successfully sent` |

VirusTotal branches independently to HTTP and Email. The workflow does not chain Email after TheHive.

## SHA-256 extraction

```regex
SHA256=([0-9A-Fa-f]{64})
```

The regex input is `$exec.text.win.eventdata.hashes`; the captured group is passed into the VirusTotal file lookup.

## TheHive API operation

The actual request was a generic HTTP POST to `/api/v1/alert` on TheHive port `9000`. The raw host, authorization header and credentials are redacted in the public export. The observed response status was `201`; the returned alert ID was `~1192048` and its `sourceRef` matched Wazuh alert ID `1794526316.3902037`.

## Email action

The Email action body used Shuffle variables for Wazuh title, agent name, rule ID, severity, process path, command line, parent image, hashes and VirusTotal URL. The recipient is redacted in the published workflow export. The captured execution output reported a successful send.

## Export note

`SOC-Automation-Workflow.sanitized.json` preserves the actual workflow structure and node settings where safe, but replaces the webhook URL, API key, authorization header, TheHive host, recipient address and account identifiers. It is a documentation copy, not an import-ready backup.

The uploaded export's later validation metadata shows one HTTP 400 issue on HTTP 1. The successful historical Debug run is separately evidenced as FINISHED with HTTP 201. Do not conflate those two records.
