# Wireshark: The Basics  
## Tool Overview & Use Cases

Wireshark is one of the most powerful tools used for **analyzing network traffic**. It is mainly used to **capture packets** and **inspect them in detail**, making it extremely valuable for troubleshooting and security investigations.

### Common Use Cases
- Finding network issues such as congestion, packet loss, or bottlenecks  
- Detecting security problems like unusual traffic, unknown hosts, or abnormal port usage  
- Learning how protocols work by analyzing headers, response codes, and payloads  

> **Important:** Wireshark is **not** an IDS (Intrusion Detection System).  
> It does **not** block traffic or raise alerts automatically. It only captures and displays packets.  
> Detection depends entirely on the analyst’s knowledge and investigation skills.

---

## Wireshark GUI and Layout

When Wireshark starts, it opens a single main window divided into multiple sections:

### Toolbar
Contains shortcuts and menus for capturing traffic, filtering, exporting data, and merging capture files.

### Display Filter Bar
Used to apply **display filters** to show only specific packets.

### Recent Files
Shows previously opened capture files for quick access.

### Capture Filters and Interfaces
Displays available network interfaces (e.g., `eth0`, `lo`, `ens33`).  
These interfaces are the connection points between the system and the network.

### Status Bar
Shows capture status, profile name, packet count, and active interface details.

---

## Loading PCAP Files

Wireshark initially opens with no packets loaded. PCAP files can be opened using:

- **File → Open**
- Drag and drop the PCAP file
- Double-clicking the PCAP file

Once loaded, Wireshark displays packet statistics and detailed packet data.

---

## Packet Display Panes

Wireshark presents packet data using **three main panes**:

### Packet List Pane
- Shows a summary of packets  
- Includes source, destination, protocol, and info  
- Clicking a packet selects it for analysis  

### Packet Details Pane
- Displays a protocol-level breakdown of the selected packet  

### Packet Bytes Pane
- Shows raw packet data in hexadecimal and ASCII  
- Clicking fields in the details pane highlights corresponding bytes  

---

## Packet Colouring

Wireshark uses colors to quickly identify protocols and traffic patterns, helping analysts spot anomalies faster.

### Colouring Rule Types
- **Temporary rules** – Valid only for the current session  
- **Permanent rules** – Saved and reused  

Colouring can be toggled via:
```text
View → Colourise Packet List
```

Custom colouring rules can also be created using filters.

---

## Traffic Sniffing Controls

- **Blue shark button** → Start capture  
- **Red square button** → Stop capture  
- **Green circular arrow** → Restart capture  

The status bar shows the active interface and total packets captured.

---

## Merging PCAP Files

Wireshark allows multiple capture files to be merged:

```text
File → Merge
```

Steps:
1. Select another PCAP file  
2. Merge it with the current capture  
3. Save the merged file before analysis  

Useful when traffic is captured in multiple segments.

---

## Viewing Capture File Details

Capture file metadata is important when working with many PCAPs. Details include:
- File hash
- Capture time
- Interface used
- Statistics

Access via:
```text
Statistics → Capture File Properties
```
or by clicking the **PCAP icon** in the bottom-left corner.

---

## Packet Dissection (Protocol Dissection)

Packet dissection means breaking packets down into **protocol layers**.  
Wireshark automatically decodes many protocols.

Packets usually contain **5–7 OSI layers**.  
Clicking a layer highlights its raw bytes in the bytes pane.

---

## Packet Layers Explained

### Frame (Layer 1)
Physical-layer information about the packet.

### MAC Addresses (Layer 2)
Source and destination MAC addresses.

### IP Addresses (Layer 3)
Source and destination IPv4 or IPv6 addresses.

### Transport Layer (Layer 4)
TCP or UDP details, including ports.

### Protocol Errors
TCP retransmissions, reassembly issues, or malformed packets.

### Application Protocol (Layer 5–7)
Protocol details such as HTTP, FTP, SMB.

### Application Data
Actual application-level payload.

---

## Packet Navigation

### Packet Numbers
Each packet has a unique number, useful for referencing packets in large captures.

### Go to Packet
Allows jumping to:
- A specific packet number  
- The next related packet in a conversation  

Available via the **Go** menu.

---

## Finding Packets

Wireshark supports searching by:
- Display filter
- Hex value
- String
- Regular expression (regex)

Search scope:
- Packet list
- Packet details
- Packet bytes

> **Important:** Search location matters.  
> If data exists in one pane but the search is performed in another, it will not be found.

---

## Marking Packets

Packets can be marked to highlight important events:
- Right-click packet → Mark Packet  
- Or use the **Edit** menu  

Marked packets appear **black**.  
Marks are **not saved** after closing the capture file.

---

## Packet Comments

Comments can be added to packets for investigation notes.  
Unlike marks, **comments are saved** with the capture file.

---

## Exporting Packets

Instead of sharing entire PCAP files, analysts can export only relevant packets:

```text
File → Export Packets
```

This reduces noise and focuses analysis on suspicious traffic.

---

## Exporting Objects (Files)

Wireshark can extract files transferred over the network.

Supported protocols include:
- HTTP
- SMB
- TFTP
- IMF
- DICOM

This feature is especially useful in **malware analysis**.

---

## Time Display Format

Default time format:
- Seconds since capture start  

Can be changed to:
- UTC time
- Other timestamp formats  

Via:
```text
View → Time Display Format
```

---

## Expert Information

Wireshark provides expert hints about potential issues.

### Severity Levels
- **Chat (Blue)** – Normal activity  
- **Note (Cyan)** – Interesting behavior  
- **Warn (Yellow)** – Suspicious behavior  
- **Error (Red)** – Malformed or broken packets  

Access via:
```text
Analyse → Expert Information
```
or the status bar shortcut.

---

## Packet Filtering Basics

Wireshark supports two types of filters:

- **Capture filters** – Applied during packet capture  
- **Display filters** – Applied after capture  

Display filters are used more frequently during analysis.

> **Golden Rule:**  
> **“If you can click it, you can filter it.”**

---

## Apply as Filter

Right-click a field → **Apply as Filter**  
Wireshark automatically generates the filter and hides unrelated packets.

---

## Conversation Filter

Shows only packets related to a specific conversation (based on IPs and ports).  
Very useful for tracking sessions.

---

## Colourise Conversation

Highlights packets belonging to a conversation without hiding others.  
Uses colouring rules instead of filters.

---

## Prepare as Filter

Similar to Apply as Filter, but does **not apply immediately**.  
Allows combining multiple filter conditions first.

---

## Apply as Column

Adds a selected field as a new column in the packet list pane.  
Helpful for comparing values across packets.

---

## Follow Stream

Reconstructs application-level traffic from packets.

Supported streams:
- TCP
- UDP
- HTTP  

Client traffic appears **red**, server traffic **blue**.

When a stream is followed:
- Wireshark automatically applies a filter  
- To return to full packet view, the filter must be cleared manually
