# Security Tools & Foundational Platforms
**Security tools and foundational platforms** are the core technologies used to detect, investigate, and respond to cyber threats. These include tools that collect and analyze data from endpoints, networks, and systems to provide visibility into suspicious activity
<br><br>

Categories of Security Tools
- **Endpoint Detection and Response** (EDR) - Tools like LimaCharlie monitor activities on endpoints (e.g., workstations and servers), collecting telemetry and enforcing detection logic in real-time
- **Security Information and Event Management** (SIEM) - Platforms such as Elastic SIEM aggregate logs from across the infrastructure, providing detection rules, dashboards, and analytics
- **Network Monitoring Tools** - Tools like Zeek or Wireshark are used to inspect raw network traffic and identify anomalies or known malicious behavior

## Intro
**A Security Operations Center** (SOC) is a centralized unit that monitors, detects, responds to, and mitigates cybersecurity threats in real time. SOC analysts are the front line of defense, working with a variety of tools and platforms to protect an organization’s assets

This lab introduces interns to **foundational tools and platforms** used daily in a SOC environment. Understanding how data flows from endpoints to security platforms—and how to investigate, triage, and act—is key to being an effective SOC analyst

## LimaCharlie Hands-On
LimaCharlie is a cutting-edge cybersecurity operations and infrastructure platform made to assist security teams in identifying, addressing, and managing threats on a large scale. With features designed for security engineers and SOC analysts, it provides an adaptable, API-driven substitute for conventional Endpoint Detection and Response (EDR) systems

For a LC hands-on lab, try the 2 part [Lima Charlie Lab](/courseFiles/Section_02-toolsAndPlatforms/lima_charlie_lab_part1.md)

### LimaCharlie Rules Examples

- DNS to known-bad domain
```bash
event: DNS_REQUEST
op: is
path: event/DOMAIN_NAME
value: bad.example
actions:
  - action: report
    name: "DNS to bad.example"
```

- Process from temp launching PowerShell
```bash
event: NEW_PROCESS
op: and
rules:
  - op: contains
    path: event/FILE_PATH
    value: \AppData\Local\Temp\
  - op: ends with
    path: event/PROC_NAME
    value: powershell.exe
actions:
  - action: report
    name: "Temp -> PowerShell"
```

- LSASS access attempt (handle open)
```bash
event: HANDLE_REQUEST
op: and
rules:
  - op: ends with
    path: event/TARGET/PROC_NAME
    value: lsass.exe
  - op: is
    path: event/ACCESS
    value: PROCESS_VM_READ
actions:
  - action: report
    name: "LSASS read attempt"
```

- Persistence via Run key
```bash
event: REGISTRY_EDIT
op: starts with
path: event/REG_PATH
value: HKCU\Software\Microsoft\Windows\CurrentVersion\Run\
actions:
  - action: report
    name: "Run key persistence"
```

## Elastic SIEM Hands-On
[Elastic SIEM Docs](/courseFiles/tools/Elastic_Doc_Cloud.md), part of the Elastic Stack (Elasticsearch, Logstash, Kibana), provides a flexible and powerful SIEM solution for log ingestion, search, visualization, and detection

After setting it up, you can try the Elastic Labs for the [Cloud Version Lab](/courseFiles/Section_02-toolsAndPlatforms/elasticLabCloud.md) to run on our VM, or the [Local Version Lab](/courseFiles/Section_02-toolsAndPlatforms/elasticLabLocal.md) in case you want to install it locally on your system

### Elastic Rules Examples

- PowerShell with encoded command
```bash
event.category:process and event.type:start and
process.name:("powershell.exe","pwsh.exe") and
process.command_line:(*"-enc"* or *"-EncodedCommand"* or *"FromBase64String("*)
```

- Delete Volume Shadow Copies
```bash
event.category:process and event.type:start and
process.name:"vssadmin.exe" and process.command_line:(*delete* and *shadows*)
```

- Multiple failed logons from one IP (Threshold rule logic)
```bash
event.dataset:"windows.security" and winlog.event_id:4625
```

- New Terms: First-time admin tool seen on a host
```bash
event.category:process and process.name:(psexec*.exe or "wmic.exe" or "at.exe" or "schtasks.e
```

## Viewing Alerts & Logs
SOC analysts must interpret alerts in context and verify their validity

Understanding Alerts:
- **Components** - Rule name, severity, MITRE technique, timestamp, affected host/user, raw log snippet
- **Enrichment** - Threat intel (e.g., VirusTotal hash lookup), geolocation, asset context
- **Triage Steps**
1. Is this a known good or benign behavior?
2. Can we correlate with other logs to confirm malicious intent?
3. Are other hosts showing similar behavior?

Log Types to Recognize:
- **Windows Event Logs** (Security, System, Application)
- **Sysmon events** (detailed host telemetry)
- **Firewall logs** (source/destination IP, port, action)
- **DNS logs** (lookups to malicious domains)
- **Web proxy logs** (user-agent, URL visited)

## Writing & Modifying Basic Detection Rules
Detection rules define what behavior is suspicious and should generate alerts. Interns should be able to read, tweak, and eventually write basic rules.

Detection Rule Elements
- **Condition** - What patterns or values to match (e.g., parent process = cmd.exe, child process = powershell.exe)
- **Filters/Exceptions** - Known-good behaviors to suppress false positives
- **Severity & Tactic** - Classification based on MITRE ATT&CK

Examples
- **Sigma Rule (YAML)** - <pre>Detect PowerShell with base64-encoded strings</pre>
- **Elastic Rule (KQL)** - <pre>event.code: 4625 and winlog.event_data.LogonType: 3</pre>
- **LimaCharlie Rule (YAML)** - <pre>Match file writes to suspicious locations</pre>

## Telemetry Searching
Analysts often need to go beyond alerts and search the raw telemetry for related or supporting evidence

Key Skills
- **Search Syntax** - Learn how to query data (e.g., KQL in Elastic, telemetry filters in LimaCharlie)
- **Pivoting** - From an IP to related processes, users, or domains
- **Filtering** - Time ranges, specific hostnames, or event types<br>
- **Common Use Cases:**
1. "Show all PowerShell executions on this host in the last 24 hours"
2. "List all logon events from a suspicious IP"
3. "Search for DLL side-loading activity using Sysmon logs"

***                                                       

<b><i>Continuing the course?</b>
</br>
[Click here for the Next Section](/courseFiles/Section_03-detectionAndThreatBehavior/detectionAndThreatBehavior.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Section](/courseFiles/Section_01-logAnalysis_Basics/logAnalysis_basics.md)

<b><i>Looking for a different section? </b></br>[Back to Section Directory](/coursenavigation.md)</i>

