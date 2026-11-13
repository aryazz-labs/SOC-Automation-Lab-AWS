# AWS and workflow security review

**Evidence date:** 13 November 2026 (screenshots captured around 18:46–18:47 Asia/Kolkata).  
**Scope:** Inbound rules shown in the supplied Wazuh and TheHive security-group screenshots, plus the actual Shuffle workflow export.

## Observed inbound rules

### TheHive security group (`Hive-sg`)

| Port | Protocol | Source | Observation |
|---|---|---|---|
| 22 | TCP | Administrator `/32` source | Restricted |
| 9000 | TCP | `0.0.0.0/0` | Exposed to all IPv4 sources |

### Wazuh security group (`Wazuh-sg`)

| Port | Protocol | Source | Observation |
|---|---|---|---|
| 1514 | TCP | `0.0.0.0/0` | Exposed to all IPv4 sources |
| 1515 | TCP | `0.0.0.0/0` | Exposed to all IPv4 sources |
| 22 | TCP | Administrator `/32` source | Restricted |
| 443 | TCP | Administrator `/32` source | Restricted |

These are the rules visible in the provided screenshots at capture time; they are not a statement about the current AWS configuration after that moment.

## Workflow security observations

- The uploaded Shuffle workflow contains an unauthenticated HTTP POST webhook trigger.
- The HTTP action targets TheHive over HTTP on port `9000`, so the observed request does not provide TLS transport encryption.
- The raw workflow export included sensitive values; only a sanitized reference copy is included in this repository.
- The original export's credential values are not repeated here. Sanitization of this repository copy does not revoke or rotate any live credential.

## Risks to document

1. Internet-wide inbound access to TheHive TCP `9000` increases exposure of the alert-management API/UI service.
2. Internet-wide inbound access to Wazuh TCP `1514` and `1515` increases the exposed attack surface for agent traffic and enrollment.
3. Unauthenticated webhook invocation can allow unwanted workflow executions if the URL is discovered or leaked.
4. HTTP between Shuffle and TheHive does not encrypt credentials or alert contents in transit.

The project owner chose not to change live settings during evidence collection. This report records the observed posture without altering the environment.
