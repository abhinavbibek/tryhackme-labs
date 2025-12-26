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

## System Configuration (`msconfig`)

The System Configuration utility (`msconfig`) is mainly used for **advanced troubleshooting**, especially for issues related to **startup and booting**.

### Important Note
- Requires **local administrator privileges** to open

### One Way to Open
```text
Start Menu → Search "msconfig"
```

---

## MSConfig Tabs Overview

MSConfig has five main tabs:

| Tab | Purpose |
|----|--------|
| General | Control startup mode |
| Boot | Configure OS boot options |
| Services | View and manage system services |
| Startup | Startup applications (client OS mainly) |
| Tools | Access system utilities |

---

## General Tab

Used to regulate what Windows loads during startup.

### Startup Options
- **Normal** – Loads all device drivers and services
- **Diagnostic** – Loads only basic devices and services
- **Selective** – Allows manual selection of what to load

> Commonly used during **boot troubleshooting**.

---

## Boot Tab

Used to configure:
- Boot parameters
- Safe boot options
- OS startup behavior

Helpful for diagnosing **boot-related issues**.

---

## Services Tab

Displays all system services:
- Running
- Stopped

### Notes
- Includes **Windows** and **third-party** services
- Helpful in identifying **suspicious or unwanted services**
- Background applications usually run as services

---

## Startup Tab (Windows Server)

In Windows Server:
- Startup tab shows little or no useful information
- Startup apps are handled differently than in Windows 10/11

### Startup Items Location
```text
Win + R → shell:startup
```

This folder contains:
- Shortcuts
- Executables that run automatically on login

---

## Tools Tab

Provides shortcuts to many Windows utilities.

### Features
- Short description of each tool
- Command used to launch it

Tools can be launched via:
- Run dialog
- Command Prompt
- Launch button in MSConfig

---

## Advanced System Settings

### Access Via
```text
Search → Advanced system settings
```

Used to manage:
- Performance
- Virtual memory
- Startup & recovery

---

## Virtual Memory (Page File)

Windows uses a **page file** as additional memory when RAM is insufficient.

### Location
```text
Advanced → Performance → Settings → Advanced
```

### Information Available
- Drive location
- Initial size
- Maximum size
- Auto-managed or manual configuration

---

## Startup and Recovery

Controls system behavior during Windows crashes.

### Crash Dump Options

| Dump Type | Description |
|--------|-------------|
| Automatic | Default option |
| Kernel | Kernel memory only |
| Small (256 KB) | Minimal information |
| Complete | Full memory dump |
| None | No dump created |

Used for debugging **Blue Screen of Death (BSOD)** issues.

---

## User Account Control (UAC)

UAC prevents **unauthorized system changes**.

### UAC Levels

| Level | Description |
|----|-------------|
| Always notify | Highest security |
| Notify for apps | Default |
| Notify without dimming | Less secure |
| Never notify | Not recommended |

### Notes
- Built-in **Administrator** bypasses UAC by default
- Shield icon indicates elevated privileges are required

---

## Computer Management (`compmgmt.msc`)

Computer Management has three main sections:

| Section | Purpose |
|------|--------|
| System Tools | Monitoring and management |
| Storage | Disk management |
| Applications & Services | Services and WMI |

---

## System Tools

### Task Scheduler
Used to run tasks:
- On login
- On logoff
- On a schedule


Create a task:
```text
Actions → Create Basic Task
```

---

### Event Viewer

Displays system logs.

#### Panes
- Left: Log providers
- Middle: Event details
- Right: Actions

Standard logs located under:
```text
Windows Logs
```

Used for:
- Troubleshooting
- Security investigations

---

### Shared Folders

Shows:
- Shared folders
- Active sessions
- Open files

Default shares:
```text
C$
ADMIN$
```

Administrators have access to these shares.

---

### Performance Monitor (`perfmon`)

Used for:
- Real-time monitoring
- Performance analysis using logs

Helpful in diagnosing:
- CPU issues
- Memory bottlenecks

---

### Device Manager

Used to:
- View hardware
- Disable devices
- Update drivers

---

## Storage

### Disk Management

Used for:
- Creating partitions
- Extending or shrinking volumes
- Assigning drive letters

> Additional tools may be available in Windows Server.

---

## Services and Applications

### Services

Background applications that run automatically or manually.

| Startup Type | Meaning |
|------------|---------|
| Automatic | Starts at boot |
| Manual | Starts when needed |
| Disabled | Never starts |

---

