## Overview

**IDA Pro (Interactive DisAssembler)** is a powerful disassembler and debugger used in malware analysis, reverse engineering, and vulnerability research

It converts binary executables into human-readable assembly code, helping analysts understand program behavior without source code access

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/1da33ccd-ae42-4e54-bb6d-38ed432fa5ce" />

---

## Setup

### **Installation Steps**

1.  Download IDA Free from [Hex-Rays officialwebsite](https://hex-rays.com/ida-free)
2.  Install the package following the platform-specific installer and request the license
3.  When opening your account, at the **Licenses** Section you should see your **IDA Free** License

<img width="237" height="88" alt="image" src="https://github.com/user-attachments/assets/f33b1e05-3f75-43f9-9ad3-df50673ceda2" />

4. Click it and scroll down, you should see a **Download Hexclic** Button

<img width="477" height="68" alt="image" src="https://github.com/user-attachments/assets/523232ac-01c3-4b77-b31a-12a826c14a96" />

5. Download it and launch IDA, when you upload a binary you will be prompted to select your license, select the **Hexclic File** that you just downloaded

---

## Interface Overview

### **Main Components**

-   **Disassembly View:** Displays assembly instructions of the loaded binary
-   **Hex View:** Raw hexadecimal representation of the binary
-   **Graph View:** Visual representation of control flow (CFG)
-   **Functions Window:** Lists detected functions with cross-references
-   **Strings View:** Extracted ASCII/Unicode strings for quick intel
-   **Imports/Exports:** Shows linked libraries and external functions

---

## 3. Core Functionalities

### **Loading a Binary**

1.  Open IDA -> `File > Open`
2.  Choose target executable (e.g., `.exe`, `.dll`, `.elf`)
3.  Select processor type (auto-detected in most cases)
4.  Choose analysis options -> Click **OK**

### **Static Analysis**

-   Navigate code using cross-references (press `x` to list references)
-   Rename functions (`n`) and variables (`y`) for readability
-   Add comments (`;`) to document insights
-   Use **Graph View (Spacebar)** for visual call flow analysis

### **Dynamic Analysis (Debugging)**

-   Start a debugger: `Debugger > Select Debugger`
-   Supported types:
    -   Local Windows/Linux Debugger
    -   Remote Debugger (Win/Linux)
    -   Bochs / VMware debugger
-   Set breakpoints (F2), step instructions (F7/F8), and inspect memory/registers

### **Searching & Identification**

-   **Strings:** `Shift + F12`
-   **Functions:** `Ctrl + F`
-   **Patterns:** `Alt + T` (Text search in disassembly)
-   **Signatures (FLIRT):** Identify standard libraries and known functions

### **Decompilation (IDA Pro + Hex-Rays Plugin)**

-   Open a function -> press **F5**
-   View high-level pseudocode for easier logic analysis
-   Rename variables/functions directly from decompiler view

---

## Useful Shortcuts

|  Action             | Shortcut      |
|  ------------------ | ------------- |
|  Graph/Linear View  | Spacebar |
|  Rename Function    | N |
|  Add Comment        | ; |
|  Jump to Address    | G |
|  View Strings       | Shift + F12 |
|  View Imports       | Shift + F11 |
|  Run Debugger       | F9 |
|  Step Into          | F7 |
|  Step Over          | F8 |
|  Toggle Breakpoint  | F2 |

---

## Tips for Malware Analysis

-   Always analyze in an isolated VM
-   Disable auto-networking before debugging malware samples
-   Use **Snapshots** or **Revert States** for quick rollbacks
-   Combine IDA with tools like **x64dbg**, **ProcMon**, **Wireshark**, and **Ghidra** for better context


---
[Back to the Section](/courseFiles/Section_11-malwareForensics/malwareForensics.md)
