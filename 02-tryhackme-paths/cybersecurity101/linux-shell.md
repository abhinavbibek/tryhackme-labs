# Linux Shells

Most Linux distributions use **Bash (Bourne Again Shell)** as their default shell. However, the shell you see when opening a terminal depends on the Linux distribution.

> **Note:** Different types of shells exist; these are discussed later.

You may already be familiar with basic Linux commands from Linux Fundamentals. Below is a brief recap of essential shell usage.

---

## Basic Linux Shell Commands

### Check Current Working Directory
```bash
pwd
```

**Example output:**
```text
/home/user
```

---

### Change Directory
```bash
cd Desktop
```

---

### List Directory Contents
```bash
ls
```

**Example output:**
```text
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

---

### Display File Contents
```bash
cat filename.txt
```

---

### Search Text in a File (`grep`)
```bash
grep THM dictionary.txt
```

**Example output:**
```text
The flag is THM
```

---

## Types of Linux Shells

### Check Current Shell
```bash
echo $SHELL
```

**Example output:**
```text
/bin/bash
```

---

### List Available Shells
```bash
cat /etc/shells
```

**Example output:**
```text
/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/bin/dash
/usr/bin/dash
/usr/bin/tmux
/usr/bin/screen
/bin/zsh
/usr/bin/zsh
```

---

### Switch Shell
```bash
zsh
```

---

### Change Default Shell
```bash
chsh -s /usr/bin/zsh
```

---

## Common Linux Shells

### Bash (Bourne Again Shell)
- Default shell on most Linux systems
- Supports scripting
- Tab completion
- Command history (`history`)
- Widely used and well documented

---

### Fish (Friendly Interactive Shell)
- Highly user-friendly
- Auto spell correction
- Syntax highlighting
- Customizable prompt and themes
- Supports scripting and command history

---

### Zsh (Z Shell)
- Advanced tab completion
- Auto spell correction
- Powerful scripting
- Highly customizable (e.g., **oh-my-zsh**)
- Plugin-based enhancements

---

## Shell Feature Comparison

| Feature | Bash | Fish | Zsh |
|------|------|------|-----|
| Full Name | Bourne Again Shell | Friendly Interactive Shell | Z Shell |
| Scripting | Widely compatible | Limited | Advanced |
| Tab Completion | Basic | Advanced suggestions | Plugin-extensible |
| Customization | Basic | Interactive tools | Extensive (oh-my-zsh) |
| User Friendliness | Moderate | High | High (with config) |
| Syntax Highlighting | No | Built-in | Via plugins |

---

## Shell Scripting Basics

Shell scripting allows automation by combining commands into `.sh` files.

---

### Create a Script
```bash
nano first_script.sh
```

---

### Shebang (Interpreter Definition)
```bash
#!/bin/bash
```

---

### Variables Example
```bash
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```

---

### Make Script Executable
```bash
chmod +x first_script.sh
```

---

### Run Script
```bash
./first_script.sh
```

---

## Loops

### For Loop Example
```bash
#!/bin/bash
for i in {1..10}; do
    echo $i
done
```

---

## Conditional Statements

### If-Else Example
```bash
#!/bin/bash
echo "Please enter your name first:"
read name

if [ "$name" = "Stewart" ]; then
    echo "Welcome Stewart! Here is the secret: THM_Script"
else
    echo "Sorry! You are not authorized to access the secret."
fi
```

---

## Comments

```bash
# This is a comment
```

Comments improve readability and do not affect execution.

---

## Locker Script (Combined Example)

```bash
#!/bin/bash

username=""
companyname=""
pin=""

for i in {1..3}; do
    if [ "$i" -eq 1 ]; then
        echo "Enter your Username:"
        read username
    elif [ "$i" -eq 2 ]; then
        echo "Enter your Company name:"
        read companyname
    else
        echo "Enter your PIN:"
        read pin
    fi
done

if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
    echo "Authentication Successful. You can now access your locker, John."
else
    echo "Authentication Denied!!"
fi
```
