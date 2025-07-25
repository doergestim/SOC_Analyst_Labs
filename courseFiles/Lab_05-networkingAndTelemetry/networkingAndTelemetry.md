#  Networking & Telemetry 101
Understanding network traffic is essential for detecting and investigating threats. In this lab, you'll get hands-on experience with tools like Wireshark, tcpdump, Zeek, Snort, and Suricata to capture, analyze, and interpret network telemetry—key skills for any SOC analyst.

 ## Understanding Key Network Protocols

### DNS (Domain Name System)
- **Purpose**: Resolves domain names to IP addresses.
- **Port**: UDP 53 (sometimes TCP 53)
- **SOC Relevance**:
  - Detect domain generation algorithms (DGA)
  - Unusual domain lookups (ex: `.xyz`, newly registered)
  - DNS Tunneling

**Example Log Entry**:
```
Query: suspicious-domain[.]com
Response IP: 192.0.2.123
```

---

### HTTP (HyperText Transfer Protocol)
- **Purpose**: Web communication (insecure)
- **Port**: TCP 80
- **SOC Relevance**:
  - Indicators of compromise (IOCs) in URLs
  - Unusual user-agent strings or command-and-control (C2) traffic

**Example Indicators**:
```
GET /malicious.js HTTP/1.1
Host: attacker-site.com
```

---

### HTTPS (HTTP Secure)
- **Purpose**: Encrypted web traffic
- **Port**: TCP 443
- **SOC Relevance**:
  - Cannot see payloads, but metadata (SNI, certs) still useful
  - Monitor for self-signed or expired certificates
  - JA3 fingerprinting for TLS client/server behavior

---

### SSH (Secure Shell)
- **Purpose**: Remote shell access
- **Port**: TCP 22
- **SOC Relevance**:
  - Brute-force attempts
  - Unauthorized access
  - Lateral movement
  - Abnormal usage outside admin hours

---

### RDP (Remote Desktop Protocol)
- **Purpose**: Remote desktop access to Windows systems
- **Port**: TCP/UDP 3389
- **SOC Relevance**:
  - Target for brute-force or exploitation
  - Detection of external RDP attempts
  - Beaconing or unexpected RDP session initiations

---

### SMB (Server Message Block)
- **Purpose**: File and printer sharing in Windows environments
- **Port**: TCP 445
- **SOC Relevance**:
  - Common target for lateral movement (ex: EternalBlue exploit)
  - Used for file transfers and service enumeration

---

### ICMP (Internet Control Message Protocol)
- **Purpose**: Network diagnostics (ex: ping)
- **Port**: N/A (Layer 3 protocol)
- **SOC Relevance**:
  - Can indicate reconnaissance (ex: ping sweeps)
  - May be used in covert channels (ICMP tunneling)

---

### FTP (File Transfer Protocol)
- **Purpose**: Transfer files between systems
- **Port**: TCP 21
- **SOC Relevance**:
  - Insecure; credentials are sent in plaintext
  - May be used by attackers to exfiltrate data

---

### LDAP (Lightweight Directory Access Protocol)
- **Purpose**: Access and maintain distributed directory information
- **Port**: TCP/UDP 389 (LDAPS: TCP 636)
- **SOC Relevance**:
  - Reconnaissance and enumeration in Active Directory environments
  - Abnormal queries or excessive lookups can indicate compromise

---

 >[Networking Protocols](/courseFiles/Lab_05-networkingAndTelemetry/network_protocols.md)



 ## Identifying Network Logs

### What is a Network Log?
Network logs are structured records of events and traffic observed by network devices and monitoring tools such as firewalls, routers, IDS/IPS, and SIEM platforms. These logs provide visibility into who is communicating, what protocols are used, and whether the traffic is legitimate or suspicious.

---

### Typical Fields in Network Logs

