
# 1. System Information Gathering

### Get basic system details:

```powershell
Get-ComputerInfo
systeminfo
```

### Get current user and logged-in users:

```powershell
whoami
Get-WmiObject -Class Win32_ComputerSystem | Select-Object UserName
```

### List local users and groups:

```powershell
Get-LocalUser
Get-LocalGroupMember -Group "Administrators"
```

### Check process list and services:

```powershell
Get-Process
Get-Service | Where-Object {$_.Status -eq "Running"}
```

---

# 2. Log Collection and Event Analysis

### List available log types:

```powershell
Get-WinEvent -ListLog *
```

### Check Security log (event ID 4625 = failed login):

```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625} | Format-Table TimeCreated, Message -AutoSize
```

### Filter failed login attempts from the last 24 hours:

```powershell
$Yesterday = (Get-Date).AddDays(-1)
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625; StartTime=$Yesterday} | 
  Select-Object TimeCreated, @{Name="User";Expression={($_.Properties[5].Value)}}, Message
```

---

# 3. Network and Connection Checks

### List all network interfaces and IPs:

```powershell
Get-NetIPAddress | Where-Object {$_.AddressFamily -eq "IPv4"}
```

### Check active TCP connections:

```powershell
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State
```

### Find remote connections by process:

```powershell
Get-NetTCPConnection | Group-Object -Property OwningProcess | Sort-Object Count -Descending
```

---

# 4. File and Folder Forensics

### List all files in a directory recursively:

```powershell
Get-ChildItem -Recurse -Force
```

### Get hash of a file (useful for malware detection):

```powershell
Get-FileHash C:\Path\To\File.exe -Algorithm SHA256
```

---

# 5. Useful One-Liners for SOC Use

### Check last login times:

```powershell
Get-LocalUser | Select-Object Name, LastLogon
```

### Export running processes to CSV:

```powershell
Get-Process | Select-Object Name, Id, CPU | Export-Csv -Path processes.csv -NoTypeInformation
```

### Save specific event log to file:

```powershell
Get-WinEvent -LogName Security -MaxEvents 1000 | Export-Clixml -Path logs.xml
```

---

resources:

- https://github.com/sbousseaden/EVTX-ATTACK-SAMPLES
- https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/
- https://learn.microsoft.com/en-us/powershell/scripting/learn/ps101/01-getting-started
- https://learn.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.5