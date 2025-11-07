**Binary Ninja** is a reverse engineering platform for analyzing binaries (PE, ELF, Mach-O, etc.) 

It’s used in **malware analysis**, **vulnerability research**, and **reverse engineering** - key skills for analysts dealing with advanced threats or obfuscated binaries

## Setup

- You can download it from [here](https://binary.ninja/free/)
- 
<img width="542" height="128" alt="image" src="https://github.com/user-attachments/assets/46b05dc6-f886-413d-a34e-80b4e6273255" />

- Choose your OS, and unzip after downloading

- On windows run the installer, on **Linux** and **MacOS**, just navigate to the main folder and run the executable

```bash
./binaryninja
```

<img width="648" height="524" alt="image" src="https://github.com/user-attachments/assets/1520ddad-e371-4e7e-9ab5-e8fbf03ae725" />

## Interface Walkthrough

1. Main Layout

- **Navigation Pane:** Functions, Symbols, Strings, Data Variables
- **Graph View:** Control Flow Graph (CFG)
- **Linear View:** Traditional disassembly
- **Medium/High-Level IL Views:** Decompilation layers
- **Log/Console Panel:** Analysis logs and Python scripting console

<img width="303" height="674" alt="image" src="https://github.com/user-attachments/assets/7e1a021b-8dcd-405a-b7cd-5bc3a57439db" />

<br><br>

2. Key Modes

- **Hex View:** Raw binary data
- **Disassembly View:** Instruction-level analysis
- **IL Views:** Simplified logic representation (useful for understanding flow without deep ASM knowledge)

<img width="892" height="596" alt="image" src="https://github.com/user-attachments/assets/f2c3319f-5a69-4a13-9251-2cc96101551e" />

## Core Functionalities

### Loading and Analyzing a Binary

- Open target binary.
- Wait for auto-analysis to complete.
- Switch to Graph View -> observe the control flow.

### Function Analysis

- Functions are automatically detected
- Hover over a function to view signature, cross-references, and comments
- You can rename ( `N` ) or comment ( `;` ) to annotate analysis

### Intermediate Languages (ILs)

- **LLIL** (Low-Level IL): Close to assembly
- **MLIL** (Medium-Level IL): Easier to read, removes most stack details
- **HLIL** (High-Level IL): Pseudo-C style for logic tracing

Switch using the toolbar or `View -> IL Mode`

### Cross-References and Strings

- `Strings View` -> find hardcoded **URLs**, **commands**, or malware **IOCs**
- Right-click -> `Show References` to trace where they’re used

### Symbol and Data Views

- Explore imports/exports to identify API usage ( **WinExec**, **InternetOpenA** )
- Spot suspicious or obfuscated functions

## Scripting and Automation

### Using the Built-in Python Console

Access via View -> Console or **Ctrl+** **`**

Example:
```python
for f in bv.functions:
    print(f.name)
```

### Automating Analysis
Sample script to list all network-related strings:

```python
from binaryninja import BinaryViewType

bv = BinaryViewType.get_view_of_file("malware_sample.exe")

for s in bv.strings:
    if b"http" in s.value or b"socket" in s.value:
        print(s)
```


---
[Back to the Section](/courseFiles/Section_11-malwareForensics/malwareForensics.md)



