# Threat-intelligence note: Mimikatz sample hash

## Hash identity

- File: `mimikatz.exe`
- File version: `2.2.0.0`
- Type: `Win32 EXE`
- Size: `1,355,264` bytes
- SHA-256: `61C0810A23580CF492A6BA4F7654566108331E7A4134C968C2D6A05261B2D8A1`
- VirusTotal API status: `200`
- VirusTotal reputation field: `-118`
- Suggested threat label: `trojan.mimikatz/stealer`

## Captured enrichment

The copied VirusTotal output showed five sandbox verdict providers (Zenbox, C2AE, Yomi Hunter, VMRay and CAPE Sandbox) categorizing the file as malicious and naming Mimikatz. Community votes in the copied response were 16 malicious and 6 harmless. The output showed 75 engine result entries, but the aggregate analysis statistics were collapsed in the text copy; this report does not infer a detection ratio.

## Assessment

The enrichment is consistent with the tested file being a Mimikatz credential-access utility / hacktool. The report's endpoint evidence confirms process execution only. It does not establish that the tool successfully extracted credentials or that credentials were compromised.

## Source and limitations

This note summarizes the actual VirusTotal node output captured during the Shuffle run. See `../samples/virustotal-node-output-capture.txt` for the text capture and `../samples/virustotal-report-summary.json` for a normalized subset. VirusTotal report contents can change over time; these values are a point-in-time capture from the project execution on 12 November 2026 UTC.
