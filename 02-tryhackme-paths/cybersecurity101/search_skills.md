## Shodan

Shodan is a search engine for **devices connected to the internet**.  
Instead of searching websites like Google, it searches servers, routers, webcams, IoT devices, and industrial systems.

It is useful when you want to know:
- What services are exposed to the internet
- Which versions of software are running
- How devices are distributed across countries

**Example use case:**  
Finding how many servers are still running **Apache 2.4.1**.

**Search query:**
```text
apache 2.4.1
```

This returns servers that have `"apache 2.4.1"` in their HTTP headers.

**Common information you can see:**
- IP address
- Open ports
- Service banners
- Country and organization

**Notes:**
- Very useful for reconnaissance and OSINT
- Historical trends are available with a paid account

---

## Censys

Censys is similar to Shodan but focuses more on:
- Hosts and websites
- SSL/TLS certificates
- Domains and internet assets

While Shodan focuses on devices, **Censys focuses more on internet infrastructure**.

**Typical use cases:**
- Finding domains related to an organization
- Auditing open ports and services
- Discovering unknown or rogue assets

**Example search:**
```text
apache 2.4.1
```

**What Censys is especially good at:**
- Certificate-based searches
- Domain and subdomain enumeration
- Asset discovery for organizations

**Notes:**
- Very useful during the reconnaissance phase
- Cleaner certificate-related data compared to Shodan

---

## VirusTotal

VirusTotal is an online service used to scan files, URLs, and hashes using multiple antivirus engines.

You can:
- Upload a file
- Scan a URL
- Search using a file hash (MD5, SHA1, SHA256)

**Example:**
```text
Upload file or paste SHA256 hash
```

**What you get:**
- Detection results from multiple antivirus engines
- Community comments
- File behavior information (in some cases)

**Important note:**
- A file flagged as malicious is not always truly malicious
- Community comments often explain false positives

**Use cases:**
- Checking suspicious downloads
- Analyzing malware samples during labs

---

## Have I Been Pwned (HIBP)

Have I Been Pwned checks whether an email address has appeared in known data breaches.

It helps answer:
- Has this email been leaked?
- Which breaches exposed the data?

**Example:**
```text
test@example.com
```

**Why this matters:**
- Leaked emails often imply leaked passwords
- Many users reuse passwords across multiple sites
- Even hashed passwords can sometimes be cracked

**Important points:**
- Used for awareness and OSINT
- Not for hacking, but for understanding breach impact
