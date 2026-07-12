# Day 16 Progress - Reflected DOM XSS, Stored DOM XSS, and WAF Bypass

## Completed Labs

### Cross-Site Scripting

* XSS Lab 12: Reflected DOM XSS
* XSS Lab 13: Stored DOM XSS
* XSS Lab 14: Reflected XSS into HTML Context with Most Tags and Attributes Blocked

## Topics Covered

* Cross-Site Scripting
* Reflected DOM XSS
* Stored DOM XSS
* Reflected XSS
* JSON response reflection
* Unsafe client-side JavaScript processing
* `eval()` as a dangerous sink
* Backslash escaping issues
* Breaking out of JSON strings
* JavaScript syntax manipulation
* Stored comment-based XSS
* Client-side filtering bypass
* JavaScript `replace()` weakness
* Angle bracket encoding bypass
* Event handler execution
* Image `onerror` payloads
* WAF filtering behavior
* WAF bypass testing
* Burp Intruder payload testing
* HTML tag fuzzing
* Event attribute fuzzing
* XSS Cheat Sheet usage
* `body` tag injection
* `onresize` event handler
* iframe-based exploit delivery
* No-user-interaction XSS execution
* Exploit Server usage

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Burp Intruder
* Burp Target Site Map
* XSS Cheat Sheet
* Exploit Server
* Web Browser
* Browser Developer Tools
* GitHub

## Reflection

Today I continued the Cross-Site Scripting learning path and completed three practitioner-level labs focused on reflected DOM XSS, stored DOM XSS, and WAF bypass techniques.

In the first lab, I learned how reflected DOM XSS works. The search input was sent to the server, and the server reflected it inside a JSON response. Then, client-side JavaScript processed that response unsafely using the dangerous `eval()` function. By using the payload `\"-alert(1)}//`, I was able to break out of the JSON string and execute `alert(1)`. This helped me understand that XSS does not always appear directly in the HTML response. Sometimes the server reflects data in a JSON response, and the real danger happens when JavaScript processes that response unsafely.

In the second lab, I practiced stored DOM XSS in the blog comment functionality. The payload was saved as a comment, but the vulnerability happened when client-side JavaScript later processed and inserted the stored comment into the DOM. The website attempted to prevent XSS using JavaScript `replace()`, but it only replaced the first occurrence of angle brackets. By using the payload `<><img src=1 onerror=alert(1)>`, the first pair of angle brackets was encoded, while the real image payload remained active. This showed me that weak client-side filtering can be bypassed and should not be trusted as a complete defense.

In the third lab, I worked on reflected XSS with most tags and attributes blocked by a WAF. A normal payload such as `<img src=1 onerror=print()>` was blocked. I used Burp Intruder to test which HTML tags and event attributes were still allowed. The testing showed that the `body` tag and the `onresize` event were allowed. I then used an iframe through the Exploit Server to automatically trigger the resize event and call `print()` without requiring user interaction. This helped me understand that WAFs may block common payloads, but they are not a complete protection against XSS.

These labs strengthened my understanding of advanced XSS contexts. I learned that XSS can happen through JSON responses, unsafe JavaScript functions, stored DOM manipulation, weak filtering, and incomplete WAF rules. The most important skill is identifying where the input goes, how it is processed, and what sink eventually handles it.

## Current Progress

* Completed Authentication Vulnerabilities labs and documentation
* Completed Access Control Vulnerabilities labs and documentation
* Completed SQL Injection labs and documentation
* Continued Cross-Site Scripting learning path
* Completed first 14 XSS labs
* Documented reflected XSS, stored XSS, DOM XSS, jQuery sinks, attribute injection, JavaScript string injection, AngularJS expression injection, reflected DOM XSS, stored DOM XSS, and WAF bypass
* Continued daily progress documentation
* Continued updating XSS notes with comparison tables and key takeaways

## Key Takeaways

* Reflected DOM XSS happens when the server reflects input and client-side JavaScript processes it unsafely.
* JSON responses can still lead to XSS if they are handled using dangerous functions such as `eval()`.
* `eval()` is dangerous because it executes strings as JavaScript code.
* Escaping quotation marks is not enough if backslashes are not handled correctly.
* Stored DOM XSS happens when stored input is later written into the DOM unsafely by JavaScript.
* JavaScript `replace()` only replaces the first occurrence when used with a normal string argument.
* Weak client-side filtering can often be bypassed.
* The `<img>` tag with an invalid `src` can execute JavaScript through the `onerror` event.
* WAFs can block common XSS payloads but may still allow other tags or event handlers.
* Burp Intruder can be used to discover allowed tags and attributes.
* The `body` tag and `onresize` event can be used to trigger JavaScript in certain contexts.
* iframe-based exploits can trigger events automatically without user interaction.
* XSS testing requires identifying the source, sink, context, and any filtering behavior.

## Milestone Achieved

Completed additional practitioner-level XSS labs covering:

* Reflected DOM XSS
* JSON response-based XSS
* Unsafe `eval()` processing
* Stored DOM XSS
* Client-side filtering bypass
* JavaScript `replace()` weakness
* WAF bypass testing
* Tag and attribute fuzzing with Burp Intruder
* No-user-interaction XSS delivery using an iframe
* Event-based JavaScript execution using `onresize`

## Topics Strengthened

* Reflected DOM XSS
* Stored DOM XSS
* Reflected XSS
* JSON and JavaScript contexts
* Dangerous JavaScript sinks
* DOM manipulation risks
* Client-side filtering weaknesses
* WAF bypass methodology
* Burp Intruder testing
* Exploit Server delivery
* Event handler payloads
* Context-aware XSS testing

## Next Step

Continue the Cross-Site Scripting learning path, focusing on advanced WAF bypasses, JavaScript event handling, custom tag and attribute testing, CSP bypass concepts, and more complex XSS contexts that require precise payload construction.
