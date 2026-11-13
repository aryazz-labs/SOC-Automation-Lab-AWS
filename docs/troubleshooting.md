# Troubleshooting Notes

This file records issues encountered while building the lab and the resolutions or workarounds that were used.

## 1. Sysmon events visible locally but missing from Wazuh

**Symptom:** Sysmon events appeared in Windows Event Viewer but were not visible in Wazuh.

**Cause:** The Wazuh agent configuration was not collecting the Sysmon Operational event channel.

**Resolution:** Added the following `localfile` entry to the Windows agent's `ossec.conf`, then restarted the Wazuh agent service:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

## 2. Wazuh agent version mismatch

**Symptom:** The Windows agent log reported that the agent version must be lower than or equal to the manager version.

**Context:** During the lab upgrade, the Wazuh manager was upgraded from 4.9.2 to 4.14.10. The endpoint agent also needed to be on a compatible version.

**Resolution:** Updated the agent to a version compatible with the manager and verified that the endpoint subsequently appeared active.

## 3. Wazuh dashboard became unavailable after an EC2 restart

**Symptom:** The dashboard stopped loading after the server was restarted.

**Cause:** The EC2 public IP address had changed, leaving the endpoint agent configured with the previous manager address.

**Resolution:** Updated the Wazuh manager address in the Windows agent configuration and restarted the agent. Also verified the relevant AWS security-group rules.

## 4. TheHive startup memory issue

**Symptom:** TheHive startup reported:

```text
java.lang.OutOfMemoryError: Metaspace
```

**Resolution:** Increased the configured `MaxMetaspaceSize` to 512 MB and restarted the service. Validate memory sizing against the actual instance resources before using this configuration elsewhere.

## 5. Shuffle native TheHive action parameter error

**Symptom:** The native TheHive action returned:

```text
unexpected keyword argument 'source'
```

**Resolution:** Replaced that action with a generic HTTP request to TheHive's REST API. The generic HTTP action successfully created an alert and returned HTTP 201.

## 6. Shuffle-to-TheHive connectivity

**Symptom:** Initial requests from Shuffle to TheHive timed out.

**Checks performed:** Confirmed that TheHive was listening on port 9000 and reviewed network connectivity and cloud security-group access.

**Final result:** A later generic HTTP integration test successfully created the TheHive alert (HTTP 201). Avoid publishing the server's public address or credentials.

## 7. Regex and output mapping in Shuffle

**Purpose:** Extract the SHA256 value from the Wazuh alert's hash string.

**Expression used:**

```regex
SHA256=([0-9A-Fa-f]{64})
```

**Mapping:** The extracted capture group was passed as the input to the VirusTotal file lookup. Ensure the downstream action receives the hash string itself rather than an entire array or object.

## Operational security reminders

- Do not commit API keys, passwords, private keys, session cookies, or authorization headers.
- Redact public IPs and personal email addresses from screenshots where appropriate.
- Restrict TheHive port 9000 to trusted sources and use HTTPS.
- A VirusTotal result is one source of enrichment, not a standalone verdict.


## 8. VirusTotal URL and hash mapping

**Symptom:** Early VirusTotal requests returned HTTP `404` (`NotFoundError`, `Resource not found`).

**Investigation:** Reviewed the generated request URL and the value passed from the regex node. The request was receiving a malformed value rather than the extracted SHA256 string.

**Resolution:** Corrected the regex output mapping so the VirusTotal action received the captured 64-character hash. A later recovered node result shows HTTP `200` and a file object.

## 9. Duplicate TheHive alert source reference

**Symptom:** A repeated alert-creation request returned HTTP `400` with a `CreateError` indicating that the alert source reference already existed in the organization.

**Interpretation:** The source-reference uniqueness check prevented creation of a duplicate alert. This is not evidence that the original alert was lost. Production workflows should implement an explicit duplicate check or idempotency strategy and handle this response intentionally.
