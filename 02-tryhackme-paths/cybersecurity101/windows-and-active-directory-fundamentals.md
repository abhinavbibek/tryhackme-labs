# Windows Fundamentals 

## Windows Desktop (GUI)

The Windows Desktop (GUI) is the main screen you see **after logging in**.
Before this, you must pass the **login screen** by entering valid credentials (username & password).
This can be a local account or an Active Directory account (domain-joined system).

### Main Components of the Windows GUI

| Component | Description |
|--------|-------------|
| Desktop | Area with shortcuts to files, folders, and apps |
| Start Menu | Access to apps, settings, power options |
| Search Box | Used to search apps, files, and settings |
| Task View | View and manage multiple desktops |
| Taskbar | Shows running and pinned applications |
| Toolbars | Extra tools (can be enabled/disabled) |
| Notification Area | Date, time, network, volume icons |

---

## Desktop

The desktop contains shortcuts to:
- Programs
- Files
- Folders

Icons may be organized or randomly placed for quick access.

Right-click on the desktop to:
- Change icon size
- Arrange icons
- Create new items
- Open Display settings
- Personalize wallpaper, theme, colors

Notes:
- Display settings allow resolution and multi-monitor setup
- Some settings are disabled in Remote Desktop sessions

---

## Start Menu

In Windows 10, the Start Menu is opened using the **Windows logo** (not the word “Start”).

### Start Menu Sections

#### Left Section
Provides shortcuts for:
- User account settings
- Lock / Sign out
- Documents & Pictures folders
- Settings
- Power options (Shutdown / Restart)

#### Middle Section
Shows:
- Recently added applications
- All installed apps (alphabetical order)

Tip:
- Clicking a letter opens an alphabet grid for faster navigation

#### Right Section (Tiles)
- Contains pinned apps (tiles)
- Tiles can be resized or removed
- Apps can be pinned using **Right-click → Pin to Start**

---

## Taskbar

The taskbar shows:
- Running applications
- Pinned apps

Features:
- Hovering over icons shows previews
- Closed apps disappear unless pinned
- Can be customized by right-clicking the taskbar

---

## Notification Area

Located at the bottom-right corner.
Displays:
- Date & time
- Network status
- Volume
- Background system icons

Icons can be added or removed from **Taskbar settings**.

---

## Windows File System (NTFS)

Modern Windows systems use **NTFS (New Technology File System)**.

Older file systems:
- FAT16 / FAT32
- HPFS

FAT is still used in:
- USB drives
- Memory cards

### NTFS Features

| Feature | Description |
|-----|-------------|
| Large files | Supports files > 4GB |
| Permissions | File & folder access control |
| Compression | Save disk space |
| Encryption | EFS (Encrypting File System) |
| Journaling | Auto-recovery after crashes |

To check file system:
- Right-click C:\ drive → Properties

---

## NTFS Permissions

Common permissions include:

| Permission | Meaning |
|---------|--------|
| Full Control | Complete access |
| Modify | Change files |
| Read & Execute | Open and run |
| List Folder Contents | View files |
| Read | View content |
| Write | Create or edit |

To view permissions:
```text
Right-click file/folder → Properties → Security tab
```

## Alternate Data Streams (ADS)

Alternate Data Streams (ADS) is an **NTFS feature** that allows multiple data streams to be associated with a single file.  
Windows Explorer does **not** show ADS by default.

### Security Note
- Malware can hide data inside ADS

### Legitimate Use
- Files downloaded from the internet are often marked using ADS

---

## Windows Folder Structure

The main Windows operating system files are stored in:
```text
C:\Windows
```

### Key Folder
```text
C:\Windows\System32
```

### Important Notes
- `System32` contains critical OS files
- Deleting files here can break Windows
- Many essential system tools are stored in this directory

### System Environment Variable
```text
%windir%
```

---

## User Accounts

There are two main types of user accounts:

| Type | Capabilities |
|----|-------------|
| Administrator | Full system control |
| Standard User | Limited to personal files |

### Administrators Can
- Add or remove users
- Install software
- Change system-wide settings

### Standard Users
- Cannot perform system-level changes
- Are restricted to personal files and settings

---

## User Profiles

User profiles are stored in:
```text
C:\Users\
```

### Example
```text
C:\Users\Max
```

A user profile is created when the user logs in for the **first time**.

### Common Folders in a Profile
- Desktop
- Documents
- Downloads
- Music
- Pictures

---

## Local Users & Groups

Access using:
```text
Run → lusrmgr.msc
```

You can view:
- Users
- Groups

### Notes
- Groups have permissions
- Users inherit permissions from groups
- A user can belong to multiple groups

---

## User Account Control (UAC)

User Account Control (UAC) helps protect the system from **unauthorized changes**.

### How It Works
- Administrator users run with limited privileges by default
- Elevated actions trigger a permission prompt

### Visual Indicator
- Shield icon on applications that require elevation

### Important Note
- The built-in **Administrator** account is not affected by UAC by default

---

## Settings vs Control Panel

| Settings | Control Panel |
|--------|---------------|
| Modern interface | Classic interface |
| Easier options | Advanced settings |
| Default in Windows 10 | Still required for some tasks |

Both can be accessed from the **Start Menu**.  
In some cases, Settings redirects to Control Panel.

---

## Task Manager

Task Manager provides information about:
- Running processes
- CPU usage
- RAM usage
- Performance statistics

### Access Method
```text
Right-click taskbar → Task Manager
```

### Views
- **Simple View** – Basic information
- **More Details** – Full system and process information
