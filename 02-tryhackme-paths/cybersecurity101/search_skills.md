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
apache 2.4.1

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

---
## CVE (Common Vulnerabilities and Exposures)

CVE can be thought of as a **dictionary of known vulnerabilities**.  
It gives every publicly known vulnerability a **unique ID**, so everyone refers to the same issue.

### Example CVE ID format
```text
CVE-2024-29988
```

### Why CVE is Important
- Standard naming for vulnerabilities
- Used by researchers, vendors, and security teams
- Avoids confusion when discussing security issues

---

## CVE Key Points

| Item | Description |
|----|------------|
| CVE ID | Unique vulnerability identifier |
| Format | CVE-YEAR-NUMBER |
| Maintained by | MITRE Corporation |

### Example
- **CVE-2014-0160** → Heartbleed vulnerability

### Notes
- CVE itself does **not** provide exploits
- It only **describes and identifies** vulnerabilities

---

## CVE & NVD

To search and read detailed CVE information, we use:
- CVE Program website
- National Vulnerability Database (NVD)

### What NVD Adds

| Feature | Purpose |
|------|---------|
| CVSS score | Severity rating |
| Impact | Affected systems |
| References | Vendor and research links |

### Notes
- NVD provides more technical details than CVE alone
- Very useful for **risk assessment**

---

## Exploit Database

The Exploit Database is a collection of **public exploit codes**.  
It is commonly used **after identifying a vulnerability**, usually via a CVE.

### Important Rule
- Exploitation should **only be done with permission**
- Typically part of a **legal penetration test or red team engagement**

---

## Why Exploit Database Is Used

| Reason | Explanation |
|------|-------------|
| Find exploit code | Saves time |
| Verified exploits | Some exploits are tested |
| Learning | Helps understand how vulnerabilities work |

### Notes
- Not all exploits work as-is
- Some exploits require modification
- Always check **exploit date** and **target version**

---

## Typical Workflow (Simplified)

```text
Find service/version → Search CVE → Check NVD → Look for exploit → Test (with permission)
```

