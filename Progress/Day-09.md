# Day 09 Progress - Access Control Bypass and IDOR Vulnerabilities

## Completed Labs

* Access Control Lab 05: User ID Controlled by Request Parameter
* Access Control Lab 06: URL-Based Access Control Can Be Circumvented
* Access Control Lab 07: Method-Based Access Control Can Be Circumvented
* Access Control Lab 08: User ID Controlled by Request Parameter with Unpredictable User IDs
* Access Control Lab 09: User ID Controlled by Request Parameter with Data Leakage in Redirect
* Access Control Lab 10: User ID Controlled by Request Parameter with Password Disclosure

## Topics Covered

* Access control fundamentals
* Horizontal privilege escalation
* Insecure Direct Object References (IDOR)
* User-controlled identifiers
* Resource ownership validation
* GUID-based identifiers
* Information disclosure vulnerabilities
* Data leakage in redirect responses
* Password disclosure vulnerabilities
* URL-based access control bypass
* Front-end and back-end access control mismatches
* X-Original-URL header abuse
* HTTP method manipulation
* Method-based authorization weaknesses
* Administrative privilege escalation

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Burp Proxy
* Web Browser
* GitHub

## Reflection

Today I continued the Access Control section and completed six labs focused on authorization weaknesses, horizontal privilege escalation, and information disclosure.

In several labs, I explored Insecure Direct Object Reference (IDOR) vulnerabilities where user-controlled identifiers were used to access resources belonging to other users. I learned that changing usernames, account identifiers, or GUID values can expose sensitive information when proper authorization checks are missing.

I also learned that using unpredictable identifiers such as GUIDs does not replace access control. Even though identifiers may be difficult to guess, they can often be discovered through publicly accessible content or application functionality.

Another important lesson was that redirecting users away from unauthorized resources does not provide security if sensitive information is still included in the response body. Using Burp Repeater allowed me to inspect redirect responses and identify leaked data that would normally be hidden by the browser.

I explored a URL-based access control bypass where a front-end system attempted to block access to administrative functionality while the back-end application trusted the X-Original-URL header. This demonstrated how differences in request interpretation between application layers can lead to authorization bypasses.

Finally, I examined method-based access control weaknesses and learned that authorization decisions should not depend on HTTP methods such as GET or POST. Administrative functionality must remain protected regardless of how a request is submitted.

These labs reinforced the importance of server-side authorization checks, proper resource ownership validation, secure handling of sensitive data, and consistent security enforcement across application components.

## Current Progress

* Completed SQL Injection fundamentals and database enumeration labs
* Completed Authentication Vulnerabilities learning path
* Completed Access Control Labs 01–10
* Completed privilege escalation fundamentals
* Completed IDOR fundamentals
* Completed information disclosure labs
* Completed URL-based access control bypass labs
* Completed method-based access control bypass labs

## Key Takeaways

* Authentication does not guarantee authorization.
* Users should only access resources they own.
* IDOR vulnerabilities occur when applications trust user-controlled identifiers.
* GUIDs improve unpredictability but do not replace authorization checks.
* Redirects are not access control mechanisms.
* Sensitive information should never appear in unauthorized responses.
* Passwords should never be recoverable or displayed by applications.
* Access control should not depend on HTTP methods.
* Front-end filtering should never be the only security control.
* Authorization must always be enforced on the server side.

## Next Step

Continue the Access Control section focusing on advanced horizontal privilege escalation, vertical privilege escalation, role-based access control weaknesses, and multi-step authorization bypass techniques.
