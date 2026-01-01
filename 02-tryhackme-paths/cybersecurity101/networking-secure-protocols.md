# Networking Secure Protocols

Earlier, it was very easy for attackers to read network traffic. Anyone on the same network could use a packet-capturing tool, put their network card into **promiscuous mode**, and capture all packets. This allowed attackers to read chats, emails, usernames, and passwords because everything was sent in **cleartext**.

Users had no control over this exposure. Credentials and sensitive data were visible by default.

To solve this problem, **secure communication protocols** were developed.

---

## TLS (Transport Layer Security)

In the early 1990s, Netscape realized that the web needed security and created **SSL (Secure Sockets Layer)**.  
- SSL 2.0 was released in 1995  
- In 1999, the IETF introduced **TLS**, an improved version of SSL  
- **TLS 1.0** was based on SSL 3.0 with stronger security  
- In **2018**, **TLS 1.3** was released after a major redesign  

The key takeaway is not the dates, but the fact that **TLS has evolved for over 20 years** and is now considered very secure.

### What TLS Provides
TLS is a **cryptographic protocol** that operates at the **transport layer** and provides:

- **Confidentiality** – Data cannot be read by attackers
- **Integrity** – Data cannot be modified in transit

Without TLS, activities such as **online shopping, banking, email, and messaging** would be unsafe.

---

## Protocols Secured Using TLS

Many existing protocols became secure simply by adding TLS:

| Insecure | Secure |
|-------|--------|
| HTTP | HTTPS |
| DNS | DoT (DNS over TLS) |
| MQTT | MQTTS |
| SIP | SIPS |

The **“S”** stands for **Secure**, indicating TLS is used.

---

## TLS Certificates and Certificate Authorities (CA)

Before a server can use TLS, it must have a **TLS certificate**.

### Certificate Process
1. Server administrator creates a **Certificate Signing Request (CSR)**
2. CSR is sent to a **Certificate Authority (CA)**
3. CA verifies the request
4. CA signs and issues the certificate

Clients trust certificates because:
- Trusted CA certificates are pre-installed in operating systems and browsers

This is similar to trusting **official stamps or seals** in real life.

### Certificate Types
- **Paid certificates** – Issued by commercial CAs
- **Free certificates** – Provided by **Let’s Encrypt**
- **Self-signed certificates** – Encrypt traffic but do **not** verify identity

Self-signed certificates provide encryption but **no trust guarantee**.

---

## HTTP vs HTTPS

### HTTP (Insecure)
- Uses **TCP**
- Default port: **80**
- All data is sent in **cleartext**

Steps when accessing a site using HTTP:
1. Resolve domain name to IP
2. Establish TCP three-way handshake
3. Send HTTP request (e.g., `GET / HTTP/1.1`)

In packet captures, HTTP traffic is **fully readable**.

---

## HTTPS (HTTP over TLS)

HTTPS is **HTTP running inside a TLS tunnel**.

Steps when accessing a site using HTTPS:
1. Resolve domain name
2. Establish TCP three-way handshake
3. Establish TLS session
4. Exchange HTTP data securely

### In Wireshark
- TCP handshake packets appear first
- TLS negotiation packets follow
- Application data is shown as **encrypted**

Wireshark displays this as **“Application Data”** because it cannot see HTTP inside encrypted TLS traffic.

Without keys, packet contents appear as **unreadable gibberish**.

---

## Decrypting HTTPS Traffic

HTTPS traffic can only be decrypted if the **encryption key** is available.

If Wireshark has the correct key:
- TCP and TLS handshakes remain unchanged
- HTTP requests (e.g., `GET`) become visible again

This proves:
- HTTPS still uses HTTP
- TLS hides HTTP from attackers
- TCP and IP remain unchanged

---

## Secure Email Protocols

Email protocols also became secure using TLS.

### Insecure Default Ports
| Protocol | Port |
|------|------|
| HTTP | 80 |
| SMTP | 25 |
| POP3 | 110 |
| IMAP | 143 |

### Secure Ports Using TLS
| Protocol | Port |
|------|------|
| HTTPS | 443 |
| SMTPS | 465, 587 |
| POP3S | 995 |
| IMAPS | 993 |

Behavior is identical to HTTPS—only encrypted.

---

## SSH (Secure Shell)

TELNET sends usernames and passwords in **cleartext**, making it insecure.

To fix this, **SSH** was created in 1995 by **Tatu Ylönen**.  
- SSH-2 improved security
- **OpenSSH** became the most widely used implementation

### Benefits of SSH
- Secure authentication (passwords, public keys, 2FA)
- Encrypted communication
- Data integrity protection
- Protection against man-in-the-middle attacks
- Port tunneling (VPN-like behavior)
- X11 forwarding for GUI apps

### SSH Command
```bash
ssh username@hostname
```

### Ports
- SSH → **TCP 22**
- TELNET → **TCP 23**

---

## SFTP and FTPS

### SFTP (SSH File Transfer Protocol)
- Uses SSH
- Port **22**
- Secure by default
- Commands: `get`, `put`
- Easy to configure with OpenSSH

### FTPS (FTP Secure)
- FTP secured using TLS
- Usually uses port **990**
- Requires TLS certificates
- More complex (separate control and data channels)

**SFTP is simpler and more firewall-friendly** than FTPS.

---

## VPN (Virtual Private Network)

VPNs allow **private communication over the public Internet**.

Used by companies with multiple branches so all locations can securely access shared resources.

### VPN Focus Areas
- **Virtual** – Uses Internet infrastructure
- **Private** – Encrypts traffic

VPN clients encrypt traffic and send it through a **secure tunnel** to a VPN server, where it is decrypted inside the private network.

### VPN Connection Types
- Site-to-site (branch networks)
- Remote access (individual users)

---

## VPN Traffic Behavior

Once connected to a VPN:
- All traffic is usually routed through the VPN
- Websites see the **VPN server’s IP**
- ISPs see **encrypted traffic only**

This allows:
- Bypassing geographic restrictions
- Preventing ISP censorship
- Appearing to be in another country

### Important Notes
- Some VPNs only route internal traffic
- Some VPNs leak IP or DNS information
- **DNS leak tests** are important for verification
