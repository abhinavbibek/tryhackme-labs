# Nmap Basics

## Introduction

While working on networks, two very common questions come up:

- Which devices are currently alive on a network?
- What services (like SSH, HTTP, DNS) are running on those devices?

Doing this manually using tools like `ping`, `telnet`, or `arp-scan` is slow, limited, and unreliable. Firewalls may block ICMP, ARP works only on local networks, and testing thousands of ports manually is not practical.

**Nmap (Network Mapper)** solves these problems. It is an open-source network scanning tool released in **1997** and is widely used for:
- Host discovery
- Port scanning
- Service detection
- OS fingerprinting

---

## Target Specification in Nmap

Nmap allows multiple ways to define targets:

- **IP range:** `192.168.0.1-10`
- **Subnet (CIDR):** `192.168.0.0/24`
- **Hostname:** `example.thm`

> Always double-check targets before scanning to avoid mistakes.

---

## Host Discovery – Finding Live Hosts

**Purpose:** Identify which hosts are online without scanning ports.

### Ping Scan (`-sn`)

Checks if hosts are alive but does not scan ports.

```bash
nmap -sn 192.168.66.0/24
```

### Local Network Scanning

When scanning a directly connected network (Ethernet/WiFi):

- Nmap sends **ARP requests**
- Hosts replying to ARP are marked as **up**
- MAC addresses and vendor names are shown

This method is **very fast and accurate**.

### Remote Network Scanning

When scanning networks behind routers:

- ARP cannot be used
- Nmap uses:
  - ICMP echo
  - ICMP timestamp
  - TCP SYN
  - TCP ACK probes
- Firewalls may block some probes

```bash
nmap -sn 192.168.11.0/24
```

Even if ICMP is blocked, Nmap may still discover hosts using TCP/UDP probes.

---

## List Scan (`-sL`)

Lists targets **without scanning them**.

```bash
nmap -sL 192.168.0.0/24
```

Useful for confirming scope before running real scans.

---
## Port Scanning – Finding Open Services

Once live hosts are identified, the next step is discovering open ports.

### TCP Connect Scan (`-sT`)

- Completes full TCP three-way handshake
- Less stealthy
- Default when run **without sudo**

```bash
nmap -sT 192.168.1.10
```

### TCP SYN Scan (`-sS`)

- Sends only SYN packet
- Does not complete handshake
- More stealthy
- Requires root privileges

```bash
sudo nmap -sS 192.168.1.10
```

### UDP Scan (`-sU`)

- Used for UDP services (DNS, NTP, SNMP)
- Slower and noisier
- Closed ports usually reply with ICMP “port unreachable”

```bash
sudo nmap -sU 192.168.1.10
```

---

## Limiting Port Scope

By default, Nmap scans the **top 1000** common ports.

Useful options:

- `-F` → Fast scan (top 100 ports)
- `-p 1-1024` → Specific range
- `-p-` → All 65,535 ports

```bash
nmap -sS -p- 192.168.1.10
```

---

## Service and OS Detection

### OS Detection (`-O`)

Uses TCP/IP fingerprinting to guess the OS.

```bash
sudo nmap -O 192.168.1.10
```

### Service Version Detection (`-sV`)

Detects service names and versions.

```bash
nmap -sS -sV 192.168.1.10
```

### Aggressive Scan (`-A`)

Enables:
- OS detection
- Version detection
- Traceroute
- Default scripts

```bash
sudo nmap -A 192.168.1.10
```

---

## Forcing Scans (`-Pn`)

If a host does not respond to ping, Nmap may mark it as down.  
`-Pn` forces scanning anyway.

```bash
nmap -sS -Pn 192.168.1.10
```

Useful when ICMP is blocked.

---

## Timing and Performance Control

Timing templates:

- `-T0` → Paranoid (very slow)
- `-T1` → Sneaky
- `-T2` → Polite
- `-T3` → Normal (default)
- `-T4` → Aggressive
- `-T5` → Insane (very fast, noisy)

```bash
nmap -sS -T4 192.168.1.10
```

Advanced options:
- `--min-parallelism`, `--max-parallelism`
- `--min-rate`, `--max-rate`
- `--host-timeout`

---

## Verbosity and Debugging

### Verbose Output (`-v`)

Shows scan progress.

```bash
nmap -sS -v 192.168.1.0/24
```

- Increase with `-vv`, `-v4`
- Press `v` during scan to increase verbosity live

### Debug Mode (`-d`)

Shows internal scan logic.

```bash
nmap -d9 192.168.1.10
```

Mostly used for learning or troubleshooting.

---

## Saving Scan Results

Output formats:

- `-oN file.nmap` → Normal
- `-oX file.xml` → XML
- `-oG file.gnmap` → Grepable
- `-oA basename` → All formats

```bash
nmap -sS 192.168.1.1 -oA scan_result
```

Creates:
- `scan_result.nmap`
- `scan_result.xml`
- `scan_result.gnmap`

---


## Key Takeaways

- Nmap is a powerful and flexible scanner
- Host discovery and port scanning are separate
- SYN scan is preferred with sudo
- UDP scanning is important but slower
- Timing and verbosity affect behavior
- Always confirm targets before scanning
- Use `sudo` to unlock full capabilities

---

## Quick Reference

### Host Discovery
- `-sn` → Ping scan
- `-sL` → List scan

### Port Scanning
- `-sT` → TCP connect
- `-sS` → TCP SYN
- `-sU` → UDP scan
- `-F` → Fast scan
- `-p-` → All ports

### Detection
- `-O` → OS detection
- `-sV` → Service versions
- `-A` → Aggressive scan

### Control
- `-Pn` → Skip host discovery
- `-T0` to `-T5` → Timing

### Output
- `-v` → Verbose
- `-d` → Debug
- `-oA` → Save all formats

