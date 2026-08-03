# Cross-Site Request Forgery Vulnerabilities

This folder contains my documentation for the Cross-Site Request Forgery labs completed on PortSwigger Web Security Academy.

The focus of this section was understanding how CSRF attacks work, how weak CSRF defenses can be bypassed, and how browser protections such as SameSite cookies and Referer validation can fail when implemented incorrectly.

## Platform

PortSwigger Web Security Academy

## Main Topic

Cross-Site Request Forgery

## Status

Completed the available CSRF labs using Burp Suite Community Edition.

One lab is pending because it requires Burp Collaborator / Burp Suite Professional.

## What is CSRF?

Cross-Site Request Forgery is a vulnerability where an attacker tricks a logged-in user's browser into sending an unwanted request to a trusted website.

The attack works because the browser automatically includes the user's session cookies with the request.

## Completed Labs

* Lab 01: CSRF Vulnerability with No Defenses
* Lab 02: CSRF Where Token Validation Depends on Request Method
* Lab 03: CSRF Where Token Validation Depends on Token Being Present
* Lab 04: CSRF Where Token Is Not Tied to User Session
* Lab 05: CSRF Where Token Is Tied to Non-Session Cookie
* Lab 06: CSRF Where Token Is Duplicated in Cookie
* Lab 07: SameSite Lax Bypass via Method Override
* Lab 08: SameSite Strict Bypass via Client-Side Redirect
* Lab 10: SameSite Lax Bypass via Cookie Refresh
* Lab 11: CSRF Where Referer Validation Depends on Header Being Present
* Lab 12: CSRF with Broken Referer Validation

## Pending / Tool-Limited Lab

* Lab 09: SameSite Strict Bypass via Sibling Domain
  Status: Pending - Requires Burp Collaborator / Burp Suite Professional

## Topics Covered

* CSRF fundamentals
* Auto-submitting HTML forms
* Hidden form fields
* CSRF token validation
* Missing CSRF tokens
* Token validation based on request method
* Token validation based on token presence
* Token not tied to user session
* Token tied to non-session cookie
* Double submit cookie weakness
* Cookie injection using `Set-Cookie`
* SameSite Lax behavior
* SameSite Strict behavior
* Method override using `_method=POST`
* Top-level GET navigation
* Client-side redirect gadgets
* OAuth login flow
* Cookie refresh behavior
* Referer header validation
* Missing Referer bypass
* Broken Referer validation
* Referrer-Policy behavior
* Burp Repeater request testing
* Exploit Server payload hosting

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Exploit Server
* Web Browser
* GitHub

## Key Concepts Learned

### CSRF Tokens

CSRF tokens should be unpredictable, required, and tied to the user's session.

### SameSite Cookies

SameSite cookies help reduce CSRF risk, but they should not replace proper CSRF tokens.

### OAuth Login Flow

OAuth login can refresh a user's session cookie, which may affect SameSite behavior.

### Referer Validation

Referer validation is weak if the application accepts missing headers or checks the domain using simple string matching.

## What I Learned

I learned how CSRF vulnerabilities happen when applications trust browser cookies without properly validating user intent.

I also learned how weak defenses such as incomplete CSRF token validation, SameSite-only protection, and Referer-based checks can be bypassed.

## Mitigation Summary

To prevent CSRF vulnerabilities:

* Use strong CSRF tokens for all state-changing requests.
* Reject missing, invalid, or reused tokens.
* Do not allow sensitive actions through GET requests.
* Avoid unsafe method override behavior.
* Do not rely only on the Referer header.
* Validate Origin or Referer headers strictly when used.
* Avoid client-side redirects based on untrusted input.
* Prevent attackers from setting trusted CSRF-related cookies.
