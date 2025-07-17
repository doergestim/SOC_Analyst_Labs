# Lab: Introduction to Detection & Threat Behavior

## Goal
Simulate and detect basic adversarial techniques mapped to the MITRE ATT&CK framework using PowerShell, Sysmon, and open-source tools.

---

## Step 1: Install Sysmon with Configuration

1. Open **Command Prompt** as Administrator.
2. Navigate to the lab directory:

```cmd
cd C:\Users\user\Desktop\sysmon-lab
```

3. Install Sysmon with the provided configuration:

```cmd
Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

---

## Step 2: Simulate ATT&CK Techniques

### Tactic 1: Execution
**Technique**: PowerShell with EncodedCommand  
**ATT&CK ID**: [T1059.001](https://attack.mitre.org/techniques/T1059/001/)

1. Open **PowerShell** as Administrator.
2. Run the following script:

```powershell
$command = 'Start-Process notepad.exe'
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encoded = [Convert]::ToBase64String($bytes)
powershell.exe -EncodedCommand $encoded
```

---

### Tactic 2: Credential Access
**Technique**: LSASS Dumping with ProcDump  
**ATT&CK ID**: [T1003.001](https://attack.mitre.org/techniques/T1003/001/)

1. Navigate to the `Procdump` directory:

```cmd
cd C:\Users\user\Desktop\sysmon-lab\Procdump
```

2. Run the memory dump command:

```cmd
.\procdump.exe -ma lsass.exe lsass.dmp
```

> **Note**: Windows Defender may block this action. If necessary, disable it temporarily for testing.

---

## Step 3: Analyze with Hayabusa

1. Open **CMD** as Administrator.
2. Navigate to the Hayabusa directory:

```cmd
cd C:\Users\user\Desktop\sysmon-lab\hayabusa
```

3. Generate an ATT&CK-mapped CSV timeline from Sysmon logs:

```cmd
hayabusa csv-timeline --file "C:\Windows\System32\winevt\Logs\Microsoft-Windows-Sysmon%4Operational.evtx" -o results.csv
```

---

## Step 4: Map Activity to MITRE ATT&CK

Map the activity from result.csv as in the example below:

| Tactic             | Technique                     | Example Command                               | Sysmon Event |
|--------------------|-------------------------------|-----------------------------------------------|--------------|
| Execution          | PowerShell EncodedCommand     | `powershell.exe -EncodedCommand ...`          | ID 1         |
| Credential Access  | LSASS Dumping (ProcDump)      | `procdump.exe -ma lsass.exe`                  | ID 10        |

---

## Evidence & Detection (results.csv Summary)

I attached a file named [results.csv](./results.csv) which is the summary from testing the lab. Down below are some notes from the Tactic detections

### Tactic 1: Execution via Encoded PowerShell Commands

These events indicate the use of base64-encoded PowerShell commands—a known obfuscation and execution technique.

- **Detection**: Suspicious Encoded PowerShell Command Line  
- **Severity**: High  
- **Example Timestamp**: `2025-07-16 17:16:08.919 +03:00`  
- **Detection Rows**: 94 through 124 (and more)  
- **Indicators**:
  - Use of `-EncodedCommand` flag
  - Multiple repeated command-line executions

---

### Tactic 2: Credential Dumping via LSASS / ProcDump

These events show interaction with the `lsass.exe` process—an indicator of credential dumping.

- **Detection**: 
  - LSASS Process Memory Dump
  - Tools Dropping Dump Files
  - `lsass` or `procdump` in CommandLine
- **Severity**: High  
- **Example Timestamp**: `2025-07-16 17:19:56.021 +03:00`  
- **Detection Rows**: 1–16 and 56–148  
- **Indicators**:
  - `procdump.exe` usage
  - Creation of `lsass.dmp`
  - Alerts on memory access to `lsass.exe`

---

# links and sources:
https://github.com/SwiftOnSecurity/sysmon-config
https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
https://learn.microsoft.com/en-us/sysinternals/downloads/procdump
https://github.com/Yamato-Security/hayabusa
https://www.microsoft.com/en-ca/software-download/windows10
https://www.virtualbox.org/


***                                                       

<b><i>Continuing the course?</b>
</br>
[Click here for the Next Lab](/courseFiles/Lab_04-socScripting/socScripting.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Lab](/courseFiles/Lab_02-toolsAndPlatforms/toolsAndPlatforms.md)

<b><i>Looking for a different lab? </b></br>[Back to Lab Directory](/coursenavigation.md)</i>