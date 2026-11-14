# PortSwigger Web Security Academy 🧪

My walkthroughs and study notes for the free labs on [PortSwigger's Web Security Academy](https://portswigger.net/web-security). Progress is ongoing, and I add labs by category as I complete them.

Each walkthrough records how I solved the lab and what I learned. Selected labs are also written up as professional-format **finding reports** in [`Reports/`](../Reports).

---

## 🔹 Lab Index

| ID | Lab | Difficulty | Category | Technique | Walkthrough | Report |
|---|---|---|---|---|---|---|
| SQL-001 | SQL injection vulnerability in WHERE clause allowing retrieval of hidden data | Apprentice | SQL injection | WHERE clause injection, URL parameter manipulation | [Walkthrough](<./SQL/(SQL-001)SQL-injection-vulnerability-in-WHERE-clause-allowing-retrevial-of-hidden-data.md>) | Planned |
| SQL-002 | SQL injection vulnerability allowing login bypass | Apprentice | SQL injection | Authentication bypass, request interception | [Walkthrough](<./SQL/(SQL-002)SQL-injection-vulnerability-allowing-login-bypass.md>) | Planned |
| API-001 | Exploiting an API endpoint using documentation | Apprentice | API testing | API documentation discovery | [Walkthrough](<./API/(API-001)Exploiting-an-API-endpoint-using-documentation.md>) | Planned |
| API-002 | Finding and exploiting an unused API endpoint | Practitioner | API testing | Unused endpoint discovery | [Walkthrough](<./API/(API-002)Finding-and-exploiting-an-unused-API-endpoint.md>) | Planned |

> Difficulty labels (Apprentice, Practitioner, Expert) are PortSwigger's own.

---

## 🔹 Study Notes

| Topic | Notes |
|---|---|
| SQL injection | [SQL-Notes](./SQL/SQL-Notes.md) |
| API testing | [API-Notes](./API/API-Notes.md) |

---

## 🔹 How the Walkthroughs Are Organized

- **Structure:** Lab Instructions → Walkthrough → Lessons Learned.
- **IDs:** `SQL-001`, `API-002` and so on, numbered per category in the order completed.
- **File names:** `(CATEGORY-NNN)Lab-title.md`.
- **Screenshots:** stored in `images/<LAB-ID>/` inside each category folder.
- **Reports:** each finding report links back to its walkthrough, and each walkthrough links to its report once one exists.

---

## 🔹 Tools

- **Burp Suite Community Edition:** intercepting and modifying requests
- **Browser developer tools:** inspecting requests and responses

---

## 🔹 Attribution and Disclaimer

Labs, lab titles and difficulty levels belong to PortSwigger. The walkthroughs and reports in this repository are my own work. All testing was performed against PortSwigger's intentionally vulnerable lab environments, which are provided for this purpose. Use these techniques only on systems you own or have explicit written permission to test.