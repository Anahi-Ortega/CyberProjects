# API-001 - Exploiting an API endpoint using documentation 

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
| 10 | [Detection & Response](#10-detection--respose) |
| 11 | [Lessons Learned](#11-lessons-learned) |
| 12 | [References](#12-references) |
| [Appendix](#appendix) | |

## 1. Metadata

| Field | Details |
|---|---|
| Platform | PortSwigger Web Security Academy |
| Lab | [Exploiting an API endpoint using documentation](https://portswigger.net/web-security/api-testing/lab-exploiting-api-endpoint-using-documentation) |
| Lab ID | API-001 |
| Difficulty | Apprentice |
| Category | API Security / Authorization |
| Date | November 3, 2026 |
| Tools | Web Browser, Burp Suite |
| Status | Completed |

## 2. Summary

This lab demonstrated how exposed API documentation can reveal application functionality that may not be accessible through the normal user interface.

By discovering an API documentation endpoint and examining its interactive functionality, I identified a DELETE operation that allowed me to delete another user's account. This demonstrated the security implications of insufficient authorization controls on sensitive API operations.

## 3. Classification

| Classification | Details |
|---|---|
| Primary Weakness | CWE-862: Missing Authorization |
| OWASP API Security Top 10 | API5:2023 – Broken Function Level Authorization |
| Related Classification | API1:2023 – Broken Object Level Authorization (potentially applicable depending on the authorization failure) |
| Severity | High |
| Illustrative CVSS v3.1 | 6.5 (Medium) |
| CVSS Vector | AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N |
| MITRE ATT&CK | T1190 – Exploit Public-Facing Application |

**Classification Rationale:**

CWE-862 describes the absence of an authorization check when a user attempts to access a resource or perform an action.

OWASP API5:2023 applies when a regular user can access functionality that should be restricted to a different role or privilege level.

API1:2023 may also apply if the underlying issue is specifically the failure to verify whether the authenticated user is authorized to perform an operation against another user's object.

The lab demonstrates unauthorized deletion of another user's account. The exact underlying authorization implementation was not available for inspection.

**CVSS Note:** The illustrative score assumes an authenticated, low-privilege user can remotely delete another user's account without affecting confidentiality or availability.

## 4. Scope & Environment

**Target Environment:**
- Platform: PortSwigger Web Security Academy
- Application: Simulated shopping application
- API Documentation Endpoint: `/api/`
- Authenticated User: `wiener`
- Provided Credentials: `wiener:peter`
- Target Account: `carlos`

**Scope:**

Testing was limited to the intentionally vulnerable PortSwigger training environment.

The objective was to discover exposed API documentation and determine whether its available operations could be used to perform unauthorized account actions.

## 5. Methodology

### Phase 1: API Reconnaissance

I began by exploring the application and testing commonly used API documentation paths.

During this process, I discovered the following endpoint:

```http
/api/
```

Accessing the endpoint revealed API documentation containing available application operations.

### Phase 2: API Documentation Analysis

The documentation included interactive functionality that allowed API requests to be constructed and submitted directly through the browser.

I examined the available operations and identified a DELETE method that accepted a user identifier.

This revealed functionality that was not readily accessible through the application's standard interface.

### Phase 3: Exploitation

Using the interactive documentation, I selected the DELETE operation and supplied `carlos` as the target user.

The request was submitted to the application.

### Phase 4: Verification

The application returned an HTTP `200 OK` response, and the lab confirmed successful completion.

This demonstrated that the authenticated user could perform an account deletion operation against another user.

## 6. Proof of Concept

**Discovered API Documentation Endpoint:**

```http
GET /api/
```

**Target Operation:**

```http
DELETE /api/user/carlos
```
More specifically: 
```
curl -vgw "\n" -X DELETE 'https://[lab-instance].web-security-academy.net/api/user/carlos' -d '{}'
```

**Authentication Context:**

```text
Username: wiener
Password: peter
```

**Observed Result:**

```http
HTTP/1.1 200 OK
```

The application confirmed successful execution of the DELETE operation, and the lab was completed.

## 7. Root Cause

The underlying security weakness was insufficient authorization enforcement around an API operation that allowed users to delete accounts.

The application exposed interactive API documentation that revealed available functionality. More importantly, the DELETE operation accepted a target user identifier without adequately restricting the authenticated user's ability to perform the requested action.

Exposed documentation alone is not necessarily a vulnerability. The security issue is the ability to execute an unauthorized operation through the API.

## 8. Impact

In a production application, insufficient authorization controls on API operations could allow users to perform actions beyond their assigned permissions.

Potential consequences include:

- **Unauthorized Account Deletion:** Users could delete accounts belonging to other individuals.
- **Data Loss:** Account-associated information and application records could be permanently removed.
- **Business Disruption:** Unauthorized deletion could interfere with application operations and user access.
- **Abuse of Administrative Functionality:** Sensitive operations could be performed by users without the appropriate privileges.

The demonstrated impact in this lab was successful deletion of another user's account.

## 9. Remediation

### 1. Apply Role-Based Access Control (RBAC)

Restrict sensitive operations to authorized roles.

For example, account deletion functionality should be limited to administrators or appropriately authorized account owners.

### 2. Implement Server-Side Authorization

Enforce authorization checks on every sensitive API operation.

The application should verify that the authenticated user has the appropriate role and permissions before allowing account deletion.

### 3. Implement Object-Level Authorization

Verify that the authenticated user is authorized to perform operations against the specific account identified in the request.

Knowing or supplying another user's identifier should never be sufficient to gain access to their account.

### 4. Review API Documentation Exposure

Avoid exposing unnecessary internal API documentation in production environments.

Where documentation is intentionally accessible, ensure that its availability does not imply permission to execute documented operations.

### 5. Apply Default-Deny Authorization

Sensitive API operations should be denied unless explicit permissions have been granted.

## 10. Detection & Response

### Potential Detection Indicators

Security teams should monitor for:

- DELETE requests targeting user account resources.
- Authenticated users attempting to delete accounts other than their own.
- Unusual access to API documentation endpoints.
- Repeated requests to undocumented or administrative API operations.
- Successful account deletion events initiated by non-administrative users.

### Relevant Log Sources

| Log Source | Information to Investigate |
|---|---|
| API Gateway Logs | HTTP methods, requested endpoints, response codes, and authenticated identities. |
| Application Logs | Account deletion events and authorization decisions. |
| Authentication Logs | User identity, session information, and authentication status. |
| Database Audit Logs | DELETE operations affecting user records. |
| WAF Logs | Unusual API access patterns and suspicious requests. |

### Recommended Detection and Response Actions

1. Investigate DELETE requests targeting sensitive account resources.
2. Correlate API requests with authenticated user identities and assigned permissions.
3. Identify whether the requesting user had authorization to perform the operation.
4. Review account deletion events and determine whether additional accounts were affected.
5. Revoke affected sessions if account compromise is suspected.
6. Restore deleted accounts or associated records where possible.
7. Review API authorization controls and validate remediation through security testing.

## 11. Lessons Learned

This lab introduced the importance of API reconnaissance and understanding how exposed documentation can reveal application functionality.

Key takeaways included:

- **API Discovery:** Learned to identify commonly used API documentation endpoints through browser-based exploration.
- **Interactive API Documentation:** Explored how API documentation can provide an interface for constructing and submitting requests.
- **HTTP Methods:** Developed a better understanding of how DELETE operations can affect application resources.
- **Authorization Testing:** Recognized the importance of verifying whether authenticated users are permitted to perform sensitive operations.
- **Attack Surface Identification:** Learned that application functionality may exist beyond what is exposed through the standard user interface.

The exercise reinforced that API security requires examining both the available functionality and the authorization controls protecting it.

## 12. References

- [PortSwigger – Exploiting an API Endpoint Using Documentation](https://portswigger.net/web-security/api-testing/lab-exploiting-api-endpoint-using-documentation)
- [PortSwigger – API Testing](https://portswigger.net/web-security/api-testing)
- [CWE-862 – Missing Authorization](https://cwe.mitre.org/data/definitions/862.html)
- [OWASP API5:2023 – Broken Function Level Authorization](https://api-security.owasp.org/editions/2023/en/0xa5-broken-function-level-authorization/)
- [OWASP API Security Top 10](https://api-security.owasp.org/)

## Appendix – Additional Evidence

Evidence collected during the exercise included:

- Discovery of the `/api/` documentation endpoint.
- Interactive API documentation exposing available operations.
- DELETE operation targeting the `carlos` account.
- Successful HTTP response confirming execution.


 
---

*Disclaimer: This walkthrough documents an authorized PortSwigger Web Security Academy training exercise. All testing was performed within the intentionally vulnerable lab environment.*
