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
### 1. Initial File Inspection
Upon opening the downloaded file (logs_data.txt), the contents appeared as a large block of seemingly random alphanumeric characters.

This pattern strongly suggested Base64 encoding, which typically:
- Uses only A–Z, a–z, 0–9, +, /
- Often ends with = padding
- Produces long continuous encoded strings

### 2. Base64 Decoding
To decode the file:
~~~
base64 -d logs_data.txt > decoded_output
~~~
Command Breakdown:

- `base64 -d` → Decodes Base64 data  
- `>` → Redirects output into a new file  
- `decoded_output` → Stores the decoded binary data  

After decoding, the output was no longer readable text, indicating it was likely binary data.  

### 3. File Type Identification
To determine what type of file was produced:  
~~~
file decoded_output
~~~
The command identified the file as a PNG image.  

The `file` command inspects magic bytes (file signatures) to determine the true file type, regardless of file extension.  

### 4. Image Analysis
Opening the PNG image revealed a string of characters embedded visually within the image.  
The string appeared to be a hexadecimal sequence (characters 0–9 and a–f).  
> At this point, the investigation shifted from file analysis to encoding analysis.  

### 5. Hexadecimal Decoding

The extracted hex string was entered into an online hex decoder tool.  
Hex decoding converts hexadecimal byte representations back into readable ASCII text.  
After decoding, the output revealed the final flag:  
~~~
picoCTF{forensics_analysis_is_amazing_2561a194}
~~~
## Security Analysis

## Key Concepts Demonstrated

## Lessons Learned

## Final Flag
~~~
picoCTF{forensics_analysis_is_amazing_2561a194}
~~~