### WMI Control

Windows Management Instrumentation (WMI):
- Used for scripting
- System administration and monitoring

---

## System Information (`msinfo32`)

Displays detailed system information.

### Sections
- Hardware Resources
- Components
- Software Environment

Useful for:
- Hardware details
- Installed software
- Environment variables

Includes a **search bar** at the bottom.

---

## Environment Variables

Store OS-related values.

### Example
```text
%WINDIR%
```

Used by programs to determine system paths.

### View Via
```text
Advanced system settings → Environment Variables
```

---

## Resource Monitor (`resmon`)

Advanced troubleshooting tool.

### Sections
- CPU
- Memory
- Disk
- Network

Each section provides:
- Detailed process usage
- Real-time graphs

Used to identify:
- Performance bottlenecks
- Hung processes

---

## Command Prompt (`cmd`)

Text-based interface to Windows.

### Basic Commands
```text
hostname   → Displays computer name
whoami     → Shows logged-in user
ipconfig   → Network information
cls        → Clear screen
```

### Help Command
```text
command /?
```

Example:
```text
ipconfig /?
```

---

## Net Commands

- `netstat` – Displays network connections
- `net` – Manages network resources

### Help
```text
net help
net help user
```

### Common Subcommands
- user
- localgroup
- share
- session

---

## Windows Registry

Central database that stores:
- User profiles
- Installed software
- Hardware information
- System settings

### Access Via
```text
regedit
```

## Windows Update

Windows Update is a Microsoft service used to provide **security patches, bug fixes, and feature upgrades** to Windows and other Microsoft software such as **Microsoft Defender**.

Patches are normally issued on the **second Tuesday of each month**, referred to as **Patch Tuesday**.  
If a vulnerability is critical, Microsoft may release an update immediately instead of waiting.

### Where to Find Windows Update
- **Settings**
- **Run dialog or Command Prompt** (command name works regardless of location)

> Importantly, when the command name is specified, the location of the command is not used.

### Update Behavior
- Some updates require a **system restart**
- Updates can be **delayed** in modern Windows versions
- Delayed updates will eventually be installed to maintain system security

---

## Windows Security

Windows Security is a built-in interface used to manage **system and user data protection**.

### Main Protection Areas
- Virus & threat protection
- Firewall & network protection
- App & browser control
- Device security

### Status Indicators
- **Green** – No action needed
- **Yellow** – Recommendation available
- **Red** – Immediate action required

---

## Virus & Threat Protection

Responsible for **malware prevention and detection**.

### Scan Options
- **Quick scan** – Scans common threat locations
- **Full scan** – Scans all files and executables
- **Custom scan** – Scans user-selected files or folders

### Threat History
- **Last scan** – Shows recent scan activity
- **Quarantined threats** – Blocked from execution
- **Allowed threats** – Approved by user (not recommended unless necessary)

### Key Settings
- **Real-time protection** – Continuous malware protection
- **Cloud-based protection** – Faster detection using cloud intelligence
- **Automatic sample submission** – Sends suspicious files to Microsoft
- **Controlled folder access** – Prevents unauthorized changes to important folders
- **Exclusions** – Items ignored during scans (use sparingly)
- **Notifications** – Security alerts and messages

---

## Firewall & Network Protection

A firewall controls **incoming and outgoing network traffic** based on security rules.

### Firewall Profiles
- **Domain** – Used when connected to a corporate domain
- **Private** – Used for trusted home or private networks
- **Public** – Used for public networks (cafes, airports)

Each profile allows:
- Enabling or disabling the firewall
- Blocking incoming connections

> It is strongly recommended to keep the firewall **enabled** unless absolutely necessary.

---

## App & Browser Control

Manages **Microsoft Defender SmartScreen**.

SmartScreen protects against:
- Malicious websites
- Phishing attacks
- Unverified applications and downloads

### Settings
- **Warn** – Displays a warning before execution
- **Block** – Prevents execution completely
- **Off** – Disables protection (not recommended)

---

## Device Security

### Exploit Protection
- Protects against common exploitation techniques
- Default settings are sufficient for most users

### Core Isolation (Memory Integrity)
- Prevents malicious code execution in secure processes
- Helps protect against kernel-level attacks

---

## Trusted Platform Module (TPM)

TPM is a **hardware-based security component** that performs cryptographic operations securely.

### Functions
- Protects encryption keys
- Ensures system integrity
- Enhances security features like BitLocker

---

## BitLocker

