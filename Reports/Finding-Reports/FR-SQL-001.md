# SQL-001 – SQL Injection in WHERE Clause Allowing Retrieval of Hidden Data

## Table of Contents
| Section | Title |
|---|---|
| 1 | [Metadata](#1-metadata) |
| 2 | [Summary](#2-summary) |
| 3 | [Classification](#3-classification) |
| 4 | [Scope & Environment](#4-scope--environment) |
| 5 | [Methodology](#5-methodology) |
| 6 | [Proof of Concept](#6-proof-of-concept) |
| 7 | [Root Cause](#7-root-cause) |
| 8 | [Impact](#8-impact) |
| 9 | [Remediation](#9-remediation) |
| 10 | [Detection & Response](#10-detection--response) |
| 11 | [Lessons Learned](#11-lessons-learned) |
| 12 | [References](#12-references) |
| [Appendix](#appendix--dead-ends--additional-evidence) | |


## 1. Metadata

| Field | Details |
|---|---|
| Platform | PortSwigger Web Security Academy |
| Lab | [SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data) |
| Lab ID | SQL-001 |
| Difficulty | Apprentice |
| Category | SQL Injection / Data Exposure |
| Date | October 26, 2026 |
| Tools | Web Browser |
| Status | Completed |

## 2. Summary

This lab demonstrated a SQL injection vulnerability in the application's product category filtering functionality.

By modifying the category parameter in the URL, I was able to manipulate the underlying SQL query and bypass the condition restricting results to released products.

The successful injection caused the application to display unreleased products that were otherwise hidden from the normal shopping interface.

## 3. Classification

| Classification | Details |
|---|---|
| Primary Weakness | CWE-89: Improper Neutralization of Special Elements used in an SQL Command |
| OWASP Top 10:2025 | A05:2025 – Injection |
| Vulnerability Type | SQL Injection |
| Severity | Medium |
| Illustrative CVSS v3.1 | 6.5 (Medium) |
| CVSS Vector | AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N |
| MITRE ATT&CK | T1190 – Exploit Public-Facing Application |

**Classification Rationale:**

CWE-89 applies because user-controlled input was incorporated into a SQL query in a manner that allowed the intended query logic to be modified.

OWASP A05:2025 covers injection vulnerabilities, including SQL injection.

MITRE ATT&CK T1190 describes the exploitation of publicly accessible application vulnerabilities to obtain initial access.

**CVSS Note:** The illustrative score assumes that an unauthenticated remote attacker can retrieve information intended to remain hidden from normal application users, without modifying application records or disrupting availability.

The confidentiality impact is rated Low for this hypothetical scenario because the demonstrated exposure involved unreleased product information rather than confirmed sensitive personal or financial data.

## 4. Scope & Environment

**Target Environment:**
- Platform: PortSwigger Web Security Academy
- Application: Simulated shopping application
- Vulnerable Component: Product category filter
- Injection Parameter: `category`
- Database Interaction: SQL WHERE clause

**Scope:**

Testing was restricted to the intentionally vulnerable PortSwigger training environment.

The objective was to identify and exploit a SQL injection vulnerability in the product category filtering functionality to retrieve products that were not intended to be displayed.

## 5. Methodology

### Phase 1: Application Reconnaissance

I launched the lab and reviewed the shopping application's navigation interface.

The application provided several product categories that could be selected through the navigation bar.

Selecting a category modified the URL to include the selected category as a parameter.

This indicated that the application was using user-controlled input to determine which products were displayed.

### Phase 2: Vulnerability Identification

The lab instructions provided an example of the underlying SQL query:

```sql
SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1
```

The query retrieves products belonging to the selected category while restricting results to products where the `released` field is set to `1`.

The category parameter therefore represented a potential injection point.

### Phase 3: Exploitation

Based on the SQL injection examples covered in the preceding instructional material, I tested the category parameter using the following payload:

```sql
'+OR+1=1--
```

The payload was appended to the category parameter in the URL.

This introduced an additional SQL condition that evaluated as true and used a comment sequence to exclude the remaining portion of the original query.

### Phase 4: Verification

After submitting the modified URL, the application returned products that were not previously visible through the standard category filtering functionality.

The presence of unreleased products confirmed that the injection successfully altered the intended query behavior.

## 6. Proof of Concept

### Original SQL Query

The lab provided the following example of the application's query:

```sql
SELECT * FROM products
WHERE category = 'Gifts'
AND released = 1
```

The query contains two conditions:

- `category = 'Gifts'`: Restricts results to the selected product category.
- `released = 1`: Restricts results to products marked as released.

### Injection Payload

The following payload was submitted through the category parameter:

```sql
'+OR+1=1--
```

### Reconstructed SQL Query

The following illustrates how the query logic changes after injection:

```sql
SELECT * FROM products
WHERE category = '' OR 1=1--'
AND released = 1
```

The condition `1=1` always evaluates to true.

The `--` sequence comments out the remaining portion of the query under the applicable SQL dialect.

Consequently, the original category and release restrictions are bypassed.

### Observed Result

The application displayed unreleased products that were previously excluded from the product listing.

This confirmed successful exploitation of the SQL injection vulnerability.

## 7. Root Cause

The underlying vulnerability was the application's apparent use of unsanitized user input when constructing its SQL query.

The category parameter was incorporated into the database query in a way that allowed specially crafted input to modify the intended SQL logic.

Instead of treating the category value strictly as data, the application interpreted injected SQL syntax as part of the query.

This allowed the original filtering conditions to be bypassed.


## 8. Impact

SQL injection vulnerabilities can expose information that an application intends to restrict from users.

In a production environment, similar weaknesses could result in:

- **Unauthorized Data Exposure:** Attackers could retrieve information that should not be accessible through normal application functionality.
- **Access Control Bypass:** Database filtering conditions could potentially be manipulated to expose restricted records.
- **Sensitive Information Disclosure:** Depending on the query and database permissions, attackers could potentially retrieve personal, financial, or business-sensitive information.
- **Further Database Exploitation:** More advanced SQL injection techniques could potentially allow unauthorized database modification or additional information extraction.

The demonstrated impact in this lab was the successful retrieval of unreleased product records.

The broader consequences depend on the application's database structure, permissions, and the data accessible through vulnerable queries.

## 9. Remediation

### 1. Use Parameterized Queries

Use prepared statements and parameter binding to ensure that user input is interpreted as data rather than executable SQL syntax.

For example:

```sql
SELECT * FROM products
WHERE category = ?
AND released = 1;
```

The category value should be supplied separately through the database driver's parameter-binding functionality.

### 2. Implement Server-Side Input Validation

Validate category values against an expected list of available categories.

Unexpected or malformed input should be rejected rather than incorporated into database queries.

### 3. Apply Least-Privilege Database Permissions

Ensure that application database accounts have only the permissions necessary to perform their intended operations.

This limits the potential impact if SQL injection is discovered elsewhere in the application.

### 4. Avoid Dynamic SQL Construction

Avoid concatenating user-controlled input directly into SQL statements.

Use secure query-building methods that maintain a clear separation between query logic and user-provided values.

### 5. Implement Appropriate Error Handling

Avoid exposing database errors or internal query information through application responses.

Detailed errors should be recorded in protected application logs for troubleshooting and security investigations.

## 10. Detection & Response

### Potential Detection Indicators

Security teams should monitor for:

- SQL operators and keywords appearing in category parameters.
- Unexpected SQL comment sequences such as `--`.
- Boolean expressions such as `OR 1=1`.
- Unusual quotation marks in URL parameters.
- Repeated requests containing variations of SQL injection payloads.
- Unexpected retrieval of records marked as unreleased.
- Application or database errors associated with malformed queries.

These patterns are indicators for investigation rather than conclusive evidence of exploitation.

### Relevant Log Sources

| Log Source | Information to Investigate |
|---|---|
| Web Server Access Logs | Requested URLs, query parameters, source IPs, and response codes. |
| Application Logs | Category filtering requests and unexpected application behavior. |
| Database Audit Logs | Query execution and unusual database access patterns. |
| WAF Logs | Requests matching SQL injection detection rules. |
| Network Monitoring | Unusual request patterns and repeated attempts against application endpoints. |

### Recommended Detection and Response Actions

1. Investigate requests containing suspicious SQL syntax in application parameters.
2. Identify the source of the requests and determine whether additional endpoints were targeted.
3. Correlate suspicious HTTP requests with application and database activity.
4. Determine whether restricted records were accessed or exposed.
5. Review database permissions and identify other potentially vulnerable query operations.
6. Implement parameterized queries and validate the remediation.
7. Monitor the affected endpoint for additional exploitation attempts.

## 11. Lessons Learned

This lab introduced practical SQL injection techniques and demonstrated how user-controlled URL parameters can influence database query execution.

Key takeaways included:

- **SQL Query Structure:** Improved my understanding of how WHERE clauses determine which database records are returned.
- **URL Parameter Manipulation:** Practiced modifying category parameters directly through the browser.
- **Boolean-Based SQL Injection:** Learned how expressions such as `OR 1=1` can alter query conditions.
- **SQL Comment Syntax:** Practiced using comment sequences to exclude portions of an existing SQL statement.
- **Data Exposure:** Demonstrated how SQL injection can bypass application filtering and reveal records that should remain hidden.
- **Secure Query Construction:** Reinforced the importance of parameterized queries and separating user input from SQL commands.

The exercise helped establish a foundation for understanding how SQL injection works before progressing to more advanced exploitation techniques.

## 12. References

- [PortSwigger – SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)
- [PortSwigger – SQL Injection](https://portswigger.net/web-security/sql-injection)
- [CWE-89 – Improper Neutralization of Special Elements used in an SQL Command](https://cwe.mitre.org/data/definitions/89.html)
- [OWASP Top 10:2025 – A05 Injection](https://owasp.org/Top10/2025/A05_2025-Injection/)
- [MITRE ATT&CK – T1190 Exploit Public-Facing Application](https://attack.mitre.org/techniques/T1190/)

## Appendix – Additional Evidence

Evidence documented during the exercise included:

- Initial shopping application interface.
- Product category navigation.
- URL containing the selected category parameter.
- SQL injection payload used in the category parameter.
- Application response displaying unreleased products.

The lab instructions provided the original SQL query, which was used to explain the effect of the injection.

The reconstructed SQL statement represents the observed query behavior rather than verified backend source code.

---

*Disclaimer: This walkthrough documents an authorized PortSwigger Web Security Academy training exercise. All testing was performed within the intentionally vulnerable lab environment.*