# Day 20 Progress - CSRF Completion and SameSite / Referer Bypass Labs

## Completed Labs

### Cross-Site Request Forgery

* CSRF Lab 08: SameSite Strict Bypass via Client-Side Redirect
* CSRF Lab 10: SameSite Lax Bypass via Cookie Refresh
* CSRF Lab 11: CSRF Where Referer Validation Depends on Header Being Present
* CSRF Lab 12: CSRF with Broken Referer Validation

## Reviewed Lab

* CSRF Lab 09: SameSite Strict Bypass via Sibling Domain
  Status: Pending - Requires Burp Collaborator / Burp Suite Professional

## Topics Covered

* SameSite Strict cookie behavior
* SameSite Lax cookie behavior
* Client-side redirect gadgets
* OAuth login flow
* Cookie refresh behavior
* Lax + POST behavior
* Popup blocker bypass using user click
* Referer header validation
* Missing Referer bypass
* Broken Referer validation
* Referrer-Policy header
* CSRF exploit construction using Exploit Server

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Exploit Server
* Web Browser
* GitHub

## Reflection

Today I completed the remaining CSRF labs that did not require Burp Collaborator.

I learned how SameSite cookie protections can reduce CSRF risk but cannot replace proper CSRF tokens. I practiced bypassing SameSite Strict using a client-side redirect gadget and bypassing SameSite Lax by refreshing the session cookie through an OAuth login flow.

I also learned how weak Referer-based CSRF defenses can be bypassed. One lab accepted requests when the Referer header was missing, while another accepted requests when the trusted domain appeared anywhere inside the Referer value.

## Current Progress

* Completed the CSRF learning path labs available with Burp Community Edition
* Documented all solved CSRF labs
* Marked the Collaborator-based sibling domain lab as pending

## Key Takeaways

* CSRF tokens should be used for all sensitive state-changing actions.
* SameSite cookies are helpful but not a complete CSRF defense.
* GET requests should not perform sensitive actions.
* Method override can introduce CSRF risk.
* Referer validation is weak if used alone.
* Missing or broken Referer validation can be bypassed.
* OAuth login flows can refresh cookies and affect SameSite behavior.
