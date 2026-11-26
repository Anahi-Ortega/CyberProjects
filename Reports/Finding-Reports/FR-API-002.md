# API-002 – Finding and Exploiting an Unused API Endpoint

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
| Lab | [Finding and exploiting an unused API endpoint](https://portswigger.net/web-security/api-testing/lab-exploiting-unused-api-endpoint) |
| Lab ID | API-002 |
| Difficulty | Practitioner |
| Category | API Security / Broken Object Property Level Authorization |
| Date | November 12, 2026 |
| Tools | Burp Suite Community Edition, Web Browser |
| Status | Completed |

## 2. Summary

This lab demonstrated how an unused API endpoint could expose functionality that was not available through the application's standard shopping interface.

By examining the application's API traffic and using Burp Suite to identify supported HTTP methods, I discovered that the product price endpoint accepted PATCH requests. I modified the product's price through the API, reducing it to $0.00 and successfully purchasing the item.

The exercise demonstrated the security implications of insufficient authorization and validation surrounding sensitive object properties exposed through API endpoints.

## 3. Classification

| Classification | Details |
|---|---|
| Primary Weakness | CWE-862: Missing Authorization (provisional) |
| Related Weakness | CWE-915: Improperly Controlled Modification of Dynamically-Determined Object Attributes |
| OWASP API Security Top 10 | API3:2023 – Broken Object Property Level Authorization |
| Severity | High |
| Illustrative CVSS v3.1 | 6.5 (Medium) |
| CVSS Vector | AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N |
| MITRE ATT&CK | T1190 – Exploit Public-Facing Application |

**Classification Rationale:**

OWASP API3:2023 addresses situations where an application fails to properly enforce authorization at the individual object-property level.

In this lab, the price property of a product could be modified through an API request even though it should not have been available for unrestricted modification by an ordinary customer.

CWE-915 is relevant to the ability to modify dynamically selected object attributes without appropriate controls. CWE-862 is a broader classification for missing authorization.

The exact implementation-level cause was not available for inspection, so these classifications describe the observed behavior rather than verified source-code defects.

## 4. Scope & Environment

**Target Environment:**
- Platform: PortSwigger Web Security Academy
- Application: Simulated shopping application
- Authenticated User: `wiener`
- Provided Credentials: `wiener:peter`
- Target Product: Lightweight l33t Leather Jacket
- Product Identifier: `1`
- API Endpoint: `/api/products/1/price`

**Scope:**

Testing was restricted to the intentionally vulnerable PortSwigger environment.

The objective was to identify an unused API endpoint, determine its supported HTTP methods, and investigate whether its functionality could be manipulated to alter product pricing.

## 5. Methodology

### Phase 1: Application Reconnaissance

I began by logging into the application using the credentials provided by PortSwigger.

After authentication, I navigated to the shopping page and identified the Lightweight l33t Leather Jacket.

I reviewed the product URL and identified its product number as `1`.

I added multiple jackets to my shopping cart and navigated to the cart to observe how the application handled product information and pricing.

### Phase 2: API Endpoint Discovery

Using Burp Suite, I examined the Target Site Map to identify available API endpoints.

During this investigation, I discovered:

```http
/api/products/1/price
```

Reviewing the endpoint's response revealed that the application returned the product's current price.

This presented an opportunity to investigate whether the endpoint supported additional operations beyond retrieving pricing information.

### Phase 3: HTTP Method Enumeration

I forwarded the GET request to Burp Suite Repeater and changed the HTTP method to OPTIONS.

The server returned the following response header:

```http
Allow: GET, PATCH
```

This indicated that the endpoint supported both GET and PATCH methods.

I then changed the request method to PATCH to investigate whether the product price could be modified.

### Phase 4: Request Modification

My initial PATCH request returned the following error:

```json
{
  "type": "ClientError",
  "code": 400,
  "error": "Only 'application/json' Content-Type is supported"
}
```

The error indicated that the endpoint expected the request body to use JSON formatting.

I updated the request headers to include:

```http
Content-Type: application/json
```

This allowed the request to be processed using the expected content type.

I then supplied a JSON request body containing the modified price.

### Phase 5: Exploitation and Verification

After submitting the modified PATCH request, I revisited the shopping cart.

The product price was now displayed as `$0.00`.

I proceeded to complete the purchase and received the order confirmation screen, successfully completing the lab.

## 6. Proof of Concept

### Discovered Endpoint

```http
GET /api/products/1/price
```

The endpoint returned the current price associated with product identifier `1`.

### HTTP Method Enumeration

```http
OPTIONS /api/products/1/price
```

**Observed Response Header:**

```http
Allow: GET, PATCH
```

This identified PATCH as an additional supported method.

### PATCH Request

The following represents the request structure used to modify the product price. The exact original JSON body was not preserved in the walkthrough.

```http
PATCH /api/products/1/price HTTP/1.1
Content-Type: application/json
```

**Illustrative JSON Body:**

```json
{
  "price": 0
}
```

### Observed Result

The application accepted the modified price.

Upon returning to the shopping cart, the Lightweight l33t Leather Jacket displayed a price of:

```text
$0.00
```

The purchase was subsequently completed, and the application displayed an order confirmation.

## 7. Root Cause

The observed vulnerability involved insufficient restrictions on modifying a sensitive product property through an API endpoint.

Although the application exposed the price endpoint for retrieving product information, it also accepted PATCH requests that allowed the price to be changed.

The application did not adequately restrict the authenticated user's ability to modify this sensitive property.

This is consistent with Broken Object Property Level Authorization, where an application fails to enforce appropriate permissions over individual object properties.

The underlying source code was not available for inspection, so the precise implementation defect could not be independently verified.

## 8. Impact

In a production e-commerce application, allowing unauthorized modification of product prices could have serious financial and operational consequences.

Potential impacts include:

- **Financial Loss:** Customers could manipulate prices and purchase products below their intended cost.
- **Business Logic Abuse:** Attackers could bypass intended purchasing rules and manipulate transaction values.
- **Data Integrity Compromise:** Unauthorized modifications could affect product records and pricing information.
- **Inventory and Order Discrepancies:** Manipulated transactions could create inconsistencies between product pricing, order records, and payment processing.
- **Broader API Abuse:** Similar authorization weaknesses could potentially affect other sensitive product attributes.

The demonstrated impact in this lab was the successful modification of a product's price to $0.00 and completion of the purchase.

## 9. Remediation

### 1. Enforce Property-Level Authorization

Restrict modifications to sensitive properties such as product pricing.

Only appropriately authorized administrative roles should be able to modify product prices.

### 2. Implement Server-Side Price Validation

Product prices should be retrieved and validated server-side during checkout.

The application should never rely exclusively on client-controlled pricing information when calculating transaction totals.

### 3. Restrict HTTP Methods

Only expose HTTP methods that are required for the endpoint's intended functionality.

If PATCH is not intended for ordinary users, it should be unavailable to those users through appropriate authorization controls.

### 4. Apply Role-Based Access Control

Ensure that product-management operations are restricted to authorized personnel.

A standard customer account should not have permission to modify catalog pricing.

### 5. Validate API Request Properties

Use explicit allowlists to determine which properties can be modified through each API operation.

Sensitive properties should not be writable through unrestricted or mass-assignment behavior.

## 10. Detection & Response

### Potential Detection Indicators

Security teams should monitor for:

- PATCH requests targeting product pricing endpoints.
- Unexpected modifications to product price properties.
- Requests modifying sensitive object attributes from ordinary customer accounts.
- Unusual changes to product prices immediately before checkout.
- Transactions completed using unexpected or unusually low product prices.
- Repeated attempts to enumerate API endpoints and supported HTTP methods.

### Relevant Log Sources

| Log Source | Information to Investigate |
|---|---|
| API Gateway Logs | HTTP methods, endpoints, request identities, and response codes. |
| Application Logs | Product modifications and authorization decisions. |
| Database Audit Logs | Changes to product pricing records. |
| E-commerce Transaction Logs | Order totals, product prices, and transaction discrepancies. |
| WAF Logs | Unusual API requests and suspicious request patterns. |

### Recommended Detection and Response Actions

1. Identify unauthorized PATCH requests affecting product pricing.
2. Correlate price modifications with authenticated user identities and assigned roles.
3. Review product records to determine whether unauthorized changes occurred.
4. Examine transactions completed after suspicious pricing modifications.
5. Restore legitimate product pricing and investigate affected orders.
6. Restrict unauthorized access to sensitive API operations.
7. Validate that server-side pricing controls cannot be bypassed through direct API requests.

## 11. Lessons Learned

This lab expanded my understanding of API reconnaissance and demonstrated the importance of examining available HTTP methods and endpoint functionality.

Key takeaways included:

- **API Endpoint Discovery:** Used Burp Suite's Target Site Map to identify API endpoints that were not immediately apparent through the application's interface.
- **HTTP Method Enumeration:** Learned how OPTIONS requests can reveal supported HTTP methods.
- **Burp Suite Repeater:** Practiced modifying HTTP requests and testing different methods before forwarding them to the application.
- **Content-Type Headers:** Learned how request headers influence how an API processes incoming data.
- **Error Message Analysis:** Used an application error response to identify the expected request format and adjust my subsequent request.
- **API Property Manipulation:** Demonstrated how insufficient restrictions on sensitive object properties can affect application business logic.

One of my biggest takeaways was becoming more comfortable exploring Burp Suite's functionality and experimenting with requests in Repeater.

I initially understood how to identify relevant API endpoints, but needed additional practice manipulating the requests themselves. Working through the error response helped me understand how to use application feedback to refine an investigation.

## 12. References

- [PortSwigger – Finding and Exploiting an Unused API Endpoint](https://portswigger.net/web-security/api-testing/lab-exploiting-unused-api-endpoint)
- [PortSwigger – API Testing](https://portswigger.net/web-security/api-testing)
- [CWE-862 – Missing Authorization](https://cwe.mitre.org/data/definitions/862.html)
- [CWE-915 – Improperly Controlled Modification of Dynamically-Determined Object Attributes](https://cwe.mitre.org/data/definitions/915.html)
- [OWASP API3:2023 – Broken Object Property Level Authorization](https://api-security.owasp.org/)

## Appendix – Additional Evidence

Evidence documented during the exercise included:

- Authenticated application session.
- Lightweight l33t Leather Jacket product listing.
- Burp Suite Target Site Map showing discovered multiple API endpoints.
- GET request to the product price endpoint.
- OPTIONS response identifying supported HTTP methods.
- PATCH request and application error response.
- Updated product price displayed in the shopping cart.
- Successful order confirmation.

The original walkthrough documented the successful outcome and includes images of exact JSON request body and some request and response details.

---

*Disclaimer: This walkthrough documents an authorized PortSwigger Web Security Academy training exercise. All testing was performed within the intentionally vulnerable lab environment.*