# Day 10 Progress - Advanced Access Control and Learning Path Completion

## Completed Labs

* Access Control Lab 11: Insecure Direct Object References
* Access Control Lab 12: Multi-Step Process with No Access Control on One Step
* Access Control Lab 13: Referer-Based Access Control

## Topics Covered

* Insecure Direct Object References (IDOR)
* File-based IDOR
* Access control on file resources
* Resource ownership validation
* Information disclosure through file access
* Multi-step workflow vulnerabilities
* Authorization bypass in confirmation steps
* Workflow security validation
* Referer header manipulation
* Header-based access control weaknesses
* Privilege escalation
* Authorization testing
* Server-side access control validation

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Burp Proxy
* Web Browser
* GitHub

## Reflection

Today I completed the final three labs in the Access Control learning path and finished the entire section.

In the first lab, I explored a file-based IDOR vulnerability where chat transcripts were stored using predictable file names. By modifying the transcript identifier, I was able to access another user's conversation and retrieve sensitive information. This demonstrated that IDOR vulnerabilities can affect files and documents, not just user profiles or account pages.

In the second lab, I learned how multi-step workflows can become vulnerable when authorization checks are applied only to the initial stages of a process. Although the application protected the first step of the user promotion workflow, the final confirmation request lacked proper authorization validation. By replaying the confirmation request with a normal user session, administrative privileges were obtained.

In the final lab, I examined access control that relied on the HTTP Referer header. The application incorrectly trusted a client-controlled header to determine whether a request should be authorized. By replaying an administrative request while preserving the Referer value, it was possible to bypass authorization checks and gain administrator privileges.

These labs reinforced the importance of server-side authorization validation, proper workflow security, and the risks of trusting user-controlled input when making access control decisions.

## Current Progress

* Completed SQL Injection fundamentals and database enumeration labs
* Completed 12 Authentication labs (all Apprentice and Practitioner labs completed; 2 Expert labs remaining)
* Completed all 13 Access Control labs
* Completed privilege escalation fundamentals through access control testing
* Completed IDOR and object-level authorization testing
* Completed workflow authorization testing
* Completed header-based access control bypass labs

## Key Takeaways

* Access control must protect files, documents, and resources, not only user accounts.
* Resource ownership must always be validated on the server side.
* Every step of a sensitive workflow requires authorization checks.
* Confirmation requests are security-sensitive endpoints.
* Client-controlled headers should never be trusted for authorization decisions.
* Access control should always be based on authenticated user permissions.
* IDOR vulnerabilities can affect many different resource types.
* Authorization logic must be consistently enforced across all application functionality.

## Milestone Achieved

* Completed Access Control Vulnerabilities Learning Path (13/13 Labs)

Topics Mastered:

* Vertical Privilege Escalation
* Horizontal Privilege Escalation
* IDOR Vulnerabilities
* User Role Manipulation
* Resource Ownership Validation
* URL-Based Access Control Bypass
* HTTP Method-Based Access Control Bypass
* Referer-Based Access Control Bypass
* Multi-Step Workflow Authorization Flaws
* Information Disclosure Through Broken Authorization

## Next Step

Continue the SQL Injection learning path or begin Cross-Site Scripting (XSS) to further strengthen web application security testing skills.
