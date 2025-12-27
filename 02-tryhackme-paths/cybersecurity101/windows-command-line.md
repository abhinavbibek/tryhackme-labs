## Why CLI Is Useful

Most people prefer a GUI at first because it feels easier and more visual. You can click around and figure things out without much effort. The CLI can feel harder initially because it requires remembering commands.

However, once you become comfortable with the CLI, it becomes **faster and more efficient**.  
For example, checking your IP address using the GUI requires multiple clicks, whereas in the CLI it requires just a single command. Repeating tasks is also significantly easier in the CLI.

### Advantages of CLI
- Uses fewer system resources (ideal for old systems or servers)
- Faster once commands are known
- Easy to automate using scripts
- Best suited for remote management using SSH
- Works well even on slow or unstable networks

---

## Windows Command Prompt Basics

Windows Command Prompt (`cmd.exe`) is the default CLI in Windows. Using it, you can:
- View system information
- Check network configuration
- Troubleshoot connectivity issues
- Manage files and folders
- View and control running processes

Commands execute from locations defined in the system **PATH** environment variable.

Environment variables can be viewed using:
```text
set
```

### Useful Basic Commands
```text
help        → Shows help for commands
cls         → Clears the screen
command /?  → Shows help for a specific command
```

---
## System Information Commands

### Windows Version
```text
ver
```
Displays the Windows OS version.

### Detailed System Information
```text
systeminfo
```
Displays:
- OS version
- Hostname
- Memory
- Processor
- Installed updates

If the output is too long:
```text
command | more
```
Allows scrolling page by page.

---

## Network Configuration

### Basic Network Info
```text
ipconfig
```
Shows:
- IP address
- Subnet mask
- Default gateway

### Detailed Network Info
```text
ipconfig /all
```
Shows:
- DNS servers
- MAC address
- DHCP status
- Lease information

---

## Network Troubleshooting Commands

### Ping
```text
ping target
```
Checks whether a system is reachable.

### Traceroute
```text
tracert target
```
Shows the path packets take to reach a destination.

### DNS Lookup
```text
nslookup domain
```
Resolves a domain name to an IP address.