BitLocker is a **disk encryption feature** that protects data from unauthorized access.

### Key Points
- Encrypts entire drives
- Protects data in case of loss or theft
- Works best when integrated with TPM

---

## Volume Shadow Copy Service (VSS)

VSS creates **snapshots of files or volumes** at specific points in time.

### Uses
- System Restore
- Backup consistency
- File recovery

### Security Note
- Malware (especially ransomware) may delete shadow copies to prevent recovery
- Reliable protection requires **offline or external backups**

---
## Windows Domain – Basics

In a very small network, managing computers individually is possible. Each computer can be configured manually, users can be created locally, and issues can be fixed in person. However, as an organisation grows, this approach becomes impractical.

When a network scales to **hundreds of computers and users across multiple locations**, manual configuration and on-site support are no longer efficient. This is where a **Windows Domain** becomes useful.

A **Windows domain** is a collection of users and computers managed **centrally**. Instead of managing each system separately, administration is done from a single location using **Active Directory (AD)**.

---

## Active Directory and Domain Controller

**Active Directory (AD)** is a central database that stores information about all objects in a Windows domain.

Objects stored include:
- Users
- Computers
- Groups
- Printers
- Other network resources

The server that runs Active Directory services is called a **Domain Controller (DC)**.

### Role of a Domain Controller
- Handles authentication
- Handles authorization
- Enforces domain-wide policies

All logins and security checks go through the **Domain Controller**.

---

## Advantages of a Windows Domain

### Centralised Identity Management
- Users are created and managed in Active Directory
- No need to create accounts on individual machines

### Centralised Security Policies
- Security settings can be applied across the entire network from one place
- Policies are enforced consistently

### Consistent User Experience
- Users can log in to multiple computers using the same credentials

### Real-World Example
In schools or workplaces:
- One username and password works on many computers
- Restrictions (such as blocked Control Panel access) are enforced using **domain policies**

---

## Active Directory Objects

Active Directory stores different types of objects. The most important ones are:
- Users
- Machines (Computers)
- Groups

---

## Users

Users are **security principals**, meaning they can authenticate to the domain and access resources.

### Types of Users
- **Human users** – Represent employees or individuals
- **Service users** – Used by services (web servers, databases) with limited permissions

Users can be assigned permissions to:
- Files
- Printers
- Network resources

---

## Machines (Computers)

Each computer that joins the domain receives a **machine account** in Active Directory.

### Machine Account Properties
- Are security principals
- Have limited domain permissions
- Are local administrators on their own computer
- Have passwords that are automatically rotated

### Machine Account Naming Format
```text
ComputerName$
```

Example:
```text
PC01$
```

---

## Security Groups

Security groups are used to **assign permissions** to resources.

### Key Points
- Groups can contain users, computers, or other groups
- Permissions are assigned to groups, not individual users
- Users can belong to multiple groups

### Important Default Domain Groups

| Group Name | Description |
|---------|-------------|
| Domain Admins | Full administrative control over the entire domain |
| Server Operators | Manage domain controllers (limited admin rights) |
| Backup Operators | Access files for backup purposes |
| Account Operators | Create and modify user accounts |
| Domain Users | All regular users in the domain |
| Domain Computers | All computers joined to the domain |
| Domain Controllers | All domain controller machines |

---

## Active Directory Users and Computers (ADUC)

**ADUC** is the primary management tool for Active Directory.

Using ADUC, administrators can:
- Create and delete users
- Reset passwords
- Manage groups
- Organise directory objects

Objects in ADUC are organised into **Organizational Units (OUs)**.

---

## Organizational Units (OUs)

OUs are **container objects** used to organise users and computers.

### Purpose of OUs
- Apply policies to users or computers
- Reflect organisational structure (IT, Sales, HR, etc.)

### Important Points
- A user or computer can belong to **only one OU**
- OUs are mainly used for **policy management**

### Default Containers in Active Directory

| Container | Purpose |
|--------|---------|
| Builtin | Default system groups |
| Computers | Default location for new computers |
| Domain Controllers | Contains domain controllers |
| Users | Default domain-wide users and groups |
| Managed Service Accounts | Service-related accounts |

---

## Security Groups vs Organizational Units

Although both are used to organise objects, they serve **different purposes**.

### Organizational Units (OUs)
- Used for applying **policies**
- Control system behaviour and restrictions
- Objects can belong to **only one OU**

### Security Groups
- Used for **access control**
- Grant permissions to resources
- Users can belong to **multiple groups**

