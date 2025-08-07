# Log Analysis Basics
### Hands-On Lab : [Log Analysis Basics Lab](/courseFiles/Lab_01-logAnalysis_Basics/logAnalysisBasics_Lab.md)


Log analysis involves reviewing system and security logs to detect unusual or malicious activity. Each log entry typically includes a timestamp, source, user account, event type, and a unique Event ID.
Most important IDs are:
- **4624** = Succesfull Logon
- **4625** = Failed logon
- **4688** = Process creation
- **4670** = Permission changes

Effective log analysis involves:
- Filtering noise
- Correlating events over time (easy using special tools like [Hayabusa](/courseFiles/tools/Hayabusa.md)
- Flagging anomalies like brute-force logins or unusual logon times



## Sysmon Logs
Sysmon (System Monitor) is a Windows system service that logs detailed system activity to the Event Log, beyond what standard logs capture. It’s often used in security monitoring and DFIR.
Sysmon logs appear under the Microsoft-Windows-Sysmon/Operational and include details like:
- **Event ID 1** – Process creation (with full command line, parent/child PID)
- **Event ID 3** – Network connections
- **Event ID 10** – Process access (used for example in credential dumping detection)
- **Event ID 11** – File creation


## Authentication Logs
Authentication logs include detailed information about the device used to access the service, the user’s location, and other considerations that might influence risk. Generally, these logs only include information about the following events:

- Successful logins
- Login attempts
- Login errors
- Logouts, with additional information shows whether a logout was manual or due to session expiration.

>[!IMPORTANT]
>
>Identity breaches may be detected with the help of this information. For instance, brute force credential-guessing attempts can be detected using authentication logs since they will appear in the audit trail as unsuccessful authentications. They are able to identify the users who were impacted by the breach, but they are unable to determine what those user accounts could access or do. Authorization audit logs are useful in this situation. By using authorization decision logs, we will be able to determine the breach's blast radius.



## Tools to Learn
Viewing alerts and logs is key to detecting suspicious activity on a system. Tools like [Hayabusa](/courseFiles/tools/Hayabusa.md) allow analysts to quickly analyze Windows event logs (.evtx), apply detection rules, and generate timelines or alerts for threats such as privilege escalation, malware, or lateral movement as well as the built in [Windows Event Viewer](/courseFiles/tools/WinEventViewer.md).


***                                                       

<b><i>Continuing the course?</b>
</br>
[Click here for the Next Lab](/courseFiles/Lab_01-logAnalysis_Basics/logAnalysis_basics.md)</i>

<b><i>Looking for a different lab? </b></br>[Back to Lab Directory](/coursenavigation.md)</i>
