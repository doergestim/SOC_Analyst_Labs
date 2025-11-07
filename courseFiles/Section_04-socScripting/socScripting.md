# Scripting for SOC Tasks

## Intro

Scripting is a vital skill for SOC analysts to automate routine tasks, analyze data, and respond to incidents efficiently. This chapter covers the fundamentals of Python and PowerShell, focusing on their practical use in security operations.

## Python Scripting Basics

### Key Concepts

#### Parsing Simple Logs (CSV & JSON)

**CSV (Comma-Separated Values)** logs:
- Common for exported logs from SIEMs (e.g., Splunk, ELK)
- Handled using Python’s built-in `csv` module

```python
import csv

with open("EXAMPLE.csv") as file:
    reader = csv.DictReader(file)
    for row in reader:
        print(row["event_type"])
```

**JSON (JavaScript Object Notation)** logs:
- Widely used in modern APIs and log formats (e.g., Sysmon, firewall logs)
- Handled with Python’s `json` module

```python
import json

with open("EXAMPLE.json") as file:
    data = json.load(file)
    for entry in data:
        print(entry["username"])
```

#### Python libraries

- `requests`: For making HTTP requests to APIs (ex: VirusTotal)
- `json`: for working with structured data from logs or API responses
- `re`: regular expressions module for searching log patterns

##### Why `requests`?
- Automates querying threat intelligence platforms like AbuseIPDB or VirusTotal
- Used to send and retrieve data via HTTP(S)

##### Why `json`?
- Most APIs return JSON responses
- Essential for parsing returned data like threat scores or verdicts

##### Why `re`?
- Extracts specific data (ex: IPs, usernames, hashes) from raw text logs

Example pattern:
```python
import re

log = "Failed login from 192.168.1.5"
match = re.search(r"(\d{1,3}\.){3}\d{1,3}", log)
if match:
    print("IP Address found:", match.group())
```

---

## Powershell Basics

PowerShell is a powerful command-line and scripting tool, especially useful for Windows environments

### Key Concepts

#### System Information Gathering

Common PowerShell commands:

```powershell
Get-ComputerInfo      # General system info
Get-Process           # Active processes
Get-Service           # Services running on the system
Get-LocalUser         # Local user accounts
```

These can be combined into scripts to automate host profiling during incidents.

#### Log Collection

PowerShell can collect and export logs from Windows Event Viewer — particularly useful for tracking failed logins or other suspicious behavior.

Example: Exporting failed login attempts (Event ID 4625):

```powershell
Get-WinEvent -LogName Security | Where-Object { $_.Id -eq 4625 } | Export-Csv failed_logins.csv
```

Explanation:
- it filters Windows Security logs for failed logins
- after that, it outputs results to a CSV file for further analysis

---

Also check out [Powershell for SOC](/courseFiles/Lab_04-socScripting/powershell_for_soc.md) for more useful commands to learn.

## Labs and Tools


[Lab 1](/courseFiles/Section_04-socScripting/lab1_detect_brute_force.md)

[Lab 1 Solution](/courseFiles/Section_04-socScripting/lab1_solution_step_by_step.md)

[Lab 2](/courseFiles/Section_04-socScripting/lab2_collect_system_info.md)

[Lab 2 Solution](/courseFiles/Section_04-socScripting/lab2_solution_steb_by_step.md)

[Scapy Documentation part 1](/courseFiles/tools/scapy_documentation_part1.md)

[Scapy Documentation part 2](/courseFiles/tools/scapy_documentation_part2.md)

[Scapy Lab](/courseFiles/Section_04-socScripting/scapy_lab.md)

[Scapy Lab solution](/courseFiles/Section_04-socScripting/scapy_lab_solution.md)


***                                                       

<b><i>Continuing the course?</b>
</br>
[Click here for the Next Section](/courseFiles/Section_05-networkingAndTelemetry/networkingAndTelemetry.md)</i>

<b><i>Want to go back?</b>
</br>
[Click here for the Previous Section](/courseFiles/Section_03-detectionAndThreatBehavior/detectionAndThreatBehavior.md)

<b><i>Looking for a different section? </b></br>[Back to Section Directory](/coursenavigation.md)</i>
