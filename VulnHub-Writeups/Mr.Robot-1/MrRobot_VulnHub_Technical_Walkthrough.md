# Mr. Robot: 1 — Technical Security Walkthrough

## Overview
**Machine:** Mr-Robot: 1  
**Platform:** VulnHub  
**Difficulty:** Beginner–Intermediate  
**Date Completed:** August 17, 2025  
**Operating System:** Kali Linux  
**Assessment Type:** Black-box  
**Tools Used:** Kali Linux, Nmap, GoBuster, WPScan, Netcat, Python, Nano  

---

## Objectives
- Identify exposed services and attack surface
- Exploit web-based vulnerabilities
- Obtain initial access and escalate privileges
- Capture all three flags

---

## Attack Path Summary
1. Network reconnaissance identified exposed web services
2. Web enumeration revealed sensitive files and WordPress authentication
3. Credential brute-force led to authenticated access
4. Reverse shell enabled local enumeration
5. Misconfigured system binaries allowed privilege escalation to root

---


## Methodology & Findings

### 1. Reconnaissance  
**MITRE ATT&CK:**  
- `T1046` – Network Service Discovery  
**NIST CSF:**  
- `ID.AM-2` – Software platforms and applications identified  

~~~
sudo nmap -sS -T4 10.38.110-120
~~~

**Findings:**
- TCP 80 (HTTP) – Apache Web Server
- TCP 443 (HTTPS)
- TCP 22 (SSH)

**Impact:**
Public-facing web services expanded the attack surface and enabled further enumeration


### 2. Web Enumeration (Flag 1)
**MITRE ATT&CK:**
- `T1595.002` – Active Scanning: Vulnerability Scanning
- `T1083` – File and Directory Discovery
**NIST CSF:**
- `PR.IP-1` – Secure configuration of information systems
~~~
gobuster dir -u http://10.38.1.111 \
-w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
~~~

**Findings:**
- `/robots.txt` exposed:
  - Hidden directory containing Flag 1
  - Credential dictionary (`fsociety.disc`)
- WordPress login page discovered (`/wp-login.php`)

**Impact:**
Sensitive operational data was disclosed through misconfigured web controls. 


### 3. Credential Access & Exploitation (Flag 2)
**MITRE ATT&CK:**
- `T1110.002` – Brute Force: Password Cracking

- `T1078` – Valid Accounts
NIST CSF:
- `PR.AC-1` – Identity and credential management
~~~
cat fsociety.dic | sort -u > filtered.dic
wpscan --url http://10.38.1.111/wp-login.php \
-U elliot -P filtered.dic
~~~
**Result:**
- Valid credentials obtained for WordPress user elliot
- Administrative access to WordPress dashboard

**Impact:**
Weak authentication controls enabled unauthorized access.


### 4. Initial Access & Shell Execution
**MITRE ATT&CK:**
- `T1505.003` - Web Shell
- `T1059` - Command and Scripting Interpreter
**NIST CSF:**
- `DE.CM-7` - Monitoring for unauthorized activity
~~~
nc -lvnp 443
~~~
- A PHP reverse shell was uploaded and executed
- Interactive shell access obtained

**Impact:**
Remote command execution enabled full system enumeration.


### 5. Lateral Movement & Credential Cracking
**MITRE ATT&CK:**
- `T1003` – Credential Dumping
**NIST CSF:**
- `PR.AC-6` – Least privilege
- Located `/home/robot`
- Cracked MD5 hash to obtain `robot` user password
- Retrieved Flag 2


### 6. Privilege Escalation (Flag 3) 
**MITRE ATT&CK:**
- `T1068` – Exploitation for Privilege Escalation
- `T1548.003` – Abuse Elevation Control Mechanism
**NIST CSF:**
- `PR.IP-7` – Least functionality
~~~
nmap --interactive
!/bin/bash
~~~

**Root Cause:**
Outdated Nmap binary with interactive mode enabled allowed shell escape

**Impact:**
Complete system compromize (root access) and retrieval of Flag 3

## Flags Captured  
Flag 1: 073403c8a58a1f80d943455fb30724b9  
Flag 2: 822c73956184f694993bede3eb39f959  
Flag 3: 04787ddef27c3dee1ee161b21670b4e4  

---

## Mitigations  
- Remove sensitive data from robots.txt  
- Enforce strong password policies and rate limiting  
- Harden WordPress authentication  
- Restrict executable binaries  
- Patch legacy software  
- Apply least-privilege principles  

---

## References
Machine on Vulnhub: https://www.vulnhub.com/entry/mr-robot-1,151/  
MITRE ATT&CK: https://attack.mitre.org  
NIST CSF: https://www.nist.goc/cyberframework  
Video Tutorial: https://www.youtube.com/watch?v=D_aiSOmC6V8
