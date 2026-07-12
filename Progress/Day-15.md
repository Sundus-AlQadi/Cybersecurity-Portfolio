# Day 15 Progress - Advanced XSS Contexts and AngularJS Expression Injection

## Completed Labs

### Cross-Site Scripting

* XSS Lab 08: Stored XSS into Anchor `href` Attribute with Double Quotes HTML-Encoded
* XSS Lab 09: Reflected XSS into a JavaScript String with Angle Brackets HTML-Encoded
* XSS Lab 10: DOM XSS in `document.write` Sink Using Source `location.search` Inside a `select` Element
* XSS Lab 11: DOM XSS in AngularJS Expression with Angle Brackets and Double Quotes HTML-Encoded

## Topics Covered

* Cross-Site Scripting
* Stored XSS
* Reflected XSS
* DOM-based XSS
* Anchor `href` attribute injection
* JavaScript URL payloads
* Dangerous URL schemes
* HTML attribute context
* Double quote HTML encoding
* JavaScript string context
* Breaking out of JavaScript strings
* Angle bracket HTML encoding
* Context-aware XSS payload construction
* `document.write()` as a dangerous sink
* `location.search` as a DOM XSS source
* XSS inside `select` elements
* Breaking out of HTML structures
* Image `onerror` event handler execution
* AngularJS expression injection
* AngularJS `ng-app` directive
* AngularJS double curly brace expressions
* Browser Developer Tools inspection
* Burp Repeater request testing

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Web Browser
* Browser Developer Tools
* GitHub

## Reflection

Today I continued the Cross-Site Scripting learning path and completed four labs focused on stored XSS, JavaScript string injection, DOM XSS inside HTML structures, and AngularJS expression injection.

In the first lab, I learned how stored XSS can occur through an anchor tag's `href` attribute. The vulnerable input was the Website field in the blog comment form. Although double quotes were HTML-encoded, the application still allowed dangerous URL schemes such as `javascript:`. By entering `javascript:alert(1)` as the website value, the payload was stored and executed when the comment author's name was clicked. This helped me understand that encoding quotes is not enough if URL values are not properly validated.

In the second lab, I practiced reflected XSS inside a JavaScript string context. The search input was reflected inside JavaScript code, while angle brackets were HTML-encoded. Because a normal `<script>` payload would not work, I used the payload `'-alert(1)-'` to break out of the JavaScript string and execute JavaScript directly. This showed me that XSS payloads must be adapted based on where the input appears in the page.

In the third lab, I worked on DOM XSS inside a `select` element. The application read the `storeId` value from `location.search` and used `document.write()` to place it inside the stock checker dropdown. Since the payload was inserted inside a `select` element, I had to break out of the dropdown first using `</select>`, then inject an image element with an invalid source and an `onerror` event handler. This helped me understand that XSS exploitation depends heavily on the surrounding HTML structure.

In the fourth lab, I learned about AngularJS expression injection. The application used AngularJS with the `ng-app` directive, and the input was processed inside an AngularJS-controlled area. Since angle brackets and double quotes were encoded, normal HTML injection was not the correct technique. Instead, I used an AngularJS expression with double curly braces to execute JavaScript and trigger `alert(1)`. This showed me that some frameworks can introduce unique XSS contexts if user input is evaluated as an expression.

These labs strengthened my understanding that XSS is not about memorizing one payload. The correct payload depends on the exact context: HTML body, HTML attribute, URL attribute, JavaScript string, DOM sink, HTML structure, or framework expression.

## Current Progress

* Completed Authentication Vulnerabilities labs and documentation
* Completed Access Control Vulnerabilities labs and documentation
* Completed SQL Injection labs and documentation
* Started Cross-Site Scripting learning path
* Completed first 11 XSS labs
* Documented reflected XSS, stored XSS, DOM XSS, jQuery sink issues, attribute context injection, JavaScript string injection, select element breakout, and AngularJS expression injection
* Continued daily progress documentation
* Continued updating XSS notes

## Key Takeaways

* Stored XSS can occur through fields that are later inserted into attributes, such as Website links.
* The `href` attribute can execute JavaScript if dangerous schemes like `javascript:` are allowed.
* Encoding double quotes helps prevent attribute breakout, but it does not prevent unsafe URL schemes.
* Reflected XSS inside JavaScript strings requires breaking out of the string context.
* Encoding `<` and `>` is not enough when quotes or JavaScript string characters are not safely escaped.
* DOM XSS can occur when `location.search` is passed into dangerous sinks such as `document.write()`.
* Payloads inside a `select` element may need to break out of the `select` structure before executable HTML can be injected.
* Event handlers such as `onerror` can execute JavaScript when browser events occur.
* AngularJS can evaluate expressions inside `{{ }}` when the page uses `ng-app`.
* AngularJS expression injection can execute JavaScript without using `<script>` tags.
* XSS testing requires identifying the exact source, sink, and execution context.

## Milestone Achieved

Completed additional XSS labs covering:

* Stored XSS through anchor `href` attributes
* Dangerous JavaScript URL schemes
* Reflected XSS inside JavaScript strings
* DOM XSS using `document.write()`
* DOM XSS inside `select` elements
* HTML structure breakout
* AngularJS expression injection
* Context-aware payload construction

## Topics Strengthened

* Stored XSS
* Reflected XSS
* DOM-based XSS
* JavaScript string context
* HTML attribute context
* URL attribute security
* DOM sources and sinks
* Event handler payloads
* AngularJS security risks
* Context-aware XSS testing
* Browser DOM inspection
* Burp Repeater request testing

## Next Step

Continue the Cross-Site Scripting learning path, focusing on more advanced XSS contexts, filter bypass techniques, JavaScript template contexts, AngularJS sandbox-related payloads, and stronger mitigation strategies such as context-aware encoding, sanitization, and Content Security Policy.
