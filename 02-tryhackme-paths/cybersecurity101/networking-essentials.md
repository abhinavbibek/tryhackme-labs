# Networking Essentials

---

## DHCP – Give Me My Network Settings

When you go to a coffee shop and connect your laptop to WiFi, you usually do **not** manually configure any network settings. Still, the connection works instantly. This happens because of **DHCP**.

To connect to a network, a device needs at least:
- IP address with subnet mask
- Default gateway (router)
- DNS server

Manually configuring these settings works for **servers**, because servers usually remain on the same network and are expected to have fixed IP addresses. However, for **laptops, phones, and tablets** that constantly move between networks, manual configuration is impractical.

**DHCP (Dynamic Host Configuration Protocol)** solves this problem by automatically assigning network settings to devices. It also prevents **IP address conflicts**, where two devices accidentally use the same IP address.

### DHCP Technical Details
- Operates at the **Application Layer**
- Uses **UDP**
- DHCP server listens on **UDP port 67**
- DHCP client sends from **UDP port 68**
- Enabled by default on most devices

---

## DHCP Process – DORA

DHCP follows a four-step process known as **DORA**:

1. **Discover**  
   The client broadcasts a message asking if any DHCP server is available.

2. **Offer**  
   The DHCP server replies with an offer, proposing an IP address and other settings.

3. **Request**  
   The client responds, indicating it wants to accept the offered IP address.

4. **Acknowledge**  
   The server confirms and officially assigns the IP address.

### Important Detail
At the start:
- Client IP: `0.0.0.0`
- Destination IP: `255.255.255.255`
- Destination MAC: `ff:ff:ff:ff:ff:ff`

Once complete, the client receives:
- IP address
- Gateway IP
- DNS server IP

After this, the device can communicate on the local network and access the Internet.

---

## ARP – Mapping IP to MAC

On local Ethernet or WiFi networks, devices communicate using **MAC addresses**, but network configuration is done using **IP addresses**.  
ARP bridges this gap.

**ARP (Address Resolution Protocol)** maps an IP address to a MAC address.

### Example
If a laptop with IP `192.168.66.89` wants to communicate with `192.168.66.1`:
- It sends an **ARP Request**:  
  “Who has 192.168.66.1?”
- The request is sent to the broadcast MAC address:  
  `ff:ff:ff:ff:ff:ff`
- The device with IP `192.168.66.1` responds with an **ARP Reply** containing its MAC address

After this exchange, communication happens directly using Ethernet frames.

### Key Points
- ARP packets are **not encapsulated inside IP or UDP**
- They are sent directly in **Ethernet frames**
- ARP is often considered **Layer 2** or **Layer 3**
- Main purpose: **Translate IP → MAC**

---

## ICMP – Network Troubleshooting

**ICMP (Internet Control Message Protocol)** is used for diagnostics and error reporting.

Common tools using ICMP:
- `ping`
- `traceroute`

---

## Ping

Ping sends an **ICMP Echo Request (Type 8)** to a target.  
If reachable, the target replies with an **ICMP Echo Reply (Type 0)**.

### Ping Is Used To:
- Check if a host is reachable
- Measure round-trip time (RTT)

### Ping May Fail Because:
- The target system is offline
- A firewall blocks ICMP traffic

Ping output typically shows:
- Packet loss
- Minimum, average, and maximum RTT

---

## Traceroute

Traceroute displays the path packets take to reach a destination.

### How It Works
- Uses the **TTL (Time To Live)** field in IP packets
- Each router reduces TTL by 1
- When TTL reaches 0, the router drops the packet
- The router sends an **ICMP Time Exceeded (Type 11)** message

By sending packets with increasing TTL values, traceroute discovers each hop.

### Notes
- Some routers do not reply → shown as `* * *`
- Results can change because routing paths are dynamic

---

## Routing

Routing is the process of forwarding packets between different networks.

Routers:
- Operate at **Layer 3**
- Inspect destination IP addresses
- Choose the best available path

The Internet contains **millions of routers**, often with multiple possible paths between networks.

### Common Routing Protocols
- **OSPF** – Link-state protocol, calculates shortest paths
- **EIGRP** – Cisco protocol using bandwidth and delay metrics
- **BGP** – Core routing protocol of the Internet (used between ISPs)
- **RIP** – Simple protocol based on hop count (small networks)

---

## NAT – Network Address Translation

IPv4 address space is limited. With billions of devices, public IP addresses would quickly run out without NAT.

**NAT (Network Address Translation)** allows multiple private IP addresses to share one or a few public IP addresses.

### Example
A company may have:
- 20 internal systems using `192.168.x.x`
- Only one public IP address

The NAT router:
- Translates internal IP + port to external IP + port
- Maintains a translation table

From the device’s perspective:
- It communicates directly with the web server

From the server’s perspective:
- Requests appear to come from the router’s public IP

### Key Points
- NAT works transparently
- Widely used in home and enterprise networks
- Conserves public IPv4 addresses
