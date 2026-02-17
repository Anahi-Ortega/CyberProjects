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

