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


