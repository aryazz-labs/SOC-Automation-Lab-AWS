# Shuffle Workflow Reference

**Workflow name:** `SOC-Automation-Workflow`  
**Platform:** Shuffle Cloud

## Recovered node sequence and branches

| Order | Node | Role |
|---:|---|---|
| 1 | Webhook 1 | Receives the selected Wazuh alert |
| 2 | Shuffle Tools 1 | Extracts the SHA256 hash |
| 3 | VirusTotal v3 1 | Queries the VirusTotal file report |
| 4a | HTTP 1 | Sends a structured alert to TheHive through its REST API |
| 4b | Email 1 | Sends an analyst notification |

The recovered final canvas shows `HTTP 1` and `Email 1` as separate outgoing branches from `VirusTotal v3 1`. Do not document Email as dependent on the TheHive HTTP node.

## Data extraction

Regular expression:

```regex
SHA256=([0-9A-Fa-f]{64})
```

A recovered Shuffle Tools result shows `SUCCESS`, `found: true`, and the captured SHA256 value.

## VirusTotal request

The VirusTotal action queried the extracted SHA256 using the v3 file-report endpoint. A recovered node result shows HTTP `200` and a file object. Earlier troubleshooting screenshots show malformed requests returning HTTP `404`; the successful result was captured after correcting the hash mapping.

## TheHive request

The generic HTTP action used an HTTP POST to:

```text
http://<THEHIVE_HOST>:9000/api/v1/alert
```

The action used a TheHive API key. Keep credentials in Shuffle's credential storage; do not publish raw authorization headers, API keys, or webhook URLs. The project notes record HTTP `201` on successful creation and the recovered TheHive screenshot shows the resulting alert.

The request creates a TheHive **alert**. A TheHive case is a separate object and is not claimed here.

## Email notification

The email action is a separate branch from the VirusTotal node in the recovered final workflow canvas. Email delivery was reported by the project owner, but no inbox screenshot was recovered.
