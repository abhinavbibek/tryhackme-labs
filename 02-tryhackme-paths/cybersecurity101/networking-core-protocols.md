# Networking Core Protocols

These notes explain **core networking protocols** and show how they can be interacted with using **telnet**, exactly as demonstrated in **TryHackMe labs**.  
All examples are for **learning and troubleshooting only**.

---

## DNS – Domain Name System (Remembering Addresses)

DNS maps **domain names to IP addresses** so users do not need to memorize IPs.

- **OSI Layer:** Application (Layer 7)
- **Default Ports:**
  - UDP 53
  - TCP 53 (fallback)

### Common DNS Record Types

**A Record**  
Maps a hostname to an IPv4 address  
Example:
```text
example.com → 172.17.2.172
```

**AAAA Record**  
Maps a hostname to an IPv6 address

**CNAME Record**  
Maps one domain name to another  
Example:
```text
www.example.com → example.com
```

**MX Record**  
Specifies the mail server responsible for receiving email for a domain

Examples:
- Visiting `example.com` → browser queries the **A record**
- Sending email to `test@example.com` → mail server queries the **MX record**

### DNS Lookup Example
```bash
nslookup www.example.com
```

DNS queries are **binary**, so telnet is not practical for queries, but a TCP connection can still be tested:
```bash
telnet <DNS_SERVER_IP> 53
```

---

## WHOIS

WHOIS provides information about **who registered a domain**.

WHOIS records may include:
- Domain creation date
- Last updated date
- Registrar details
- Registrant contact information (unless privacy protected)

### WHOIS Lookup Example
```bash
whois example.com
```

Privacy protection may hide registrant details while still showing registrar information.

---

## HTTP and HTTPS – Accessing the Web

HTTP and HTTPS are used by browsers to communicate with web servers.

### Common HTTP Methods
- **GET** – Retrieve data
- **POST** – Submit data
- **PUT** – Create or overwrite data
- **DELETE** – Remove data

### Default Ports
- HTTP → TCP 80
- HTTPS → TCP 443  
Common alternatives: **8080**, **8443**

### HTTP via Telnet
```bash
telnet <WEB_SERVER_IP> 80
```

Then type:
```text
GET / HTTP/1.1
Host: example.com
```
(Press Enter twice)

Request a specific file:
```text
GET /file.html HTTP/1.1
Host: example.com
```

**HTTPS cannot be used with telnet** because encryption (TLS) is required.

---

## FTP – File Transfer Protocol

FTP is designed for **efficient file transfer**.

- **Control connection:** TCP 21
- **Data transfer:** Uses a separate connection

### Common FTP Commands
- USER – Username
- PASS – Password
- LIST – List files
- RETR – Download file
- STOR – Upload file

### FTP Using Telnet
```bash
telnet <FTP_SERVER_IP> 21
```

Example session:
```text
USER anonymous
PASS anonymous
LIST
RETR file.txt
QUIT
```

---

## SMTP – Sending Email

SMTP is used to **send emails** between clients and servers.

- **Default Port:** TCP 25

### Common SMTP Commands
- HELO / EHLO – Start session
- MAIL FROM – Sender
- RCPT TO – Recipient
- DATA – Message body
- . – End message

### SMTP Using Telnet
```bash
telnet <SMTP_SERVER_IP> 25
```

Example session:
```text
HELO client.thm
MAIL FROM:user@client.thm
RCPT TO:victim@server.thm
DATA
From: user@client.thm
To: victim@server.thm
Subject: Test Email

Hello, this email was sent using telnet.
.
QUIT
```

---

## POP3 – Receiving Email

POP3 retrieves emails from a mail server and usually **deletes them afterward**.

- **Default Port:** TCP 110

### Common POP3 Commands
- USER – Username
- PASS – Password
- STAT – Message count and size
- LIST – List messages
- RETR – Retrieve message
- DELE – Mark message for deletion
- QUIT – End session

### POP3 Using Telnet
```bash
telnet <POP3_SERVER_IP> 110
```

Example session:
```text
USER username
PASS password
STAT
LIST
RETR 1
QUIT
```

---

## IMAP – Synchronizing Email

IMAP allows email **synchronization across multiple devices**.

Unlike POP3:
- Emails remain on the server
- Changes sync across all clients

- **Default Port:** TCP 143

### Common IMAP Commands
- LOGIN – Authenticate user
- SELECT – Choose mailbox
- FETCH – Retrieve message
- MOVE – Move message
- COPY – Copy message
- LOGOUT – End session

### IMAP Using Telnet
```bash
telnet <IMAP_SERVER_IP> 143
```

Example session:
```text
A LOGIN username password
B SELECT inbox
C FETCH 1 body[]
D LOGOUT
```

> Command tags (A, B, C…) are **mandatory** in IMAP.

---

## TELNET

Telnet itself allows **remote login**.

- **Default Port:** TCP 23
- Sends credentials in **plaintext**

### Telnet Login
```bash
telnet <TARGET_IP> 23
```

Then enter:
```text
username
password
```

---

## Default Port Summary

| Protocol | Port |
|--------|------|
| TELNET | TCP 23 |
| DNS | UDP/TCP 53 |
| HTTP | TCP 80 |
| HTTPS | TCP 443 |
| FTP | TCP 21 |
| SMTP | TCP 25 |
| POP3 | TCP 110 |
| IMAP | TCP 143 |

---

## Security Note

All **telnet-based communication is unencrypted**.  
Anyone capturing network traffic can read:
- Usernames
- Passwords
- Data

### Secure Alternatives
- HTTPS
- IMAPS
- POP3S
- SMTPS
- SFTP
