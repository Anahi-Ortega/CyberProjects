# Flag in Flame - Techincal Security Writeup  

## Overview  
**Challenge:** Flag in Flame  
**Platform:** picoMini by CMU-Africa
**Difficulty:** Easy  
**Assesment Type:** File Analysis / Encoding Analysis  
**Category:** Forensics  
**Date Completed:** February 19, 2026  
**Tools Used:**  Kali Linux, base64, file, Linux File Explorer, Online Hex Decoder  
**Description:** The SOC team discovered a suspiciously large log file after a recent breach. When they opened it, they found an enormous block of encoded text instead of typical logs. Could there be something hidden within? Your mission is to inspect the resulting file and reveal the real purpose of it. The team is relying on your skills to uncover any concealed information within this unusual log. Download the encoded data here: Logs Data. Be prepared—the file is large, and examining it thoroughly is crucial .


## Objectives
- Inspect the provided log file
- Identify the encoding format
- Decode the file contents
- Analyze the decoded output
- Extract the hidden flag

## Attack Path Summary 
1. Observed that the file contents were encoded rather than plaintext logs
2. Identified the encoding format as Base64
3. Decoded the Base64 content into a new file
4. Determined the decoded file type
5. Extracted hidden data from the resulting image
6. Decoded a hexadecimal string to retrieve the final flag

## Methodology & Findings

## Security Analysis

## Key Concepts Demonstrated

## Lessons Learned

## Final Flag
~~~
picoCTF{forensics_analysis_is_amazing_2561a194}
~~~
