# Windows PowerShell

These are **simple learner-written notes** for understanding PowerShell basics.  
Written for reference while studying **Windows and Cyber Security concepts**.

---

## What is PowerShell?

PowerShell is a **command-line shell and scripting language** developed by Microsoft.  
It is used for **automation, system management, and administration**.

**Official definition (simplified):**  
PowerShell is a cross-platform automation and configuration tool built on **.NET**.

### Key Difference from CMD
- **CMD** → Text-based
- **PowerShell** → **Object-based**

PowerShell works with structured objects instead of plain text.

---

## Why PowerShell Is Powerful

- Handles complex system data easily
- Automates repetitive administrative tasks
- Deep integration with Windows
- Works on **Windows, Linux, and macOS**
- Widely used in **cyber security**

---

## Brief History

- Created to overcome CMD limitations
- Designed by **Jeffrey Snover**
- Released in **2006**
- **PowerShell Core** released in **2016** (open-source and cross-platform)

---

## Objects in PowerShell

PowerShell outputs **objects**, not text.

An object contains:
- **Properties** → Data (Name, Size, Status)
- **Methods** → Actions (Stop, Copy, Remove)

### Example Concept
A file object may include:
- Properties: Name, Size, Extension
- Methods: Copy(), Delete()

This avoids unreliable text parsing.

---

## PowerShell Prompt

When PowerShell starts, the prompt looks like:
```powershell
PS C:\Users\User>
```

`PS` indicates an active PowerShell session.

---

## Cmdlets (Verb-Noun)

PowerShell commands are called **cmdlets**.

### Naming Format
```text
Verb-Noun
```

### Examples
- Get-Content
- Set-Location
- Get-Process

This naming convention makes commands easier to understand and guess.

---

## Discovering Commands

### List All Commands
```powershell
Get-Command
```

### Filter by Type
```powershell
Get-Command -CommandType Function
```

---

## Getting Help

One of the most important PowerShell features.

```powershell
Get-Help Get-Date
```

### Useful Help Options
```powershell
Get-Help Get-Date -Examples
Get-Help Get-Date -Detailed
Get-Help Get-Date -Full
```

---

## Aliases

Aliases are shortcuts for cmdlets.

| Alias | Actual Cmdlet |
|----|---------------|
| dir | Get-ChildItem |
| cd | Set-Location |
| cat | Get-Content |

### List All Aliases
```powershell
Get-Alias
```

---

## File System Commands

### List Files
```powershell
Get-ChildItem
```

### Change Directory
```powershell
Set-Location path
```

### Create Files and Folders
```powershell
New-Item -Path file.txt -ItemType File
New-Item -Path folder -ItemType Directory
```

### Delete Items
```powershell
Remove-Item file.txt
```

### Copy and Move
```powershell
Copy-Item source destination
Move-Item source destination
```

### Read File Contents
```powershell
Get-Content file.txt
```

---

## Piping (`|`)

Piping sends the output of one cmdlet as input to another.

```powershell
Get-ChildItem | Sort-Object Length
```

PowerShell pipes **objects**, not text.

---

## Filtering Data

### Where-Object
```powershell
Get-ChildItem | Where-Object Extension -eq ".txt"
```

### Common Operators
- `-eq` → Equal
- `-ne` → Not equal
- `-gt` → Greater than
- `-ge` → Greater or equal
- `-lt` → Less than
- `-le` → Less or equal
- `-like` → Pattern matching

---

## Select-Object

Used to display only specific properties.

```powershell
Get-ChildItem | Select-Object Name, Length
```

---

## Select-String (grep Equivalent)

```powershell
Select-String -Path file.txt -Pattern "text"
```

Supports regular expressions.

---

## System Information

### Full System Details
```powershell
Get-ComputerInfo
```

### Local Users
```powershell
Get-LocalUser
```

---

## Network Information

### Network Configuration
```powershell
Get-NetIPConfiguration
```

### IP Addresses
```powershell
Get-NetIPAddress
```

---

## Process and Service Monitoring

### Running Processes
```powershell
Get-Process
```

### Services Status
```powershell
Get-Service
```

### TCP Connections
```powershell
Get-NetTCPConnection
```

Useful for:
- Incident response
- Malware analysis
- Threat hunting

---

## File Hashing

Generate a file hash:
```powershell
Get-FileHash file.txt
```

Used for:
- Integrity checking
- Malware analysis
- Incident response

---

## Alternate Data Streams (ADS)

View file streams:
```powershell
Get-Item file.txt -Stream *
```

ADS can hide data inside files (NTFS feature).

---

## Scripting Basics

PowerShell scripts:
- Saved with `.ps1` extension
- Automate tasks
- Reduce manual work

Used heavily by:
- Blue Team (incident response, threat hunting)
- Red Team (enumeration, automation)
- System administrators

---

## Invoke-Command (Remote Execution)

Runs commands on remote systems.

```powershell
Invoke-Command -ComputerName Server01 -ScriptBlock { Get-Process }
```

### Why It Is Important
- Remote administration
- Automation at scale
- Used by both attackers and defenders