### Example
- Use an **OU** to apply password policies to the IT department
- Use a **group** to grant access to a shared folder or printer

---

## Active Directory Maintenance and Delegation

As a domain administrator, one of the first responsibilities is to review the existing **Active Directory (AD)** structure and ensure it matches the organisation’s current layout. Over time, departments may close, users may leave, and new roles may be added, so AD must be kept **clean and accurate**.

---

## Removing Unnecessary OUs and Users

Sometimes departments are closed due to business decisions such as budget cuts. If an OU no longer exists in the organisation chart, it should be removed from Active Directory.

By default, **Organizational Units (OUs)** are protected against accidental deletion. This prevents administrators from unintentionally deleting entire departments along with their users and groups.

### Steps to Delete an OU
1. Enable **Advanced Features** from the *View* menu  
2. Open the **OU properties**  
3. Disable **Protect object from accidental deletion**  
4. Delete the OU (this also deletes all objects inside it)

After removing unused OUs:
- Review all user accounts
- Delete users who no longer exist in the organisation
- Create missing users as needed

The goal is to ensure Active Directory matches the **official organisational chart**.

---

## Delegation in Active Directory

Delegation allows specific users to perform **limited administrative tasks** without granting full **Domain Admin** privileges.

This follows the **principle of least privilege**, improving overall security.

### Common Delegation Scenario
Allow IT support staff to:
- Reset user passwords
- Unlock user accounts

This avoids involving Domain Administrators in routine helpdesk tasks.

### Scope of Delegation
- Delegation is applied at the **OU level**
- Delegated users can manage only the objects inside that OU

### Example Use Case
- An IT support lead is allowed to reset passwords for users in:
  - Sales OU
  - Marketing OU
  - Management OU
- The delegated permissions do **not** apply outside these OUs

This approach makes administration **scalable and secure**.

---

## Password Reset After Delegation

Once delegation is configured, the delegated user can reset passwords for users within the assigned OU.

### Best Practices After Password Reset
- Force the user to **change password at next logon**
- Avoid continued use of passwords known by IT staff

This ensures **confidentiality and security**.

---

## Computer Accounts and the Computers Container

By default, all domain-joined machines (except Domain Controllers) are placed in the **Computers** container.

This includes:
- Workstations
- Laptops
- Servers

Keeping all machines in a single container is **not recommended**, as different device types require different security policies.

---

## Recommended Device OU Structure

A best practice is to separate machines based on their **role**.

### Typical Device Categories

#### Workstations
- Used by employees for daily tasks
- Browsing, documents, and applications
- Should not have privileged accounts logged in

#### Servers
- Provide services such as:
  - File sharing
  - Databases
  - Applications
- Require stricter security controls

#### Domain Controllers
- Manage authentication and Active Directory
- Most sensitive systems in the domain
- Stored in a dedicated OU by default

---

## Improving AD Structure for Devices

To improve management and policy control:

1. Create a **Workstations OU**
2. Create a **Servers OU**
3. Leave **Domain Controllers** in their default OU

### After Creating the OUs
- Move employee PCs and laptops into the **Workstations OU**
- Move server machines into the **Servers OU**

This enables:
- User-focused policies for workstations
- Stronger hardening and security policies for servers

---
## Group Policy Objects (GPO) – Meaning and Idea

It is not sufficient to only organise users and computers into **Organizational Units (OUs)**.  
The real purpose is to apply **different policies to different groups of users or machines**. This allows administrators to enforce department-specific settings, security rules, and restrictions.

Windows implements these policies using **Group Policy Objects (GPOs)**.

A **GPO** is a collection of policies and configurations that control the behaviour of users and computers in a domain.

GPOs can apply to:
- **Users** (User Configuration)
- **Computers** (Computer Configuration)

This separation allows administrators to manage **identity-based** and **machine-based** policies independently.

---

## Overview: Group Policy Management

The **Group Policy Management** tool is used to manage GPOs.  
It displays the complete Active Directory hierarchy, including:
- Domains
- Organizational Units (OUs)
- Existing GPOs

### General Procedure for Using GPOs
1. Create a GPO in the **Group Policy Objects** container  
2. Configure the required settings within the GPO  
3. Link the GPO to the appropriate **OU or domain**

A GPO only affects the location where it is linked, but it also applies to **all child OUs** beneath that location.  
This inheritance behaviour is critical when designing policy structures.

---

## Default GPOs and Scope