| Field                   | Description                                  |
| ----------------------- | -------------------------------------------- |
| **Source IP**           | IP address initiating the connection         |
| **Destination IP**      | IP address receiving the connection          |
| **Source Port**         | Ephemeral port on the source device          |
| **Destination Port**    | Port for the service (e.g., 80 for HTTP)     |
| **Protocol**            | Transport layer protocol (TCP/UDP/ICMP)      |
| **Action**              | Outcome (e.g., allowed, denied, logged)      |
| **Bytes Sent/Received** | Amount of data transferred                   |
| **Packets**             | Number of packets transferred                |
| **Flags**               | TCP flags (e.g., SYN, ACK, FIN)              |
| **Application**         | Identified application or service            |
| **Timestamp**           | Time the event occurred                      |

#### Sample Log (Zeek Format)
```
192.168.1.5 -> 93.184.216.34 TCP 443 "SSL" Bytes: 4321
```

---

### Log Sources and Use Cases

- **Firewall Logs**: Show traffic allowed/denied by firewall rules
- **Proxy Logs**: Record web browsing activity (URLs, domains)
- **IDS/IPS Logs**: Alert on detected malicious patterns
- **Endpoint Logs**: Provide insight into local process activity and network usage
- **SIEM Aggregated Logs**: Centralized log aggregation, correlation, alerting


 >[Network Logs](/courseFiles/Lab_05-networkingAndTelemetry/network_logs.md)

 ## Examining Packet Captures
 
 Check out the [Wireshark LDAP Lab](/courseFiles/Lab_05-networkingAndTelemetry/wireshark_ldap_lab.md)

 ## Metadata NetFlow vs Full Packet Capture
 
### NetFlow (Metadata-Only)
- Originally developed by Cisco, NetFlow provides summarized network flow metadata.
- Does **not** include payload, only metadata about traffic flows.
- Widely supported (NetFlow, IPFIX, sFlow are variants)

#### NetFlow Fields:
- Source/Destination IP & Port
- Protocol (TCP, UDP, ICMP)
- Number of packets
- Number of bytes
- Start and end timestamps
- Interface identifiers (ingress/egress)
- Flow direction

#### Advantages:
- Lightweight, scalable
- Good for traffic patterns, anomaly detection, volumetrics
- Often used in large networks to detect DDoS, scanning, and beaconing

#### Limitations:
- No packet contents (can't see commands, data exfiltration, malware)
- Not suitable for detailed forensics

#### Example:
```
Src IP: 10.0.0.5 -> Dst IP: 172.16.0.1
Protocol: TCP
Bytes: 1540
Packets: 10
```

---

### Full Packet Capture (PCAP)
- Captures the entire content of each packet, including headers and payloads
- Tools: Wireshark, tcpdump, Zeek (for indexing), Arkime

#### Advantages:
- Enables deep forensic analysis
- Useful for malware reverse engineering
- Detects zero-day and custom exploits

#### Limitations:
- Requires large storage
- Privacy concerns (captures passwords, PII)
- Needs indexing/metadata tools for scalability

---

### Comparison

| FEATURE            | NetFlow              | Full Packet Capture         |
|--------------------|----------------------|------------------------------|
| **Data Type**       | Flow metadata only   | Full content + headers       |
| **Detail Level**    | Medium               | Very high                    |
| **Storage Needs**   | Low                  | High                         |
| **Use Cases**       | Trend analysis, alerting | Deep forensics, payload analysis |
| **Tooling**         | nfcapd, YAF, SiLK     | Wireshark, tcpdump, Arkime  |
| **Privacy Risk**    | Low                  | High                         |

---

 >[Netflow vs Full Packet Capture](/courseFiles/Lab_05-networkingAndTelemetry/netflow_vs_full_packet_capture.md)

 ## Labs

 [Wireshark LDAP Lab](/courseFiles/Lab_05-networkingAndTelemetry/wireshark_ldap_lab.md)

 [Wireshark LDAP Lab Solution](/courseFiles/Lab_05-networkingAndTelemetry/wireshark_ldap_lab_solution.md)

 [IDS Snort/Suricata Lab](/courseFiles/Lab_05-networkingAndTelemetry/ids_lab.md)

 ***               

<b><i>Continuing the course?</b>
</br>
[Click here for the Next Lab](/courseFiles/Lab_06-browserAndCloudSecurity/browserAndCloudSecurity.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Lab](/courseFiles/Lab_04-socScripting/socScripting.md)

<b><i>Looking for a different lab? </b></br>[Back to Lab Directory](/coursenavigation.md)</i>