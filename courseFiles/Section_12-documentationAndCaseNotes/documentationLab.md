Here is some example of logs of a Windows workstation with centralized logging (Sysmon + EDR)

<pre>[2025-07-24 13:02:15] Microsoft-Windows-Security-Auditing
Event ID: 4624 (Successful Logon)
Security ID: S-1-5-21-334565123-2489635241-879453123-1120
Account Name: jsmith
Logon Type: 2 (Interactive)
Logon Process: User32
Workstation Name: WKS-204
Source Network Address: 10.0.5.44

[2025-07-24 13:03:02] Microsoft-Windows-Sysmon
Event ID: 1 (Process Create)
Image: C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE
CommandLine: "C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE" "invoice_672.docm"
ParentImage: C:\Windows\explorer.exe
User: DOMAIN\jsmith

[2025-07-24 13:03:08] Microsoft-Windows-Sysmon
Event ID: 1 (Process Create)
Image: C:\Windows\System32\cmd.exe
CommandLine: cmd.exe /c powershell.exe -enc UwB0AGEAcgB0AC0AUAByAG8AYwBlAHMAcw...
ParentImage: C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE
User: DOMAIN\jsmith

[2025-07-24 13:03:09] Microsoft-Windows-Sysmon
Event ID: 3 (Network Connection)
Image: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
Protocol: tcp
Initiated: true
Source IP: 10.0.5.44
Source Port: 49722
Destination IP: 103.145.67.233
Destination Hostname: updates-checker[.]co
Destination Port: 443

[2025-07-24 13:03:10] Microsoft-Windows-Sysmon
Event ID: 11 (File Create)
TargetFilename: C:\Users\jsmith\AppData\Local\Temp\rat_payload.dll
CreationUtcTime: 2025-07-24 13:03:10.430

[2025-07-24 13:03:12] Microsoft-Windows-Sysmon
Event ID: 1 (Process Create)
Image: C:\Windows\System32\rundll32.exe
CommandLine: rundll32.exe C:\Users\jsmith\AppData\Local\Temp\rat_payload.dll,Install
ParentImage: C:\Windows\System32\cmd.exe
User: DOMAIN\jsmith

[2025-07-24 13:03:17] Microsoft-Windows-Sysmon
Event ID: 10 (Process Access)
SourceImage: C:\Windows\System32\rundll32.exe
TargetImage: C:\Windows\System32\lsass.exe
GrantedAccess: 0x1010
CallTrace: C:\Windows\System32\rundll32.exe+0x1234

[2025-07-24 13:03:22] Microsoft-Windows-Sysmon
Event ID: 11 (File Create)
TargetFilename: C:\Users\jsmith\AppData\Local\Temp\mimikatz.log
CreationUtcTime: 2025-07-24 13:03:22.522

[2025-07-24 13:03:30] EDR Platform
Alert ID: EDR-1022
Detection Name: Credential Access - LSASS Dump (rundll32)
Severity: Critical
Action: Blocked
Analyst Note: rundll32.exe accessed LSASS memory, mimikatz signatures detected

[2025-07-24 13:04:01] SOC Platform
Action: Isolated host WKS-204 from network
Operator: Analyst01

[2025-07-24 13:05:17] Microsoft-Windows-Sysmon
Event ID: 3 (Network Connection)
Image: rundll32.exe
Protocol: tcp
Initiated: true
Source IP: 10.0.5.44
Destination IP: 185.62.188.34
Destination Hostname: beacon.stage-backup[.]org
Destination Port: 443

[2025-07-24 13:07:08] Microsoft-Windows-Sysmon
Event ID: 1 (Process Create)
Image: C:\Windows\System32\whoami.exe
CommandLine: whoami /all
ParentImage: rundll32.exe
User: DOMAIN\jsmith
</pre>

## What you should do?
### Build a structured incident timeline
### Write a triage summary with:
- What happened
- What triggered the alerts
- What actions were taken
- What should happen next

>[!IMPORTANT]
>
>Following up is what your work should look like, for your progress' sake, look only after you finished and compare. What did you miss out? What could've you explained more in detail? 

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

### Incident Timeline
<pre>[2025-07-24 13:02:15 UTC] User DOMAIN\jsmith logged in to WKS-204 via interactive session (Logon Type 2).
[2025-07-24 13:03:02 UTC] WINWORD.exe opened a macro-enabled document (invoice_672.docm).
[2025-07-24 13:03:08 UTC] WINWORD.exe spawned cmd.exe → PowerShell with encoded payload.
[2025-07-24 13:03:09 UTC] PowerShell connected to external IP 103.145.67.233 (updates-checker[.]co) over TCP/443.
[2025-07-24 13:03:10 UTC] Malicious DLL saved to: C:\Users\jsmith\AppData\Local\Temp\rat_payload.dll.
[2025-07-24 13:03:12 UTC] DLL executed via rundll32.exe.
[2025-07-24 13:03:17 UTC] rundll32.exe accessed lsass.exe memory (credential dumping behavior).
[2025-07-24 13:03:22 UTC] Mimikatz log file written to Temp directory.
[2025-07-24 13:03:30 UTC] EDR detected and blocked LSASS access — alert triggered (Critical).
[2025-07-24 13:04:01 UTC] Host WKS-204 isolated from the network by SOC Analyst01.
[2025-07-24 13:05:17 UTC] rundll32 attempted outbound connection to beacon.stage-backup[.]org — blocked.
[2025-07-24 13:07:08 UTC] Attacker executed `whoami /all` — confirmed post-exploitation.</pre>

### Triage Summary
**Alert Name:** Suspicious PowerShell Execution + Credential Access  
**Alert ID:** EDR-1022  
**Severity:** Critical  
**Host:** WKS-204  
**User:** DOMAIN\jsmith  
**Date/Time First Observed:** 2025-07-24 13:02 UTC  

---

**Summary:**  
Initial alert was triggered by encoded PowerShell execution from WINWORD.exe (likely macro). The PowerShell session contacted an external IP (103.145.67.233) associated with a suspicious domain (updates-checker[.]co) and downloaded a DLL payload.

The DLL (rat_payload.dll) was executed via rundll32.exe, which accessed LSASS memory — consistent with credential dumping. Mimikatz artifacts were observed. The EDR platform blocked the credential dump attempt and generated a critical alert.

Host was immediately isolated. Attempts to beacon to a known C2 domain (beacon.stage-backup[.]org) were blocked post-isolation. Attacker executed reconnaissance commands (e.g., whoami). Indicators suggest hands-on-keyboard activity.

---

**Actions Taken:**
- Investigated PowerShell activity and parent-child process chain
- Verified outbound network connections to known C2 indicators
- Observed and confirmed LSASS memory access
- Host isolated from network via EDR tooling
- Logs and volatile memory acquired
- Case escalated to Tier 2 and Incident Response

---

**Next Steps:**
- Analyze phishing vector (email chain or document origin)
- Submit rat_payload.dll to sandbox
- Search for IOCs (C2 IPs/domains) across enterprise endpoints
- Initiate password resets for jsmith and high-value accounts
- Monitor for lateral movement from subnet 10.0.5.0/24</pre>

---
[Back to the Section](/courseFiles/Section_11-documentationAndCaseNotes/documentationAndCaseNotes.md)
- Monitor for lateral movement from subnet 10.0.5.0/24
