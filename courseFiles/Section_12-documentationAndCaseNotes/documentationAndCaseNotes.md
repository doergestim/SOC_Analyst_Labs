# Documentation & Case Notes
Why does it matter
- **Auditability** – Incident records must be reproducible for compliance (e.g., ISO 27001, SOC 2)
- **Operational Continuity** – Night shift picks up where day shift left off
- **Evidence Trail** – Case notes may be used in legal proceedings or HR actions
- **Knowledge Base** – Patterns emerge from well-documented incidents, improving detection

Documentation types
| Type  | Purpose | Example |
| ------------- | ------------- | ------------ |
| Case Notes  | Running log of your findings/actions  | Notes in your SOAR tool |
| Incident Timeline  | 	Chronological record  | Used in after-action reports |
| Alert Triage Summary | Final write-up after triage | SOC ticket or escalation |
| Investigation Report | Full postmortem | Often created by Tier 2+ |

Bad Documentation Causes
- Missed hand-offs
- Incorrect escalation
- Slower containment
- Incomplete RCA (Root Cause Analysis)

## Case Notes - The Analyst’s Daily Log
Key Rules
1. **Start Immediately** – Begin documenting as soon as the alert is opened
2. **Be Objective** – Focus on what you observed, not assumptions
3. **Timestamp Everything** – Use UTC and standard formats (e.g., YYYY-MM-DD HH:MM UTC)
4. **Log Every Step** – Even “negative” findings (what you checked and ruled out)

Case Notes Structure
<pre>[2025-07-25 10:22 UTC] Alert triggered: Suspicious PowerShell execution on host WKS-204
[2025-07-25 10:24 UTC] Queried EDR logs – script matches encoded PowerShell from MITRE T1059.001
[2025-07-25 10:28 UTC] Queried Sysmon – found parent process was MS Word; likely macro
[2025-07-25 10:32 UTC] Informed Tier 2. Awaiting sandbox results</pre>

Tip - Always write as if someone unfamiliar with the case will read it next

## Writing an Incident Timeline
Purpose
- Summarizes all events in time order
- Required for formal reporting (incident response, regulatory)

Timeline Format
<pre>[2025-07-24 13:55 UTC] User opened malicious email attachment
[2025-07-24 13:58 UTC] Word spawned powershell.exe (base64 encoded payload)
[2025-07-24 14:01 UTC] Alert triggered by EDR
[2025-07-24 14:05 UTC] Host isolated by SOC Tier 1
[2025-07-24 14:15 UTC] Triage confirmed malware behavior: credential dumping
[2025-07-24 15:00 UTC] Escalated to Incident Response</pre>

You should include: **Triggering event**, **Each detection**, **All actions taken**, **Handoff/escalations**, **Remediation steps**

## Writing an Alert Triage Summary
**Purpose** - Conclusion of an alert investigation—used by management, auditors, and the IR team

### Template 
- **Alert Name** - Suspicious DNS Query
- **Severity** - Medium
- **Assets Involved** - Host WKS-223, user jsmith
- Summary
<pre>Alert was triggered due to excessive DNS queries to DGA-like domains. Investigation confirmed no legitimate application behavior matching this pattern. PCAP showed suspicious outbound traffic to IP 103.54.22.11, known to be used by malware “XLoader.”

No lateral movement observed. Host isolated. Sample sent to sandbox. Awaiting results. Case escalated to Tier 2 for full investigation.</pre>

Actions Taken
1. Queried DNS logs and correlated with EDR
2. Confirmed suspicious behavior (DGA + C2)
3. Isolated host
4. Opened case with IR team

Next Steps

5. Monitor for lateral movement from host’s subnet
6. Analyze sandbox results
7. Determine initial infection vector

## Real-World Examples
### SingHealth Data Breach (Singapore, July 2018)
One of the most publicly dissected breaches involved SingHealth, affecting 1.5 million patients—including Singapore’s Prime Minister. The public Committee of Inquiry report provides a detailed timeline of discovery, escalation, containment, and response

Timeline(from public findings)
<pre>27 June–4 July 2018: Massive data exfiltration occurred—personal particulars of ~1.495 million patients and outpatient medication records of ~159,000 were stolen
4 July 2018: IHIS database administrators detected unusual activity, halted initial exfiltration attempts, and began enhanced monitoring
10 July 2018: Confirmed cyberattack; Ministry of Health, SingHealth, and Cyber Security Agency notified
12 July 2018: SingHealth filed a police report
19 July 2018: Additional attempted malicious activity detected but without exfiltration
20 July 2018: Network‑wide Internet surfing separation enforced; public announcement issued by Ministry of Health and Communications & Information</pre>

[Wiki Page](https://en.wikipedia.org/wiki/2018_SingHealth_data_breach)

## Your turn
### Try this hands-on [Documentation Lab](/courseFiles/Section_11-documentationAndCaseNotes/documentationLab.md)

***
<b><i>Continuing the course?</b>
</br>
[Click here for the Next Section](/courseFiles/Section_13-softSkills/softSkills.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Section](/courseFiles/Section_11-malwareForensics/malwareForensics.md)

<b><i>Looking for a different section? </b></br>[Back to Section Directory](/coursenavigation.md)</i>
