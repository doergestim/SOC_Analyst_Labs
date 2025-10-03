# For the Ubuntu VM

## The objective of this lab is to use Hayabusa to analyze Sysmon logs and detect suspicious activity related to process creation, network connections, and authentication events.

**If you want to learn a bit about this tool check the [Hayabusa Documentation](/courseFiles/tools/Hayabusa.md)**

- First things first navigate to hayabusa at `/home/ubuntu/labs/hayabusa_lab/hayabusa/target/release`

```bash
cd /home/ubuntu/labs/hayabusa_lab/hayabusa/target/release
```

- To start off we need to make sure we have the detection rules of hayabusa

```bash
./hayabusa update-rules
```

<img width="527" height="174" alt="image" src="https://github.com/user-attachments/assets/ccf7acb4-342c-45b1-a4b2-ca38b935a450" />

Let's also get the logs that we will be working with and rename them if you don't have it already

```bash
curl -L -o sysmon.evtx https://raw.githubusercontent.com/sbousseaden/EVTX-ATTACK-SAMPLES/master/AutomatedTestingTools/PanacheSysmon_vs_AtomicRedTeam01.evtx
```


- First thing we will do to start disecting the logs is to get some basic **metrics** to understand what system the logs came from, number of events, time range.

```bash
./hayabusa log-metrics --file sysmon.evtx
```

<img width="1900" height="507" alt="image" src="https://github.com/user-attachments/assets/eeff5e5b-c62c-44e1-b054-06ed7cd46c97" />

The logs span about 30 minutes and there are only 565 events, small enough to dig manually but we will do it the smart way.<br><br>

- Next let's see the Event **ID Distribution** to dentify common or suspicious Sysmon events, we are looking for **1**, **3**, **10**, **11** or even **8**

```bash
./hayabusa eid-metrics --file sysmon.evtx
```

<img width="552" height="501" alt="image" src="https://github.com/user-attachments/assets/a08afb66-20a1-4ba5-a2e7-72b20fe7b597" />

Important observations:
1. **Process Creation (ID 1 = 90%)**, that's extremely high volume, and now our primary hunting ground
2. **WMI Activity (IDs 19, 20, 21)**, rare in normal activities, could be remote execution
3. **Network Connections (ID 3)**, check what process made the connection, destination IP/port, and timing.<br><br>

- Now let's proceed with a **Full Timeline Analysis**

```bash
./hayabusa csv-timeline --file sysmon.evtx -o timeline.csv
``` 
>[!TIP]
>
>(include all rules)

<img width="1175" height="859" alt="image" src="https://github.com/user-attachments/assets/4d4748ed-6645-4c22-ae13-71cd4ad79be4" />

Immediately we can see some really telling information, we got hits on 50 events(8.85%), 11 of them being critical alerts of a known backdoor and ransomware

Let's dig deeper

```bash
less timeline.csv | grep "high"
```

One of the alerts looks like this:

<pre>"2019-07-19 17:57:04.412 +03:00","Proc Exec (Non-Exe Filetype)","high","MSEDGEWIN10","Sysmon",1,4070,"Cmdline: C:\Users\IEUser\AppData\Local\Temptcm.tmp -decode c:\file.exe file.txt ¦ Proc: C:\Users\IEUser\AppData\Local\Temptcm.tmp ¦ User: MSEDGEWIN10\IEUser ¦ ParentCmdline: cmd.exe /c C:\Users\IEUser\AppData\Local\Temptcm.tmp -decode c:\file.exe file.txt ¦ LID: 0x50951 ¦ LGUID: 747F3D96-D4B4-5D31-0000-002051090500 ¦ PID: 6260 ¦ PGUID: 747F3D96-DA40-5D31-0000-0010AB5F3C00 ¦ ParentPID: 3932 ¦ ParentPGUID: 747F3D96-DA40-5D31-0000-0010565D3C00 ¦ Description: CertUtil.exe ¦ Product: Microsoft® Windows® Operating System ¦ Company: Microsoft Corporation ¦ Hashes: SHA1=459D928381CDDFDC31D03C3DA5C28E63B1190194,MD5=535CF1F8E8CF3382AB8F62013F967DD8,SHA256=85DD6F8EDF142F53746A51D11DCBA853104BB0207CDF2D6C3529917C3C0FC8DF,IMPHASH=683B8A445B00A271FC57848D893BD6C4","CurrentDirectory: C:\AtomicRedTeam\ ¦ FileVersion: 10.0.17763.1 (WinBuild.160101.0800) ¦ IntegrityLevel: High ¦ ParentImage: C:\Windows\System32\cmd.exe ¦ RuleName: ¦ TerminalSessionId: 1 ¦ UtcTime: 2019-07-19 14:57:04.381","8d1487f1-7664-4bda-83b5-cb2f79491b6a"
</pre>
We get the command: "**C:\Users\IEUser\AppData\Local\Temptcm.tmp -decode c:\file.exe file.txt**" which is a classic Living-off-the-Land (LOLBAS) technique, certutil -decode is often used to decode base64-encoded payloads that were dropped by phishing or scripts

Why it's suspicious?
1. **CertUtil.exe** isn't normally used by regular users
2. Executing from AppData\Local with a .tmp file? Classic sign of malware staging
3. Suggests payload delivery step in malware chain

Following the chain we meet these commands:

**sc.exe create AtomicTestService binPath= C:\AtomicRedTeam\atomics\T1050\bin\AtomicService.exe** - Service Creation for Persistence

**sc.exe start AtomicTestService** - Service Execiution

**sc.exe stop AtomicTestService** - Service Stop (Cleanup?)<br><br>

- We can also do some **Hunting Scenarios**, searching for special keywords

```bash
./hayabusa search --file sysmon.evtx --regex '(?i)(cmd\.exe|powershell|whoami|mimikatz)'
```

<img width="1902" height="277" alt="image" src="https://github.com/user-attachments/assets/9161e8c4-4131-45b3-b563-a6d78e84c199" />

Following up this lead we can get to the same results as earlier, or use it to group alerts by services, the possibilities are endless
<br><br>

## Your turn
>[!TIP]
>
> Try extracting any encrypted payloads and pulling authentication activity yourself, if there is any, using the documentation of the tool.

### Also try finding everything you found in this lab by using [Windows Event Viewer](/courseFiles/tools/WinEventViewer.md)

---
[Back to the Section](/courseFiles/Section_01-logAnalysis_Basics/logAnalysis_basics.md)



