# Day 18 Progress - XSS Impact, Event Handler Contexts, and CSRF Bypass

## Completed Labs

### Cross-Site Scripting

* XSS Lab 20: Stored XSS into `onclick` Event with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes and Backslash Escaped
* XSS Lab 21: Reflected XSS into a Template Literal with Multiple Characters Escaped
* XSS Lab 24: Exploiting XSS to Bypass CSRF Defenses

## Pending Labs

* XSS Lab 22: Exploiting XSS to Steal Cookies
* XSS Lab 23: Exploiting XSS to Capture Passwords

Labs 22 and 23 were reviewed and understood conceptually, but they were marked as pending because the intended solutions require Burp Collaborator / Burp Suite Professional to receive external interactions.

## Topics Covered

* Cross-Site Scripting
* Stored XSS
* Reflected XSS
* JavaScript event handler context
* `onclick` injection
* HTML entity bypass using `&apos;`
* Encoding vs escaping
* JavaScript template literals
* Template literal expression injection
* `${}` expression execution
* XSS impact beyond `alert(1)`
* Cookie theft concept
* Password capture concept
* CSRF token theft
* CSRF protection bypass
* Authenticated action execution through XSS

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Repeater
* Web Browser
* Browser Developer Tools
* GitHub

## Reflection

Today I continued the Cross-Site Scripting learning path and focused on more realistic XSS impact scenarios.

In Lab 20, I learned how stored XSS can happen inside an `onclick` event handler. The application encoded or escaped several characters, but the payload used `&apos;` to introduce a single quote after browser parsing. This allowed the payload to break out of the JavaScript context and execute when the comment author name was clicked.

In Lab 21, I worked with reflected XSS inside a JavaScript template literal. Instead of breaking out of the string, the payload used `${alert(1)}` because template literals evaluate expressions inside `${}`. This helped me understand that template literal contexts have their own XSS technique.

Labs 22 and 23 were reviewed conceptually but marked as pending because they require Burp Collaborator. These labs show how XSS can be used to steal cookies or capture passwords, which demonstrates real impact beyond simple alert payloads.

In Lab 24, I completed the XSS to CSRF bypass lab. The stored XSS payload loaded the victim's account page, extracted the CSRF token, and sent a valid request to change the victim's email address. The lab was solved immediately after posting the comment because the simulated victim automatically viewed the comment.

## Current Progress

* Completed Authentication Vulnerabilities labs and documentation
* Completed Access Control Vulnerabilities labs and documentation
* Completed SQL Injection labs and documentation
* Completed core Cross-Site Scripting Practitioner labs
* Completed first 21 XSS labs
* Completed XSS Lab 24: XSS to bypass CSRF defenses
* Marked Labs 22 and 23 as pending due to Burp Collaborator requirements
* Deferred Expert-level XSS labs for future study
* Continued updating XSS notes and README documentation

## Key Takeaways

* XSS can appear inside JavaScript event handlers, not only inside HTML content.
* Encoding and escaping are different, and both must be handled correctly.
* HTML entities such as `&apos;` can affect how payloads are interpreted by the browser.
* Template literals can execute JavaScript expressions using `${}`.
* XSS can bypass CSRF protections by reading tokens from the victim's session.
* Cookie theft and password capture are serious XSS impacts, but the related labs require Burp Collaborator.
* XSS is more dangerous when it can perform authenticated actions as another user.

## Milestone Achieved

Completed the main Practitioner-level XSS path, with two tool-limited labs clearly marked as pending.

The XSS section now covers:

* Reflected XSS
* Stored XSS
* DOM XSS
* JavaScript string contexts
* Template literals
* AngularJS expression injection
* SVG-based XSS
* WAF bypass
* Event handler injection
* CSRF bypass using XSS
* Real-world XSS impact concepts

## Next Step

Move to another PortSwigger vulnerability category to broaden practical coverage, while leaving Expert-level XSS labs and Burp Collaborator-based labs for future study.
