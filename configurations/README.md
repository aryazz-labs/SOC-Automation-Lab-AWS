# Configuration artifacts

- `windows-agent-ossec.conf.example` is a focused, sanitized excerpt of the deployed Windows agent configuration. The live manager address is replaced with `[WAZUH_MANAGER_ADDRESS]`.
- `ossec.conf.example` is the minimal Sysmon Operational event-channel collection block.
- `sysmonconfig.xml` is the actual XML file uploaded from the Windows lab. Its hash, schema version, installation history, and upstream attribution are recorded in `sysmonconfig-source.md`.

The Sysmon configuration is derived from a third-party Sysmon Modular configuration. See `../licenses/SYSMON-MODULAR-license.md` before redistributing or modifying it. The exact source commit/release is not pinned in the uploaded XML.
