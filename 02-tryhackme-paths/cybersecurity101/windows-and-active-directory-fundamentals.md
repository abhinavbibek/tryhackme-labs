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
