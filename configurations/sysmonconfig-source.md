# Sysmon configuration provenance

- File: `sysmonconfig.xml`
- XML schema version: `4.91`
- SHA-256: `64bec37ba849c5f2934a55d2a29e7ddfae46a30d56f7f2908d1f8a454898797e`
- XML parser validation: passed
- Installed Sysmon version recorded in the PowerShell installation output: `15.22`
- Installation command recorded in project history: `Sysmon64.exe -accepteula -i .\sysmonconfig.xml`
- Source attribution embedded in the XML: modified Sysmon Modular configuration; Olaf Hartong and contributors
- Upstream project: https://github.com/olafhartong/sysmon-modular
- Upstream license: https://github.com/olafhartong/sysmon-modular/blob/master/license.md

The user confirmed this was the filename supplied to the initial Sysmon installation command. No later `Sysmon64.exe -c` update command was found in the project chat history. The uploaded file is therefore documented as the initial install configuration, but byte-for-byte identity with the historical installed file cannot be independently proven because no historical checksum was recorded.

The XML header does not pin an upstream commit or release. Keep the upstream attribution and license notice when redistributing this file.
