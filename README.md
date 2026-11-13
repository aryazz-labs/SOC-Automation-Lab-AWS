
# SOC Automation Lab — Architecture

The diagram below shows the end-to-end flow from Windows endpoint monitoring to automated threat intelligence enrichment, TheHive alert creation, and email notification.

```mermaid
flowchart TD
  subgraph Endpoint["Endpoint layer"]
    A["Windows 11 Pro VM"] --> B["Sysmon"]
    B --> C["Wazuh Agent"]
  end

  subgraph SIEM["SIEM layer - AWS EC2"]
    D["Wazuh Manager"] --> E["Wazuh Indexer"]
  end

  subgraph Automation["Shuffle Cloud"]
    F["Webhook"] --> G["Shuffle Tools: SHA256 extraction"]
    G --> H["VirusTotal v3"]
    H --> I["HTTP action: TheHive API"]
    H --> J["Email notification"]
  end

  C --> D
  D --> F
  I --> K["TheHive alert"]
```
