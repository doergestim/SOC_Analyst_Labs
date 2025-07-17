# Introduction to MITRE ATT&CK Framework

**MITRE ATT&CK** (Adversarial Tactics, Techniques, and Common Knowledge) is a globally recognized framework that documents how real-world adversaries operate during cyberattacks.

## Structure of the Framework

The framework is organized in a **matrix** format representing different phases of an attack, known as **tactics**. Each tactic contains multiple **techniques**, and some techniques may include **sub-techniques** for more granular detail.

- **Reconnaissance** – Using browsing services to find publicly available information  
- **Resource Development** – Develop malware, obtain digital certificates  
- **Initial Access** – Phishing, exploit public-facing applications  
- **Execution** – PowerShell, command and scripting interpreters  
- **Persistence** – Registry Run Keys, scheduled tasks  
- **Privilege Escalation** – Exploitation for privilege escalation  
- **Defense Evasion** – Obfuscated files or information  
- **Credential Access** – Keylogging, credential dumping  
- **Discovery** – System information discovery  
- **Lateral Movement** – Remote Desktop Protocol (RDP), SMB  
- **Collection** – Screen capture, input capture  
- **Command and Control** – DNS tunneling, web service usage  
- **Exfiltration** – Exfiltration over HTTPS  
- **Impact** – Data destruction, defacement


## Variants of ATT&CK

MITRE ATT&CK has several versions for different environments:

- **Enterprise** - Windows, Linux, macOS, Cloud
- **Mobile**
- **ICS** - Industrial Control Systems

## Mapping Log Events to ATT&CK Techniques

To apply MITRE ATT&CK effectively, security teams often map log events (from Sysmon, Windows Event Logs, etc.) to ATT&CK techniques. This mapping helps:

- Identify the **stage** of an attack
- Prioritize **alerts** based on tactics and techniques
- Detect anomalies aligned with known **adversary behavior**

### Example:

| Log Source     | Event ID | Observed Behavior           | ATT&CK Technique                  |
|----------------|----------|-----------------------------|-----------------------------------|
| Sysmon         | 1        | PowerShell Execution        | `T1059.001 - PowerShell`          |
| Windows Events | 4624     | Successful Logon            | `T1078 - Valid Accounts`          |
| Sysmon         | 11       | File Created in Startup Dir | `T1547.001 - Registry Run Keys`   |

Security Information and Event Management (SIEM) tools like **Splunk**, **ELK**, or **Microsoft Sentinel** can automate this mapping using detection rules or correlation engines.

## Open Source Projects
Several open-source tools and projects help integrate ATT&CK into detection, analysis, and red teaming workflows:

- **ATT&CK Navigator**  
  A web-based tool for visualizing techniques across the matrix.  
  🔗 https://mitre-attack.github.io/attack-navigator/

- **Sigma**  
  A generic rule format for log detection that includes mappings to ATT&CK techniques.  
  🔗 https://github.com/SigmaHQ/sigma

- **Hayabusa**  
  A fast, open-source EVTX log analyzer that parses Windows event logs and maps detections to MITRE ATT&CK.  
  🔗 https://github.com/Yamato-Security/hayabusa

- **Atomic Red Team**  
  A library of small, testable scripts that simulate attacker behavior for each ATT&CK technique.  
  🔗 https://github.com/redcanaryco/atomic-red-team

- **Caldera**  
  An automated red teaming framework developed by MITRE that uses ATT&CK for planning and execution.  
  🔗 https://github.com/mitre/caldera


***                                                       