Every domain contains default GPOs such as:
- **Default Domain Policy** – applies domain-wide
- **Default Domain Controllers Policy** – applies only to domain controllers

### GPO Scope Defines
- Where the GPO is linked
- Which users or computers it applies to

### Security Filtering
Security Filtering can restrict a GPO so that it applies only to specific users or computers, even within the same OU.

By default, GPOs apply to **all authenticated users and computers**.

---

## GPO Settings Structure

Each GPO contains two major sections:

- **Computer Configuration** – applies only to machines
- **User Configuration** – applies only to users

This design ensures:
- If a GPO is linked to an OU containing only users, computer settings are ignored
- If linked to an OU containing only computers, user settings are ignored

### Example
Password policies are typically **computer-based policies**, not user-based, because they apply at the system level.

---

## Awareness and Editing of GPO Policies

When editing a GPO, administrators navigate structured policy paths to configure:
- Security settings
- System behaviour
- User restrictions

Policy explanations usually include:
- What the policy does
- When it should be enabled
- Possible side effects

> It is strongly recommended to read policy descriptions carefully, especially for **domain-wide policies**.

---

## GPO Distribution and Updates

GPOs are stored in a shared domain folder called **SYSVOL**.

### SYSVOL Characteristics
- Present on all domain controllers
- Stores all policy data

Computers and users periodically refresh policies from SYSVOL (approximately every **90–120 minutes** by default).

### Immediate Policy Refresh
If changes must apply immediately, a **manual policy refresh** can be triggered on a system.

---

## Example Scenario: Restricting Control Panel Access

A common requirement is preventing non-IT users from accessing system configuration tools such as the **Control Panel**.

This is achieved using a **user-based GPO** that:
- Denies access to Control Panel and system settings
- Is linked only to OUs containing non-IT users

Because the GPO is not linked to IT OUs, IT users remain unaffected.  
This maintains **least privilege** and avoids unnecessary complexity.

---

## Real-Life Application: Automatic Screen Lock

Another important security requirement is forcing systems to lock after inactivity.

This is implemented using a **computer-based GPO** that:
- Sets an inactivity timeout
- Automatically locks the screen after the specified time

This GPO can be linked at the **domain level**, ensuring all computers inherit it.  
User OUs do not affect this policy because it applies only to computers.

---

## Windows Domain Authentication

Domain Controllers store all domain credentials and authenticate users when they access resources.

Windows domains support two authentication protocols:
- **Kerberos** (default and modern)
- **NetNTLM** (legacy compatibility)

---

## Kerberos Authentication (Modern Standard)

Kerberos uses a **ticket-based authentication system**, avoiding repeated transmission of credentials.

### High-Level Flow
1. User authenticates and receives a **Ticket Granting Ticket (TGT)**
2. TGT is used to request service-specific tickets
3. Services grant access based on valid service tickets

### Security Benefits
- Reduces password exposure
- Supports mutual (two-way) authentication
- Limits ticket scope and lifetime

---

## NetNTLM Authentication (Legacy)

NetNTLM uses a **challenge-response** mechanism:
1. Server sends a challenge
2. Client responds using a hash
3. Domain Controller verifies the response

Although passwords are not sent directly, NetNTLM is weaker than Kerberos and vulnerable to:
- Relay attacks
- Replay attacks

It exists mainly for **backward compatibility**.

---

## Scaling Active Directory: Domains, Trees, and Forests

### Single Domain
Suitable for small organisations, but becomes difficult to manage as complexity grows.

---

### Trees
A **tree** is a collection of domains sharing a common namespace.

Benefits:
- Departmental or regional separation
- Independent administration
- Automatic trust within the namespace

Each domain has its own Domain Admins, while **Enterprise Admins** manage the entire tree.

---

### Forests
A **forest** consists of multiple domain trees, possibly with different namespaces.

Common in:
- Mergers
- Acquisitions

Trees are administratively independent but can establish trusts for collaboration.

---

## Trust Relationships

Trusts allow users from one domain to access resources in another domain.

### Key Points
- Trusts do not automatically grant access
- Permissions must still be explicitly assigned
- **One-way trust** → access in one direction
- **Two-way trust** → mutual access

Trusts enable collaboration while preserving security boundaries.

---

## Key Takeaways

- GPOs are the primary mechanism for enforcing security and configuration in Windows domains
- Effective OU design is critical for successful policy deployment
- Kerberos is the preferred authentication protocol
- Multi-domain structures improve scalability and administrative control
- Trusts allow controlled cross-domain access without compromising security

