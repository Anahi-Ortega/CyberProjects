# Reports 📄

Professional-format reports from my labs, CTFs and simulations, all in one place. Walkthroughs show how I learned a technique. Reports present the result the way a client or security team would expect to receive it.

Every report links back to its source walkthrough, and the walkthrough links forward to the report.

---

## 🔹 Report Types

| Type | Folder | ID prefix | Audience | Scope |
|---|---|---|---|---|
| **Finding report** | `Finding-Reports/` *(created with the first report)* | `FR-` | Technical and security staff | One vulnerability |
| **Engagement report** | [`Engagement-Reports/`](./Engagement-Reports) | `ER-` | Executives and technical leads | A full assessment |
| **Incident report** | [`Incident-Reports/`](./Incident-Reports) | `IR-` | SOC and incident response | One investigated alert |

The folder is determined by the type of report. The platform appears in the index tables below.

---

## 🔹 Index

### Finding Reports

| ID | Title | Platform | Severity | Status |
|---|---|---|---|---|
| FR-SQL-001 | SQL Injection in Category Filter Exposing Hidden Data | PortSwigger | | Planned |
| FR-SQL-002 | Authentication Bypass via SQL Injection | PortSwigger | | Planned |
| FR-API-001 | Exposed API Documentation Enabling Account Deletion | PortSwigger | | Planned |
| FR-API-002 | Hidden API Endpoint Allowing Price Manipulation | PortSwigger | | Planned |

*Titles are working titles and may change when each report is written. A consolidated engagement report for PortSwigger will follow once a category is complete.*

### Engagement Reports

| ID | Title | Platform | Status |
|---|---|---|---|
| ER-001 | [Mr. Robot: 1 Executive Penetration Test Report](./Engagement-Reports/ER-001-Mr-Robot-1-Executive-Pentest-Test-Report.md) | VulnHub | Complete |

### Incident Reports

| ID | Title | Platform | Classification |
|---|---|---|---|
| IR-8814 | [Suspicious Email Link](./Incident-Reports/IR-8814-Suspicious-Email-Link-False-Positive.md) | TryHackMe SOC Simulator | False positive |
| IR-8815 | [Phishing Email with Shortened URL](./Incident-Reports/IR-8815-Phishing-Email-Shortened-URL.md) | TryHackMe SOC Simulator | True positive |
| IR-8816 | [Access to Blacklisted External URL Blocked by Firewall](./Incident-Reports/IR-8816-Access-to-Blacklisted-External-URL-Blocked-by-Firewall.md) | TryHackMe SOC Simulator | True positive |
| IR-8817 | [Phishing with Lookalike Microsoft Domain](./Incident-Reports/IR-8817-Phishing-Lookalike-Microsoft-Domain.md) | TryHackMe SOC Simulator | True positive |

*Incident IDs match the alert IDs in the simulator. Part of the SOC Simulator's Introduction to Phishing room; see the [write-up](../TryHackMe/SOC-Simulator/Introduction-to-Phishing/README.md).*

---

## 🔹 Naming Conventions

- **File names:** `ID-Descriptive-Title.md`, for example `FR-SQL-002-Authentication-Bypass-via-SQL-Injection.md`.
- **Finding report IDs** reuse the source lab's ID, so `SQL-002` becomes `FR-SQL-002`.
- **Status** is recorded in each report's header (Draft, v1.0) rather than in folder or file names.
- **Severity** uses CVSS v3.1 base scores. In lab environments the score is illustrative, since asset criticality and compensating controls do not apply.

---

## 🔹 Report Structures

**Finding report**

1. Document control and metadata
2. Executive summary
3. Finding summary (severity, CWE, OWASP, MITRE ATT&CK)
4. Scope and environment
5. Methodology with numbered, captioned evidence
6. Proof of concept
7. Root cause
8. Impact
9. Remediation
10. Detection and response
11. Lessons learned
12. References

**Engagement report:** executive summary, risk overview, attack narrative, business impact, key findings and recommendations, conclusion.

**Incident report:** time of activity, affected entities, classification rationale, escalation rationale, recommended remediation, attack indicators.

---

## ⚠️ Disclaimer

All reports describe activity in authorized training environments. Severity ratings, business impacts and recommendations are illustrative and do not represent real organizations or systems.