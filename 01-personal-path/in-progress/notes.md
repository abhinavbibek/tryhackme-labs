# Steganography – `steghide`

Useful commands for identifying and extracting hidden data from files:

```bash
steghide info file
```
Displays information about a file and checks whether it contains embedded data.

```bash
steghide extract -sf file
```
Extracts embedded data from the specified file.

---

## Vulnerability & Exploit Research

### National Vulnerability Database (NVD)

NVD keeps track of CVEs (Common Vulnerabilities and Exposures) — whether or not there is an exploit publicly available — so it's a really good place to look if you're researching vulnerabilities in a specific piece of software.

**CVE format:**
```text
CVE-YEAR-IDNUMBER
```

### Exploit Database (ExploitDB)

ExploitDB tends to be very useful for hackers, as it often actually contains exploits that can be downloaded and used straight out of the box. It is often one of the first stops when encountering software in a CTF or pentest.

### searchsploit (Kali Linux)

If you're inclined towards the CLI on Linux, Kali comes pre-installed with a tool called `searchsploit` which allows you to search ExploitDB from your own machine.  
This works offline using a downloaded copy of the database.

```bash
searchsploit apache
```

---

## File & Text Processing Commands

```bash
find -name passwords.txt
find -name *.txt
wc -l access.log
grep "81.143.211.90" access.log
ls --help
man ls
```

---

## Linux Operators

```text
&   → Run command in background
&&  → Chain commands (run next only if previous succeeds)
>   → Redirect output (overwrite)
>>  → Redirect output (append)
```

---

## TryHackMe Connection

```bash
sudo openvpn ~/Desktop/veytrixbk.ovpn
ssh tryhackme@10.201.83.71
```

---

## Linux Flags & Switches

Flags and switches modify how commands behave.

| Flag | Meaning | Example |
|----|--------|--------|
| -h | Human-readable sizes | ls -lh, du -h |
| -l | Long listing | ls -l, ps aux |
| -a | Show hidden files | ls -a |
| -r | Reverse / recursive | ls -lr, rm -r |
| -R | Recursive | grep -R "foo" . |
| -v | Verbose | cp -v file dest |
| -c | Count / create | grep -c, tar -cvf |
| -n | Number lines | head -n 20, grep -n |
| -s | Silent / summary | curl -s, du -s |
| -p | Preserve / port | cp -p, ssh -p 2222 |
| -P | Numeric / absolute | ss -tulpn |
| -f | Force | rm -f |
| -t | Sort by time | ls -lt |
| -o | Output file | gcc -o app main.c |
| -z | Compress | tar -czf |
| -x | Extract / execute | tar -xvf, chmod +x |
| -y | Assume yes | apt -y install |
| --help | Help text | command --help |
| --version | Show version | openssl version |
| -i | Ignore case / in-place | grep -i, sed -i |
| -Pn | Skip host discovery | nmap -Pn |

---

## Switching Between Users

```bash
su user2
```
Switch user, keep current environment.

```bash
su -l user2
```
Switch user and load full environment (recommended).

---

## Root Directories

### /etc
System configuration files.

```bash
ls -l /etc
```

Important files:
- /etc/passwd — user accounts
- /etc/shadow — password hashes
- /etc/sudoers — sudo permissions

### /var
Variable service data (logs, caches, databases).

```bash
sudo ls -l /var
```

### /root
Home directory for root user.

```bash
sudo ls -l /root
```

### /tmp
Temporary files (cleared on reboot).

```bash
sudo ls -l /tmp
```

---

## File Transfer & Hosting

### wget
```bash
wget https://example.com/myfile.txt
```

### scp
```bash
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/
scp ubuntu@192.168.1.30:/home/ubuntu/file.txt local.txt
```

### Python HTTP Server
```bash
python3 -m http.server 8000
```

Download from another machine:
```bash
wget http://<IP>:8000/filename
curl -O http://<IP>:8000/filename
curl -o saved.txt http://<IP>:8000/filename
```

---

## Linux Processes

Processes are running programs managed by the kernel.

### Viewing Processes
```bash
ps
ps aux
top
```

### Managing Processes
```bash
kill <PID>
```

**Signals:**
- SIGTERM — graceful stop
- SIGKILL — force stop
- SIGSTOP — pause

---

## systemd Services

```bash
systemctl start apache2
systemctl stop apache2
systemctl enable apache2
systemctl disable apache2
```

---

## Foreground & Background Jobs

```bash
echo "Hello"
echo "Hello" &
```

Job control:
```text
Ctrl + Z  → suspend
jobs      → list jobs
fg        → resume foreground
```

---

## Cron & Crontabs

### What is Cron?
Cron is a time-based job scheduler used for automation.

### Crontab Commands

| Command | Description |
|------|------------|
| crontab -e | Edit crontab |
| crontab -l | List jobs |
| crontab -r | Remove crontab |
| crontab -u user | Manage another user |

### Cron Format
```text
* * * * * command
```

| Field | Meaning |
|----|--------|
| Minute | 0–59 |
| Hour | 0–23 |
| Day | 1–31 |
| Month | 1–12 |
| Weekday | 0–7 |

### Examples

| Schedule | Entry |
|-------|------|
| Every minute | * * * * * /script.sh |
| Midnight daily | 0 0 * * * /backup.sh |
| Sunday 5PM | 0 17 * * 0 /script.sh |
| Every 5 min | */5 * * * * /task.sh |
| On reboot | @reboot /script.sh |

---

## APT Packages & Repositories

### What Are APT Repositories?
APT repositories store software packages verified by GPG keys.

### Repository Locations
```text
/etc/apt/sources.list
/etc/apt/sources.list.d/
/etc/apt/trusted.gpg.d/
/etc/apt/preferences.d/
/etc/apt/apt.conf.d/
```

### Adding a Repository (Example: Sublime Text)

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo touch /etc/apt/sources.list.d/sublime-text.list
sudo apt update
sudo apt install sublime-text
```

### Removing Packages
```bash
apt remove <package-name>
```
