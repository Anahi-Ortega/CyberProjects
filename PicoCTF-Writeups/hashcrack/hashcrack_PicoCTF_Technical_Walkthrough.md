# hashcrack - Techincal security Writeup  

## Overview  
**Challenge:** hashcrack  
**Platform:** PicoCTF 2025  
**Difficulty:** Easy  
**Assesment Type:** Black-box/ Remote Service  
**Category:** Cryptography  
**Date Completed:** February 16, 2026  
**Tools Used:** CrackStation, Netcat  
**Description:** A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?


## Objectives
- Identify the hashing algorithms used  
- Crack each provided hash  
- Retrieve the final flag from the remote service

## Attach Path Summary
1. Connected to the remote challenge service using Netcat  
2. Identified hash types based on length and structure  
3. Used an online hash-cracking database to recover plaintext passwords  
4. Submitted cracked values to the service  
5. Retrieved the final flag

## Methodology & Findings
### 1. Service Interaction
Connected to the remote challenge instance:
~~~
nc verbal-sleep.picoctf.net 60717
~~~
The services prompted for plaintext passwords corresponding to provided hashes.  

### 2. Hash Identification & Cracking
Hash lengths were used to determine algorithm types:

| Hash | Length | Algorithm |
|------|--------|-----------|
|482c811da5d5b4bc6d497ffa98491e38 |32 chars | MD5 |
|b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3 | 40 chars | SHA-1 |
|916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745 | 64 chars | SHA-256 |

**Identification Logic:**
The hashes were submitted to CrackStation, a public hash database that contains precomputed hashes of common passwords.

### 3. Crached Passwords
| Algorithm | Hash | Plaintext |
|-----------|------|-----------|
|MD5 | 482c811da5d5b4bc6d497ffa98491e38 | password123 |
|SHA-1 | b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3 | letmein |
|SHA=256 | 916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745 | qwerty098 |

Each recoreved plaintext password was entered back into the Netcat session to prompt the next hash.

### 4. Flag Retrieval
After submitting the final cracked password: 
~~~
Correct! You've cracked the SHA-256 hash with a secret found.
The flag is: picoCTF{UseStr0nG_h@shEs_&PaSswDs!_4c95d69f}
~~~

## Security Analysis
**Root Cause**
The challenge demonstrates the weakness of: 
- Using common passwords
- Using fast hashing algorithms without salting
- Relying on outdated hashing algorithms (MD5, SHA-1)

**Why This Was Vulnerable**
- MD5 and SHA-1 are cryptographically broken
- All hashes were unsalted
- Passwords were common dictionary entries
- Public rainbow tables and hash databases exist

Because the passwords were weak and unsalted, they were instantly recoverable via lookup tables.

## Defensive Recommendations
To prevent similar compromises:
1. Use strong password policies
2. Implement salted password hashing
3. Use slow, adaptive hashing algorithms such as:
   - bcrypt
   - Argon2
   - PBKDF2
4. Enforce minimum password complexity
5. Implement account lockout policies against brute force attempts

## Key Takeaways
- Hashing alone does not make passwords secure.
- Fast hash algorithms are inappropriate for password storage.
- Weak passwords are easily cracked even with strong algorithms.
- Salting significantly increases resistance against rainbow table attacks.

## Final Flag
~~~
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_4c95d69f}
~~~
