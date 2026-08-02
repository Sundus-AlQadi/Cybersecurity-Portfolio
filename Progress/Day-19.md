# Day 19 Progress - CSRF Token Weaknesses and SameSite Lax Bypass

## Completed Labs

### Cross-Site Request Forgery

* CSRF Lab 05: CSRF Where Token Is Tied to Non-Session Cookie
* CSRF Lab 06: CSRF Where Token Is Duplicated in Cookie
* CSRF Lab 07: SameSite Lax Bypass via Method Override

## Topics Covered

* Cross-Site Request Forgery
* CSRF token validation
* CSRF token and cookie binding
* Non-session cookie weakness
* Double submit cookie weakness
* Cookie injection using `Set-Cookie`
* SameSite Lax behavior
* Method override using `_method=POST`
* Top-level GET navigation
* Auto-submitting CSRF exploits
* Exploit Server usage

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Exploit Server
* Web Browser
* GitHub

## Reflection

Today I continued the CSRF learning path and completed three practitioner-level labs.

In the first lab, I learned that CSRF tokens should not be tied only to a separate cookie such as `csrfKey`. The exploit worked by planting the matching `csrfKey` cookie in the victim's browser and submitting the matching CSRF token in the form.

In the second lab, I learned about the weakness of the double submit cookie technique. The application only checked whether the `csrf` cookie matched the `csrf` form value. By setting both values to `fake`, the request was accepted.

In the third lab, I learned how SameSite Lax can be bypassed using top-level GET navigation and method override. The exploit used `document.location` to send a GET request with `_method=POST`, allowing the email change request to succeed.

## Current Progress

* Started the CSRF learning path
* Completed CSRF Labs 01–07
* Documented CSRF token validation weaknesses
* Practiced CSRF exploits using hidden forms and top-level navigation
* Continued updating CSRF notes and daily progress

## Key Takeaways

* CSRF tokens must be tied to the user's session.
* A token should not only be tied to a separate cookie.
* Double submit cookie protection is weak if the attacker can control the cookie.
* SameSite Lax is helpful, but it is not a full CSRF defense.
* State-changing actions should not be allowed through GET requests.
* Method override can create CSRF risk if not handled carefully.

## Next Step

Continue the CSRF learning path and focus on remaining labs involving SameSite bypasses, Referer validation weaknesses, and token validation flaws.
