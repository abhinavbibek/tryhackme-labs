# Networking Concepts – Learner Notes (TryHackMe)

These are **simple reference notes** written in a student/learner style for revision.  
Not deep theory—just enough to understand concepts during labs and rooms.

---

## OSI Model (ISO)

The OSI (Open Systems Interconnection) model is a **conceptual framework** that explains how data moves across a network.  
It is theoretical but very important for understanding networking terms.

Total layers: **7**

Mnemonic (bottom → top):  
**Please Do Not Throw Spinach Pizza Away**

| Layer No | Layer Name |
|--------|-----------|
| 1 | Physical |
| 2 | Data Link |
| 3 | Network |
| 4 | Transport |
| 5 | Session |
| 6 | Presentation |
| 7 | Application |

Remembering layer numbers is useful for terms like **Layer 3 switch** or **Layer 7 firewall**.

---

## Layer 1 – Physical Layer

- Deals with **physical transmission**
- Defines how bits (0s and 1s) are sent
- Uses electrical, optical, or wireless signals

Examples:
- Ethernet cable
- Optical fiber
- WiFi radio bands (2.4 GHz, 5 GHz, 6 GHz)

---

## Layer 2 – Data Link Layer

- Handles communication **within the same network segment**
- Defines how devices communicate locally
- Uses **MAC addresses**

Key points:
- MAC address = 6 bytes (hexadecimal)
- Example format: `cc:5e:f8:02:21:a7`
- First 3 bytes identify the vendor (OUI)

Examples:
- Ethernet (802.3)
- WiFi (802.11)

Each frame contains:
- Source MAC address
- Destination MAC address
- Data

---

## Layer 3 – Network Layer

- Handles communication **between different networks**
- Responsible for routing packets
- Determines the best path to the destination

Examples:
- IP (Internet Protocol)
- ICMP
- VPN protocols (IPSec, SSL/TLS)

Routers operate at **Layer 3**.

---

## Layer 4 – Transport Layer

- Enables **end-to-end communication**
- Connects applications on different hosts
- Handles segmentation and reliability

Protocols:
- TCP
- UDP

---

## Layer 5 – Session Layer

- Establishes and manages sessions
- Synchronizes communication
- Helps recover from interruptions

Examples:
- RPC
- NFS

---

## Layer 6 – Presentation Layer

- Ensures data is in a readable format
- Handles:
  - Encoding
  - Compression
  - Encryption

Examples:
- ASCII
- Unicode
- MIME
- JPEG, PNG

---

## Layer 7 – Application Layer

- Closest to the user
- Provides network services to applications

Examples:
- HTTP
- FTP
- DNS
- SMTP
- POP3
- IMAP

---

## OSI Model Summary Table

| Layer | Name | Function | Examples |
|-----|------|---------|----------|
| 7 | Application | User-facing services | HTTP, FTP, DNS |
| 6 | Presentation | Encoding & encryption | Unicode, MIME |
| 5 | Session | Session management | RPC, NFS |
| 4 | Transport | End-to-end delivery | TCP, UDP |
| 3 | Network | Routing & addressing | IP, ICMP |
| 2 | Data Link | Local delivery | Ethernet, WiFi |
| 1 | Physical | Signal transmission | Cables, radio |

---

## TCP/IP Model

Unlike the OSI model (theoretical), **TCP/IP is practical and implemented**.

### TCP/IP Layers (Top → Bottom)

| TCP/IP Layer | OSI Equivalent |
|-------------|----------------|
| Application | OSI 7, 6, 5 |
| Transport | OSI 4 |
| Internet | OSI 3 |
| Link | OSI 2 |
| Physical | OSI 1 (sometimes included) |

Common protocols:
- Application: HTTP, HTTPS, FTP, SSH
- Transport: TCP, UDP
- Internet: IP, ICMP
- Link: Ethernet, WiFi

---

## IP Addresses (IPv4)

Example IPv4 address:
```text
192.168.66.89
```

Key points:
- 32 bits total
- 4 octets
- Each octet ranges from 0–255

### Special Addresses
- Network address → `.0`
- Broadcast address → `.255`

Example:
- Network: `192.168.66.0`
- Broadcast: `192.168.66.255`
- Valid hosts: `192.168.66.1 – 192.168.66.254`

---

## Checking IP Address (Linux)

```bash
ifconfig
```
or
```bash
ip a s
```

Example meaning:
- IP address: `192.168.66.89`
- Subnet mask: `255.255.255.0`
- `/24` → first 24 bits are network bits

---

## Private IP Address Ranges (RFC 1918)

Must be memorized.

| Range | CIDR |
|------|------|
| 10.0.0.0 – 10.255.255.255 | /8 |
| 172.16.0.0 – 172.31.255.255 | /12 |
| 192.168.0.0 – 192.168.255.255 | /16 |

Private IPs:
- Cannot be accessed directly from the Internet
- Use **NAT** to access public networks

---

## Routing

- Routers forward packets between networks
- Operate at **Layer 3**
- Inspect destination IP address
- Choose the best path

Packets usually pass through **multiple routers** before reaching the destination.

---

## UDP (User Datagram Protocol)

- Transport layer protocol
- Connectionless
- Fast but unreliable
- No delivery confirmation

Key points:
- Uses port numbers
- Port range: `1–65535`
- Port `0` is reserved

Real-life analogy:
- Sending normal mail without delivery confirmation

Used when **speed matters more than reliability**.

---

## TCP (Transmission Control Protocol)

- Transport layer protocol
- Connection-oriented
- Reliable delivery
- Uses sequence and acknowledgment numbers

### TCP Three-Way Handshake
1. **SYN** – Client requests connection
2. **SYN-ACK** – Server responds
3. **ACK** – Client confirms

Used when reliability is critical (web browsing, login, file transfer).

---

## Encapsulation

Encapsulation means **each OSI layer adds its own header**.

Process:
1. Application data
2. Transport layer adds TCP/UDP header
3. Network layer adds IP header
4. Data link layer adds frame header and trailer

On the receiving side, headers are removed in reverse order.

---

## Life of a Packet (Simplified)

1. User enters data in browser
2. HTTP request is created
3. TCP connection is established
4. IP packet is formed
5. Frame is sent to router
6. Routers forward the packet
7. Destination receives and decapsulates the data

---

## Telnet

TELNET is a **remote terminal protocol**.

Key points:
- Works over TCP
- Sends data in plaintext
- Mainly used for testing services

### Telnet Syntax
```bash
telnet <IP> <PORT>
```

---

### Echo Server (Port 7)
```bash
telnet 10.66.169.9 7
```
The server echoes everything back.

Exit telnet:
```text
CTRL + ]
quit
```

---

### Daytime Server (Port 13)
```bash
telnet 10.66.169.9 13
```
Returns current date and time, then closes the connection.

---

### HTTP via Telnet (Port 80)
```bash
telnet 10.66.169.9 80
```

Then type:
```text
GET / HTTP/1.1
Host: telnet.thm
```

Press **Enter twice** to send the request.
