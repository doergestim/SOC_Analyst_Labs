# Snort
 
Snort is an open-source Network Intrusion Detection System (NIDS) and Intrusion Prevention System (IPS) developed by Cisco. It inspects network traffic in real-time and uses a powerful rule-based language to detect a wide range of malicious activities. Snort is essential for SOC Analysts in detecting, alerting, and even blocking threats.
 
---
 
## Installation
 
- **Linux (Ubuntu/Debian):**
  ```bash
  sudo apt install snort
  ```
- **From Source:** [https://www.snort.org/downloads](https://www.snort.org/downloads)
 
 
Make sure `daq`, `libpcap`, and network interfaces are configured correctly.
 
---
 
## Snort Operating Modes
 
| Mode          | Command Example                                     | Description                                          |
| ------------- | --------------------------------------------------- | ---------------------------------------------------- |
| Sniffer       | `snort -v`                                          | Displays packet headers                              |
| Packet Logger | `snort -dev -l /var/log/snort`                      | Logs packets to file                                 |
| NIDS          | `snort -A console -c /etc/snort/snort.conf -i eth0` | Intrusion Detection System with alerts               |
| IPS Mode      | Used with inline configuration (NFQUEUE)            | Blocks/drop packets inline (with iptables/netfilter) |
 
---
 
## Snort Architecture
 
- **Packet Decoder**: Decodes layer 2–7 traffic
- **Preprocessors**: Normalize traffic (e.g., frag3, stream5)
- **Detection Engine**: Uses rules to analyze normalized packets
- **Logging/Alerting**: Generates alerts/logs (console, files, syslog)
- **Output Plugins**: Customize logging behavior
 
---
 
## Snort Rule Structure
 
```
action protocol src_ip src_port -> dst_ip dst_port (options)
```
 
### Example:
 
```
alert tcp $EXTERNAL_NET any -> $HOME_NET 80 (msg:"Potential HTTP Attack"; flow:to_server,established; content:"/etc/passwd"; nocase; sid:1000001; rev:1;)
```
 
### Rule Actions:
 
- `alert` – generate an alert
- `log` – log the packet
- `pass` – ignore the packet
- `drop` – block and log the packet (IPS)
- `reject` – block and send reset/ICMP
 
### Rule Options:
 
- `msg` – message to log
- `sid` – Snort ID (unique rule identifier)
- `rev` – revision number
- `content` – match specific content in payload
- `pcre` – match regex patterns
- `flow` – direction of traffic (to\_server, to\_client)
- `dsize` – payload size
- `byte_test`, `byte_jump` – inspect binary data
- `threshold`, `rate_filter` – limit alerts
 
---
 
## Snort Configuration
 
Main config file: `/etc/snort/snort.conf`
 
Key elements:
 
- Define `HOME_NET`, `EXTERNAL_NET`, `RULE_PATH`
- Include rule files (e.g., `include $RULE_PATH/community.rules`)
- Output configuration (console, unified2, log directory)
 
---
 
## Relevant Use Cases
 
| Detection Task                  | Relevant Feature or Rule Type           |
| ------------------------------- | --------------------------------------- |
| Detect brute-force login        | `flow`, `threshold`, `flags:S`          |
| Malware C2 communication        | `content`, `flow`, `pcre`               |
| DNS exfiltration                | `content:".xyz"`, UDP port 53           |
| Scanning and recon              | `flags:S`, multiple connections from IP |
| Exploit detection (HTTP/SHELL)  | `content:"/etc/passwd"`, `uricontent:`  |
| Detect suspicious file download | `flow`, `content:".exe"`, HTTP rules    |
 
---
 
## Logging and Alerts
 
- **Console output**:
  ```bash
  snort -A console -c /etc/snort/snort.conf -i eth0
  ```
- **Syslog output**: Define in `snort.conf`
- **Unified2**: Used with Barnyard2 for SIEM integration
- **Log directory**: `/var/log/snort/` (alert, log files, packet dumps)
 
---
 
##  Automation/Integration
 
- Use Snort logs in SIEM (Splunk, Elastic, QRadar)
- Write custom scripts to parse alert files
- Use Barnyard2 to format Snort logs for external analysis
 
---
 
## Best Practices
 
- Always keep rules updated (Snort.org, Emerging Threats)
- Tailor `HOME_NET` and `EXTERNAL_NET` to your environment
- Tune noisy rules to reduce false positives
- Use `threshold` and `rate_filter` to control alert volumes
- Run Snort alongside Zeek and Suricata for coverage
- Regularly review alerts and logs
 
---
 
## Rule Sources
 
- [Snort Community Rules](https://www.snort.org/downloads#rule-downloads)
- [Emerging Threats](https://rules.emergingthreats.net/)
- [ET Open](https://rules.emergingthreats.net/open/snort-2.9.0/)
 
---
 
## Documentation/ other resources
 
- [Official Snort Docs](https://docs.snort.org/)
- [Snort User Manual (PDF)](https://www.snort.org/documents)
- [snorpy](https://snorpy.cyb3rs3c.net) -A Web Based Snort Rule Creator

---