# Log Hunt - Techincal Security Writeup  

## Overview  
**Challenge:** Log Hunt  
**Release:** picoMini by CMU-Africa  
**Difficulty:** Easy  
**Assesment Type:** Log Analysis / File Forensics
**Category:** General Skills  
**Date Completed:** February 19, 2026  
**Tools Used:** Kali Linux, cat, grep, sort, cut
**Description:** Our server seems to be leaking pieces of a secret flag in its logs. The parts are scattered and sometimes repeated. Can you reconstruct the original flag? Download the logs and figure out the full flag from the fragments.


## Objectives
- Analyze the provided log file
- Identify recurring patterns related to the flag
- Extract relevant log entries
- Remove duplicate entries
- Reconstruct the complete flag

## Attack Path Summary 
1. Inspected the log file for visible patterns
2. Identified recurring FLAGPART entries
3. Filtered log lines containing flag fragments
4. Removed duplicate entries
5. Stripped unnecessary log metadata (timestamps/log levels)
6. Reconstructed the full flag from cleaned output

## Methodology & Findings
### 1. Initial Log Inspection
The first step was to mannually inspect the log file:
~~~
cat server.log
~~~
**Reasoning:** This allowed for quick visual inspection of the file contents to identify any obvious patterns or anomalies.

**Finding:** The first line contained the keyword \FLAGPART\, suggesting that the flag was split across multiple log entries with this common word. 

### 2. Filtering Relevant Log Entries
Since mulpile lines contained \FLAFPART\, filtering was necessary to isolate relevenat entries: 
~~~
grep "INFO LOGPART" server.log | sort -u > cleanedLogs.txt
~~~
**Command Breakdown:**  
- grep ``"INFO FLAGPART"`` → Extracts lines containing both INFO and FLAGPART
- ``sort -u`` → Sorts output and removes duplicates
- ``>`` → Redirects output to a new file
This reduced noise adn removed exact duplicate log entries.

### 3. Refining the Output Further
To further clean the log entries and remove timestamps and unnecessary metadata (timestamps):
~~~
grep "FLAGPART" server.log | cut -c38- | sort -u > cleaned.log
~~~
**Command Breakdown:**
- ``grep "FLAGPART"`` → Extracts all lines containing flag fragments
- ``cut -c38``- → Removes the first 37 characters (timestamps/log formatting)
- ``sort -u`` → Removes duplicate flag fragments
- Output redirected to ``cleaned.log``
This command isolated only the unique flag fragments without timestamps or log-level prefixes.

### 4. Flag Reconstruction
The unique flag fragements can be reviewed.  
~~~
cat cleaned.log
~~~
The resulting fragments were combined in the correct order to form the final flag. 


## Security Analysis
**Root Cause**  
The vulnerability demonstrated in this challenge is:
- Sensitive data (flag fragments) logged in plaintext
- Repeated exposure of sensitive information
- Lack of log sanitization  

**Why This Is a Problem**
Logging secrets is a common real-world security issue. Logs often:
- Persist for long periods
- Are accessible to administrators and support staff
- May be aggregated into centralized logging systems
- Can be exposed in data breaches
Even partial leaks (like fragmented secrets) can be reconstructed with simple text-processing tools.

## Defensive Recommendations

1. Never log sensitive information (passwords, tokens, secrets)
2. Implement proper log scrubbing/sanitization
3. Use centralized logging with access controls
4. Apply log retention and rotation policies
5. Perform regular audits of application logs

## Key Takeaways
- Command-line tools are extremely powerful for log analysis.
- Pattern recognition is critical in CTF challenges and real-world investigations.
- Duplicate removal ``(sort -u)`` simplifies reconstruction tasks.
- Metadata stripping (``cut``) can quickly isolate relevant data.
- Sensitive information should never be written to logs in production systems.

## Final Flag
~~~
picoCTF{us3_y0urlinux_sk1lls_ cedfa5fb}
~~~
