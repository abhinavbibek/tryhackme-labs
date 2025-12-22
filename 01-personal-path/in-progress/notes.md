# Steganography – `steghide`

The useful commands to identify and extract hidden data in files are:

```bash
steghide info file
```
Displays the data in a file and verifies whether there is embedded data in the file.

```bash
steghide extract -sf file
```
Reads data that is contained in the given file.

---

## Exploit Research & Vulnerability Research

### National Vulnerability Database (NVD)

NVD maintains a collection of CVEs (Common Vulnerabilities and Exposures) which may or may not have an exploit publicly available.  
It is a good location to search when researching vulnerabilities in a particular piece of software.

**CVE format:**
```text
CVE-YEAR-IDNUMBER
```

---

### Exploit Database (ExploitDB)

ExploitDB can be very useful to hackers, as it often contains exploits that can be downloaded and executed directly without modification.  
When encountering software during a pentest or CTF, it is commonly one of the first resources visited.

---

### searchsploit (Kali Linux)

If you prefer working with the CLI on Linux, Kali Linux is preconfigured with a tool called `searchsploit`, which allows searching ExploitDB locally.

This works offline using a downloaded copy of the database.

```bash
searchsploit apache
```

---

## File and Text Processing Commands

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
&    → Run command in the background
&&   → Run next command only if previous succeeds
>    → Redirect output (overwrite)
>>   → Redirect output (append)
```

---

## TryHackMe Connection

```bash
sudo openvpn ~/Desktop/veytrixbk.ovpn
ssh tryhackme@10.201.83.71
```

---

## Linux Flags & Switches

Commands are altered by flags and switches.

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
Change user while retaining current environment.

```bash
su -l user2
```
Change user and load complete environment (recommended).

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

---

### /var
Dynamic service data such as logs, caches, and databases.

```bash
sudo ls -l /var
```

---

### /root
Home directory for the root user.

```bash
sudo ls -l /root
```

---

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
scp allimportant.txt ubuntu@192.168.1.30:/home/ubuntu/
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

Processes are running programs controlled by the kernel.

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
Cron is a time-based job scheduler used to automate tasks.

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
APT repositories store software packages that are verified using GPG keys.

### Repository Locations
```text
/etc/apt/sources.list
/etc/apt/sources.list.d/
/etc/apt/trusted.gpg.d/
/etc/apt/preferences.d/
/etc/apt/apt.conf.d/
```

### Installing a Repository (Example: Sublime Text)

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

## 1. What is the `man` Command?

The `man` (manual) command is used to display **official documentation** for Linux commands, system calls, configuration files, and utilities.

### Syntax
```bash
man [section] command
```

### Examples
```bash
man ls
man nano
man nc
```

---

## 2. Useful `man` Command Options

| Option | Description |
|-----|-------------|
| -k keyword | Search all man pages for a keyword |
| -f command | Show a short description of the command |
| -a command | Display all matching manual pages |
| -w command | Show the location of the man page file |

### Examples
```bash
man -k network
man -f ls
man -a passwd
man -w nano
```

---

## 3. Structure of a Man Page

Common sections found inside a man page:

| Section | Purpose |
|------|--------|
| NAME | Command name and short description |
| SYNOPSIS | Command syntax |
| DESCRIPTION | Detailed explanation |
| OPTIONS | Available flags/switches |
| EXAMPLES | Usage examples |
| SEE ALSO | Related commands |

---
## 4. `nano` Man Page

### Command
```bash
man nano
```

### Common Options

| Option | Meaning |
|-----|--------|
| -l | Display line numbers |
| -c | Show cursor position |
| -i | Auto-indent new lines |
| -B | Create a backup file |
| -R | Open file in read-only mode |

### Examples
```bash
nano -l file.txt
nano -R config.conf
```

---

## 5. `netcat (nc)` Man Page

### Command
```bash
man nc
```

### Common Options

| Option | Meaning |
|-----|--------|
| -l | Listen for incoming connections |
| -p | Specify local port |
| -v | Verbose output |
| -n | Disable DNS resolution |
| -z | Zero-I/O mode (used for scanning) |

### Examples
```bash
nc -lvnp 4444
nc -zv 127.0.0.1 1-1000
```

---
## 6. `ls` Man Page 

### Command
```bash
man ls
```

### Common Options

| Option | Description |
|-----|-------------|
| -l | Long listing format |
| -a | Show hidden files |
| -h | Human-readable file sizes |
| -R | Recursive directory listing |

### Example
```bash
ls -lah
```

---

## 7. `grep` Man Page

### Command
```bash
man grep
```

### Common Options

| Option | Description |
|-----|-------------|
| -i | Ignore case |
| -r | Recursive search |
| -n | Show line numbers |
| -v | Invert match |

### Example
```bash
grep -rin "password" /etc
```
---
