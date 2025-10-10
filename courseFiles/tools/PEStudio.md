
# PEstudio for Linux — Setup and Usage Guide

## Overview

**PEstudio** is a static analysis tool used to examine Windows executables for potential indicators of compromise without running the file. While PEstudio is natively a Windows tool, it can be effectively used on Linux systems via Wine. This guide covers setup, core functionality, and practical usage for SOC analysts.

---

## 1. Setup on Linux

### 1.1 Requirements

Before installation, ensure the following packages are installed:

```bash
sudo apt update && sudo apt install wine winetricks wget unzip -y
```

### 1.2 Install PEstudio

1. **Download the latest version:**
   ```bash
   wget https://www.winitor.com/tools/pestudio/current/pestudio.zip -O pestudio.zip
   ```

2. **Extract it:**
   ```bash
   unzip pestudio.zip -d ~/pestudio
   ```

3. **Launch using Wine:**
   ```bash
   wine ~/pestudio/pestudio.exe
   ```

4. (Optional) **Create a desktop shortcut:**
   ```bash
   echo -e '[Desktop Entry]
Name=PEstudio
Exec=wine ~/pestudio/pestudio.exe
Type=Application
Categories=Utility;' > ~/.local/share/applications/pestudio.desktop
   ```

---

## 2. Core Functionalities

### 2.1 Static File Analysis

- Open a PE file (e.g., `.exe`, `.dll`) in PEstudio.
- It automatically scans and displays key sections like:
  - **Headers:** Metadata, timestamps, compiler info.
  - **Imports/Exports:** External functions and libraries.
  - **Strings:** Extracted ASCII and Unicode strings.
  - **Resources:** Embedded icons, manifests, and version info.
  - **Indicators:** Suspicious traits based on internal rules.

### 2.2 Indicators and Scoring

PEstudio assigns severity levels (0–3) based on heuristics such as:
- Blacklisted API calls.
- Obfuscated imports.
- Packed or encrypted sections.
- Suspicious network-related strings.

### 2.3 VirusTotal Integration

If configured, PEstudio can query **VirusTotal** to check file reputation.
- Go to `Options > VirusTotal > Enable`.
- Insert your API key (from VirusTotal).
- The “VirusTotal” tab will display aggregated detection results.

### 2.4 Shellcode and Signature Detection

- Detects shellcode patterns using built-in YARA-like signatures.
- Highlights anomalies in the `.text` section or imports.

### 2.5 Command-Line Mode (Optional)

For automation, PEstudio supports CLI mode through:
```bash
wine pestudio.exe -file sample.exe -log result.xml
```
This allows integration with analysis pipelines or sandbox environments.

---

## 3. Workflow Example

1. Copy a suspicious file to your Linux analysis directory.
2. Launch PEstudio via Wine.
3. Review sections:
   - **Indicators → High Severity** first.
   - **Imports** for network/system APIs.
   - **Strings** for domains, URLs, or command-line arguments.
4. Export results as XML or TXT for reporting.

---

## 4. Best Practices

- Use **read-only** access when analyzing malware samples.
- Keep **Wine sandboxed** (avoid running with elevated privileges).
- Integrate **VirusTotal** and **YARA** for stronger triage.
- Cross-check findings with other tools like `binwalk`, `strings`, or `exiftool`.

---

## 5. Troubleshooting

| Issue | Fix |
|-------|-----|
| PEstudio doesn’t start | Run `winecfg` once to initialize Wine |
| Missing fonts/UI issues | `winetricks corefonts` |
| No network access for VirusTotal | Check Wine proxy or DNS config |
| Crashes with large files | Increase Wine memory limit or analyze on Windows VM |

---

## 6. References

- Official site: [https://www.winitor.com/](https://www.winitor.com/)
- VirusTotal API: [https://www.virustotal.com/](https://www.virustotal.com/)
- Wine documentation: [https://wiki.winehq.org/](https://wiki.winehq.org/)

---

### Author
SOC Analyst Training Module – Core Tools Series
