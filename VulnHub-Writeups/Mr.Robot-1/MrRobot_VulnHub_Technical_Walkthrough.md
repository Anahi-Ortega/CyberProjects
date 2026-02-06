# Mr. Robot: 1 — Technical Security Walkthrough

## Overview
**Machine:** Mr-Robot: 1
**Platform:** VulnHub
**Difficulty:** Beginner–Intermediate
**Date Completed:** August 17, 2025
**Operating System:** Linux
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

---

### 1. Reconnaissance  
**MITRE ATT&CK:**  
- `T1046` – Network Service Discovery  
**NIST CSF:**  
- `ID.AM-2` – Software platforms and applications identified  

'''
sudo nmap -sS -T4 10.38.110-120
'''
**

## Flags Captured  
Flag 1: 073403c8a58a1f80d943455fb30724b9  
Flag 2: 822c73956184f694993bede3eb39f959  
Flag 3: 04787ddef27c3dee1ee161b21670b4e4  
---

## Lessons Learned
- Deploy password salting  
- Require login attempt limit for timeframes
- 
- 
---

## References
Machine on Vulnhub: https://www.vulnhub.com/entry/mr-robot-1,151/
Video Tutorial: https://www.youtube.com/watch?v=D_aiSOmC6V8
