## Overview

**IDA Pro (Interactive DisAssembler)** is a powerful disassembler and debugger used in malware analysis, reverse engineering, and vulnerability research

It converts binary executables into human-readable assembly code, helping analysts understand program behavior without source code access

------------------------------------------------------------------------

## 1. Installation & Setup

### **Requirements**

-   OS: Windows, Linux, or macOS
-   RAM: Minimum 4 GB (Recommended: 8 GB+)
-   Disk Space: \~1 GB
-   License: Paid (with Free version available as IDA Free)

### **Installation Steps**

1.  Download IDA Pro or IDA Free from [Hex-Rays official
    website](https://hex-rays.com/ida-pro/).
2.  Install the package following the platform-specific installer.
3.  (Optional) Configure license via the **license.key** file or online
    activation.
4.  Verify installation by launching IDA and loading a sample binary.

------------------------------------------------------------------------

## 2. Interface Overview

### **Main Components**

-   **Disassembly View:** Displays assembly instructions of the loaded
    binary.
-   **Hex View:** Raw hexadecimal representation of the binary.
-   **Graph View:** Visual representation of control flow (CFG).
-   **Functions Window:** Lists detected functions with
    cross-references.
-   **Strings View:** Extracted ASCII/Unicode strings for quick intel.
-   **Imports/Exports:** Shows linked libraries and external functions.

------------------------------------------------------------------------

## 3. Core Functionalities

### **3.1 Loading a Binary**

1.  Open IDA → `File > Open`
2.  Choose target executable (e.g., `.exe`, `.dll`, `.elf`)
3.  Select processor type (auto-detected in most cases)
4.  Choose analysis options → Click **OK**

### **3.2 Static Analysis**

-   Navigate code using cross-references (press `x` to list references).
-   Rename functions (`n`) and variables (`y`) for readability.
-   Add comments (`;`) to document insights.
-   Use **Graph View (Spacebar)** for visual call flow analysis.

### **3.3 Dynamic Analysis (Debugging)**

-   Start a debugger: `Debugger > Select Debugger`
-   Supported types:
    -   Local Windows/Linux Debugger
    -   Remote Debugger (Win/Linux)
    -   Bochs / VMware debugger
-   Set breakpoints (F2), step instructions (F7/F8), and inspect
    memory/registers.

### **3.4 Searching & Identification**

-   **Strings:** `Shift + F12`
-   **Functions:** `Ctrl + F`
-   **Patterns:** `Alt + T` (Text search in disassembly)
-   **Signatures (FLIRT):** Identify standard libraries and known
    functions.

### **3.5 Decompilation (IDA Pro + Hex-Rays Plugin)**

-   Open a function → press **F5**
-   View high-level pseudocode for easier logic analysis.
-   Rename variables/functions directly from decompiler view.

------------------------------------------------------------------------

## 4. Useful Shortcuts

  Action              Shortcut
  ------------------- -------------
  Graph/Linear View   Spacebar
  Rename Function     N
  Add Comment         ;
  Jump to Address     G
  View Strings        Shift + F12
  View Imports        Shift + F11
  Run Debugger        F9
  Step Into           F7
  Step Over           F8
  Toggle Breakpoint   F2

------------------------------------------------------------------------

## 5. Tips for Malware Analysis

-   Always analyze in an isolated VM.
-   Disable auto-networking before debugging malware samples.
-   Use **Snapshots** or **Revert States** for quick rollbacks.
-   Combine IDA with tools like **x64dbg**, **ProcMon**, **Wireshark**,
    and **Ghidra** for better context.

------------------------------------------------------------------------

## 6. References

-   [Hex-Rays
    Documentation](https://hex-rays.com/products/ida/support/idapronotes/)
-   [IDA User Guide
    (PDF)](https://hex-rays.com/products/ida/support/idadoc/)
-   \[Practical Malware Analysis by Sikorski & Honig\]
-   [Malware Unicorn - IDA Pro
    Tutorials](https://malwareunicorn.org/workshops/re101.html)

------------------------------------------------------------------------

**Author:** Jupane\
**Course:** SOC Core Skills --- Malware Forensics\
**Tool:** IDA Pro v8.x+
