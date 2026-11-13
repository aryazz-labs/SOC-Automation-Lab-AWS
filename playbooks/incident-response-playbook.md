# Incident response playbook: suspicious credential-access tool execution

This is an analyst-guided playbook for the lab detection. It does not implement automated response.

## 1. Validate the alert

- Confirm Wazuh rule ID `100002` and the observed Level `12`.
- Confirm the event is Sysmon Event ID `1` and inspect `OriginalFileName`, image, command line, parent image, user context and integrity level.
- Confirm the endpoint/agent identity and correlate event timestamps across Sysmon and Wazuh.
- Distinguish tool execution from proof of successful credential access.

## 2. Preserve evidence

- Export the Wazuh alert JSON.
- Preserve the original Sysmon event and relevant adjacent events.
- Record SHA-256 and any additional hashes.
- Preserve process/parent process details and timestamps.
- Record the Shuffle execution ID/output and TheHive alert ID.

## 3. Enrich

- Query the SHA-256 in VirusTotal.
- Record the query time, report identity, reputation, sandbox results and any visible analysis statistics.
- Treat third-party reputation as one source of evidence, not a final verdict.

## 4. Escalate and contain

- Determine whether the process execution was authorized.
- Review adjacent logon activity, LSASS access telemetry, process events and network connections.
- Escalate to the incident-response owner if unauthorized execution or corroborating credential-access behavior is identified.
- Endpoint isolation, process termination, password resets and credential revocation require an approved response decision; none are automated in this lab.

## 5. Close and document

- Record the disposition, rationale, evidence references and any containment approvals.
- Close the TheHive alert only after the investigation outcome is documented.
- Create a case only when the incident process requires one; the current workflow creates an alert, not a case.
