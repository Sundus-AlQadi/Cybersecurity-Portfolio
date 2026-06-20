# Access Control Vulnerabilities

Status: Completed

Labs Completed: 13/13

Topics Covered:

* Unprotected Administrative Functionality
* Hidden Administrative Interfaces
* Privilege Escalation
* Vertical Privilege Escalation
* Horizontal Privilege Escalation
* Insecure Direct Object References (IDOR)
* GUID-Based IDOR
* File-Based IDOR
* User-Controlled Identifiers
* User Role Manipulation
* Mass Assignment
* URL-Based Access Control Bypass
* HTTP Method-Based Access Control Bypass
* Referer-Based Access Control Bypass
* Multi-Step Workflow Authorization Flaws
* Information Disclosure Through Access Control Weaknesses
* Password Disclosure Vulnerabilities
* Resource Ownership Validation
* Server-Side Authorization Failures

Tools Used:

* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* PortSwigger Web Security Academy
* Web Browser

Key Skills Gained:

* Access control testing
* Authorization analysis
* Privilege escalation testing
* IDOR discovery and exploitation
* HTTP request manipulation
* Session analysis
* User role assessment
* Resource ownership validation
* Multi-step workflow testing
* Information disclosure identification
* Server-side authorization evaluation

Key Concepts Learned:

* Authentication and authorization are different security concepts.
* Users should only access resources they own.
* Hidden URLs do not provide security.
* GUIDs improve unpredictability but do not replace authorization checks.
* Access control must always be enforced on the server side.
* HTTP methods should not determine authorization decisions.
* Client-controlled headers must never be trusted for authorization.
* Every step of a sensitive workflow requires authorization validation.
* Sensitive information must never be disclosed through unauthorized responses.
* Files, documents, APIs, and account data can all be vulnerable to IDOR attacks.
