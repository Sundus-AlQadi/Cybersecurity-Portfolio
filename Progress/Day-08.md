# Day 08 Progress - Access Control Fundamentals and Privilege Escalation

## Completed Labs

* Access Control Lab 01: Unprotected Admin Functionality
* Access Control Lab 02: Unprotected Admin Functionality with Unpredictable URL
* Access Control Lab 03: User Role Controlled by Request Parameter
* Access Control Lab 04: User Role Can Be Modified in User Profile

## Topics Covered

* Access control fundamentals
* Authorization vs authentication
* Unprotected administrative functionality
* Security by obscurity
* Hidden administrative interfaces
* Client-side role manipulation
* Privilege escalation
* User-controlled authorization data
* Cookie-based role manipulation
* Role-based access control (RBAC)
* User profile modification vulnerabilities
* Sensitive parameter manipulation
* Mass assignment vulnerabilities
* Server-side authorization checks

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Web browser
* GitHub

## Reflection

Today I started the Access Control section in PortSwigger Web Security Academy and completed four introductory labs focused on authorization weaknesses and privilege escalation.

In the first lab, I learned that administrative functionality must always be protected by server-side authorization checks. Simply exposing an administrative page without proper access restrictions allows unauthorized users to perform privileged actions.

In the second lab, I learned that hiding an administrative interface behind an unpredictable URL does not provide real security. The administrative endpoint was disclosed within client-side JavaScript, demonstrating that security should never rely on obscurity alone.

In the third lab, I explored a privilege escalation vulnerability where administrative privileges were controlled using client-side data. By modifying a role-related value, I gained access to administrative functionality and performed privileged actions that should have been restricted.

In the fourth lab, I learned how profile update functionality can become dangerous when sensitive attributes are accepted from user-controlled requests. By modifying role-related data within a profile update request, it was possible to escalate privileges and gain administrator access.

These labs reinforced a critical access control principle: authorization decisions must always be enforced on the server side, and applications should never trust user-controlled data when determining permissions or privileges.

## Current Progress

* Completed Access Control Labs 01–04
* Completed privilege escalation fundamentals
* Completed administrative functionality exposure labs

## Key Takeaways

* Authentication and authorization are different security concepts.
* Hidden URLs are not a security control.
* Administrative functionality must always be protected by server-side checks.
* User roles should never be controlled by client-side data.
* Cookies, request parameters, and profile fields should not determine authorization decisions.
* Sensitive attributes such as roles and permissions must not be modifiable by regular users.
* Security by obscurity cannot replace proper access control.
* Privilege escalation often occurs when applications trust client-controlled data.

## Next Step

Continue the Access Control section, focusing on horizontal privilege escalation, vertical privilege escalation, IDOR vulnerabilities, and role-based access control weaknesses.
