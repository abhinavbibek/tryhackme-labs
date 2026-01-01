# Introduction to Tcpdump

Tcpdump is a **command-line packet capture tool** used to monitor and analyze network traffic.  
It is written in **C/C++** and uses the **libpcap** library, which is also used by tools like Wireshark.

### Key Points
- Very fast and stable (developed in the late 1980s / early 1990s)
- Mainly used on **Unix/Linux** systems
- On Windows, similar functionality exists via **WinPcap / Npcap**
- Best suited for **terminal-based traffic analysis and scripting**

---

## Basic Packet Capture

Running `tcpdump` without arguments only confirms that the tool is installed.  
In real scenarios, you must specify:
- Which interface to listen on
- What traffic to capture
- Whether to save or display packets

---

## Selecting a Network Interface

To capture traffic, a network interface must be selected.

### List Available Interfaces
```bash
ip a s
```

### Common Interfaces
- `lo` → Loopback
- `eth0`, `ens5` → Wired Ethernet
- `wlo1` → WiFi
- `any` → All interfaces

### Example
```bash
sudo tcpdump -i ens5
```

---

## Saving Captured Packets

Captured traffic can be saved using the `-w` option.

### Example
```bash
sudo tcpdump -i ens5 -w capture.pcap
```

### Notes
- File extension is usually `.pcap`
- When `-w` is used, packets are **not displayed on screen**
- Files can later be analyzed using `tcpdump` or **Wireshark**

---

## Reading Packets from a File

Previously captured packets can be read using `-r`.

### Example
```bash
tcpdump -r capture.pcap
```

### Use Cases
- Studying protocol behavior
- Analyzing suspicious traffic
- Investigating attacks from packet captures

> Root privileges are **not required** when reading capture files.

---

## Limiting Packet Count

To automatically stop capture after a fixed number of packets, use `-c`.

### Example
```bash
sudo tcpdump -i ens5 -c 10
```

Without `-c`, capture runs until manually stopped using **CTRL + C**.

---

## Disabling Name Resolution

By default, tcpdump:
- Resolves IP addresses to domain names
- Resolves port numbers to service names

### Options
- `-n`  → Disable DNS resolution
- `-nn` → Disable DNS **and** port resolution

### Example
```bash
sudo tcpdump -i ens5 -c 5 -n
```

This improves performance and avoids unnecessary lookups.

---

## Increasing Verbosity

Verbose output provides more packet-level details.

### Verbosity Levels
- `-v`   → Basic verbosity
- `-vv`  → More details
- `-vvv` → Maximum verbosity

### Example
```bash
tcpdump -i eth0 -v
```

---

## Summary of Basic Options

| Option | Description |
|-----|-------------|
| `-i INTERFACE` | Capture on specific interface |
| `-w FILE` | Write packets to file |
| `-r FILE` | Read packets from file |
| `-c COUNT` | Capture limited number of packets |
| `-n` | Disable DNS resolution |
| `-nn` | Disable DNS and port resolution |
| `-v / -vv / -vvv` | Increase verbosity |

---

## Filtering Concepts (Why Filtering Matters)

Capturing all traffic is noisy and impractical.  
Filtering helps focus on **relevant traffic**, similar to listening to one conversation in a crowd.

---

## Filtering by Host

Capture traffic to or from a specific host.

### Examples
```bash
tcpdump host example.com
tcpdump src host 192.168.1.10
tcpdump dst host 8.8.8.8
```

### Saving Filtered Traffic
```bash
sudo tcpdump host example.com -w http.pcap
```

---

## Filtering by Port

Capture traffic on specific ports.

### Examples
```bash
tcpdump port 53        # DNS traffic
tcpdump src port 80
tcpdump dst port 443
```

### DNS Example
```bash
sudo tcpdump -i ens5 port 53 -n
```

---

## Filtering by Protocol

Limit capture to specific protocols.

### Common Protocols
- `tcp`
- `udp`
- `icmp`
- `ip`, `ip6`

### Example
```bash
sudo tcpdump -i ens5 icmp -n
```

Useful for detecting:
- Ping (ICMP echo)
- Traceroute behavior
- Network errors

---

## Logical Operators

Operators used to combine filters:

- `and` → Both conditions must be true
- `or`  → Either condition is true
- `not` → Exclude traffic

### Examples
```bash
tcpdump host 1.1.1.1 and tcp
tcpdump udp or icmp
tcpdump not tcp
```

---

## Reading PCAP Files with Filters

Filters can also be applied while reading capture files.

### Example
```bash
tcpdump -r traffic.pcap -c 5 -n
```

### Counting Matching Packets
```bash
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc
```

---

## Advanced Filtering Concepts

Tcpdump supports filtering based on:
- Packet length
- Header bytes
- TCP flags

### Packet Length Filters
```text
greater 100
less 50
```

---

## Binary Operations (TCP Flags)

Binary operators used:
- `&` → AND
- `|` → OR
- `!` → NOT

Used for inspecting header fields at the **bit level**.

---

## TCP Flag Filtering

### Common TCP Flags
- `tcp-syn`
- `tcp-ack`
- `tcp-fin`
- `tcp-rst`
- `tcp-push`

### Examples
```bash
tcpdump "tcp[tcpflags] == tcp-syn"
tcpdump "tcp[tcpflags] & tcp-syn != 0"
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"
```

Used to analyze:
- Connection attempts
- Port scans
- TCP handshakes

---

## Displaying Packet Data

Tcpdump output format can be customized.

### Display Options
- `-q`  → Quick summary
- `-e`  → Show MAC addresses
- `-A`  → ASCII output
- `-xx` → Hexadecimal output
- `-X`  → Hex + ASCII output

---

## Output Display Examples

### Quick View
```bash
tcpdump -r file.pcap -q
```

### Include MAC Addresses
```bash
tcpdump -r file.pcap -e
```

### ASCII Output
```bash
tcpdump -r file.pcap -A
```

### Hex Output
```bash
tcpdump -r file.pcap -xx
```

### Hex + ASCII
```bash
tcpdump -r file.pcap -X
```

---

## Final Summary – Display Options

| Option | Meaning |
|-----|--------|
| `-q` | Brief output |
| `-e` | Include MAC addresses |
| `-A` | ASCII output |
| `-xx` | Hexadecimal output |
| `-X` | Hex + ASCII output |

