# Troubleshooting notes

This document records issues and workarounds from the project build. Only recorded outcomes are stated.

## 1. Sysmon events visible locally but missing from Wazuh

**Symptom:** Sysmon events were visible in Windows Event Viewer but did not appear in Wazuh Threat Hunting.

**Resolution:** Added a `localfile` block for `Microsoft-Windows-Sysmon/Operational` with `eventchannel` log format to the Windows agent `ossec.conf`, then restarted the agent service.

## 2. Wazuh agent/manager version mismatch

**Symptom:** The Windows agent log reported that the agent version must be lower than or equal to the manager version.

**Context:** The manager was upgraded from Wazuh `4.9.2` to `4.14.10`; the Windows agent was updated to `4.14.10` and subsequently appeared Active.

## 3. Wazuh dashboard address changed after EC2 restart

**Symptom:** The dashboard/agent connection stopped working after the Wazuh EC2 instance was restarted.

**Cause recorded in project work:** The public EC2 address changed.

**Resolution:** Updated the Windows agent's manager address and restarted the service. Avoid embedding the live address in public files.

## 4. TheHive startup Metaspace issue

**Symptom:** TheHive reported `java.lang.OutOfMemoryError: Metaspace` during startup.

**Recorded workaround:** Increased configured `MaxMetaspaceSize` to `512 MB` and restarted the service. Validate memory sizing against actual instance resources before reusing.

## 5. Native Shuffle TheHive action compatibility

**Symptom:** The native TheHive action returned `unexpected keyword argument 'source'`.

**Workaround:** Used a generic Shuffle HTTP POST action to TheHive's alert API. A successful creation returned HTTP `201`.

## 6. Initial Shuffle-to-TheHive timeouts

**Symptom:** Initial requests from Shuffle to TheHive timed out.

**Recorded checks:** TheHive listener and TCP port `9000`, network reachability and AWS security-group rules were reviewed. A later HTTP action successfully created an alert.

## 7. VirusTotal HTTP 404 from malformed mapping

**Symptom:** Earlier VirusTotal requests returned HTTP `404`.

**Recorded cause:** The action received a malformed value rather than the extracted SHA-256 string.

**Workaround:** Corrected the regex output mapping so VirusTotal received the 64-character hash. A later request returned HTTP `200`.

## 8. Duplicate TheHive source reference

**Symptom:** A repeated create request returned HTTP `400` with a `CreateError` indicating that the source reference already existed.

**Interpretation:** TheHive rejected a duplicate alert source reference. The original alert was not thereby shown to be lost. Production workflows should implement an explicit duplicate check/idempotency strategy.

## 9. Wazuh alert file rotation

**Symptom:** Searching only `/var/ossec/logs/alerts/alerts.json` did not return the captured rule `100002` event.

**Resolution:** Searched rotated compressed alert logs with `zgrep`. The matching alert was found in a compressed archive and exported to a sanitized JSON sample.
