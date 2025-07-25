# Suricata 
 
Suricata is a powerful open-source Network Security Monitoring (NSM), Intrusion Detection System (IDS), and Intrusion Prevention System (IPS) engine. Developed by the Open Information Security Foundation (OISF), it is capable of deep packet inspection (DPI), TLS handshake analysis, protocol parsing, file extraction, and more—all at high performance.
 
---
 
## Installation
 
- **Linux:**
  ```bash
  sudo apt install suricata
  ```
- **From Source:**
  - [https://suricata.io/download/](https://suricata.io/download/)
 
---
 
## Architecture Overview
 
- **Packet Acquisition**: libpcap, PF_RING, AF_PACKET, DPDK, Netmap
- **Decode**: Parses L2–L7 headers
- **Stream Engine**: Reassembles TCP streams
- **Detection Engine**: Processes rules (Snort-compatible + Suricata-unique keywords)
- **Logging & Output**: JSON, EVE format, PCAP, file extraction
- **Multi-threaded**: Can leverage all CPU cores
 
---
 
## Modes of Operation
 
- **IDS** (default): Passive monitoring
  ```bash
  suricata -c /etc/suricata/suricata.yaml -i eth0
  ```
- **IPS/Inline**: Active blocking via NFQUEUE
- **NSM Mode**: Full metadata + file logging + protocol parsers
 
---
 
## Configuration File (`suricata.yaml`)
 
Key sections:
- `vars`: Define `HOME_NET`, `EXTERNAL_NET`, `DNS_SERVERS`, etc.
- `af-packet` or `pfring`: Interface acquisition method
- `outputs`: Enable/disable EVE JSON, file-store, PCAP
- `logging`: Configure log levels, rotation, etc.
- `rule-files`: Include paths to rulesets
 
---
 
## Rule Syntax (Snort-Compatible + Extended)
 
### Basic Structure:
```
action proto src_ip src_port -> dst_ip dst_port (options)
```
 
### Key Keywords:
- `alert`, `drop`, `reject`, `pass`
- `msg`, `sid`, `rev`, `classtype`
- `content`, `nocase`, `offset`, `depth`
- `http_uri`, `http_header`, `http_method`, `tls_sni`, `dns_query`
- `flowbits`, `xbits`, `threshold`, `iprep` (IP reputation)
- `filemagic`, `filestore`, `filemd5`, `filesize`
 
---
 
## Output Formats
 
Suricata's strength lies in its rich logging:
 
### EVE JSON Log (default):
Path: `/var/log/suricata/eve.json`
 
Events include:
- `alert`: IDS alerts
- `dns`: Parsed DNS queries/responses
- `http`: Parsed HTTP metadata
- `flow`: Flow records
- `tls`: TLS handshake metadata
- `fileinfo`: File transfer metadata
- `ssh`, `smtp`, `nfs`, `smb`, `krb5`, etc.
 
### Other Logs:
- `fast.log`: Human-readable alerts
- `stats.log`: Performance and event stats
- `pcap.log`: Captured packets
- `files-json.log`: File metadata
 
---
 
## Protocol Detection & Parsing
 
Suricata automatically detects and parses:
- HTTP/S
- DNS
- TLS/SSL
- SSH, FTP, SMB, NFS
- SMTP, IMAP, POP3
- DHCP
- Modbus, DNP3, and other ICS protocols
 
Parsed fields are used for matching in rules and logging.
 
---
 
## Advanced Features
 
### Multi-threading:
- One of Suricata's standout features
- Each core handles a stream of packets for high throughput
 
### File Extraction:
- Automatically extracts and logs files transferred over HTTP, FTP, SMTP, SMB
- Use `filestore` keyword and enable file logging in `suricata.yaml`
 
### Lua Scripting:
- Extend Suricata with Lua scripts for custom detections or protocol handling
 
### IP Reputation Lists:
- Configure external lists (e.g. abuse.ch) for blocking or alerting
 
### TLS Analysis:
- Extracts SNI, certificate, version, JA3/JA3S fingerprints
- Helps identify malicious encrypted traffic
 
---
 
## Common Use Cases
 
| Use Case                     | Feature(s) Used                           |
|-----------------------------|-------------------------------------------|
| Detect brute-force logins   | `flow`, `threshold`, `http_user_agent`    |
| DNS tunneling detection     | `dns_query`, entropy, size, frequency     |
| Malware download via HTTP   | `http_uri`, `content`, `filemagic`        |
| Suspicious SNI (TLS)        | `tls_sni`, JA3 fingerprinting             |
| SMB/FTP file exfiltration   | `filestore`, `fileinfo`, `filesize`       |
| Detect beaconing behavior   | `flow`, `flowbits`, `threshold`           |
| Threat intel correlation    | `iprep`, Suricata-Update feeds            |
 
---
 
## Integration
 
- **SIEMs**: Elastic, Splunk, QRadar via EVE JSON logs
- **Logstash**: Parses EVE JSON into Elasticsearch
- **Security Onion**: Seamless integration for NSM
- **TheHive/Cortex**: Use alerts for case enrichment
 
---
 
## Updating Rules
 
Use **Suricata-Update** to manage and fetch rulesets:
```bash
suricata-update
```
Sources:
- Emerging Threats Open (ET)
- Custom rules
- Threat intel feeds
 
---
 
## Tools & Utilities
 
- `suricatactl`: Suricata command helper (start, stats, reload)
- `evebox`: EVE log web UI & alert triage platform
- `jq`: Parse and manipulate EVE logs from CLI
- `suricata-verify`: Test Suricata rules and config changes
 
---
 
## Best Practices
 
- Run Suricata in IDS + NSM mode for rich metadata
- Tune noisy rules to reduce alert fatigue
- Use `flowbits`/`xbits` to track multi-packet conditions
- Leverage JA3/JA3S for TLS fingerprinting
- Extract and analyze transferred files automatically
- Combine Suricata with Zeek for visibility and correlation
- Monitor `stats.log` for dropped packets or bottlenecks
 
---
 
## Learning Resources
 
- [Official Docs](https://docs.suricata.io/)
- [Suricata Training Slides](https://suricata.io/training/)
- [Suricata + SIEM integration](https://docs.suricata.io/en/suricata-6.0.0/output/eve/eve-json-output.html)
- [Emerging Threats Rules](https://rules.emergingthreats.net/)
 
---