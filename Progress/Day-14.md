# Day 14 Progress - DOM XSS and Attribute Context Injection

## Completed Labs

### Cross-Site Scripting

* XSS Lab 05: DOM XSS in jQuery Anchor `href` Attribute Sink Using `location.search`
* XSS Lab 06: DOM XSS in jQuery Selector Sink Using a Hashchange Event
* XSS Lab 07: Reflected XSS into Attribute with Angle Brackets HTML-Encoded

## Topics Covered

* Cross-Site Scripting
* DOM-based XSS
* Reflected XSS
* `location.search` as a DOM XSS source
* `location.hash` as a DOM XSS source
* jQuery sinks
* jQuery `.attr()` sink
* jQuery `$()` selector sink
* Anchor `href` attribute injection
* JavaScript URL payloads
* URL encoding
* Hashchange event exploitation
* Exploit server usage
* iframe-based exploit delivery
* Event handler injection
* HTML attribute context
* Attribute breakout techniques
* Angle bracket HTML encoding
* Context-aware XSS payload construction
* Browser Developer Tools inspection

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Web Browser
* Browser Developer Tools
* Exploit Server
* GitHub

## Reflection

Today I continued the Cross-Site Scripting learning path and completed three labs focused on DOM XSS, jQuery sinks, and attribute-based reflected XSS.

In the first lab, I learned how DOM XSS can happen when client-side JavaScript reads data from `location.search` and places it into an anchor tag's `href` attribute using jQuery. The vulnerable page took the `returnPath` parameter from the URL and assigned it to the Back link. By setting the value to a JavaScript URL, I was able to execute `alert(document.cookie)` when the Back link was clicked. This helped me understand that attributes such as `href` can become dangerous when they accept untrusted input without validation.

In the second lab, I practiced DOM XSS using `location.hash` and a jQuery selector sink. The application used the hash value from the URL to auto-scroll to a blog post. I learned that a jQuery selector is normally used to select existing elements, but if unsafe user-controlled input is passed into it, HTML can be interpreted in a dangerous way. I used an iframe exploit that changed the hash after the page loaded, triggering the `hashchange` event and executing the `print()` function through an image `onerror` handler.

In the third lab, I worked on reflected XSS in an HTML attribute context. Angle brackets were HTML-encoded, so a normal `<script>` payload did not work. Instead, I learned how to break out of a quoted attribute and inject a new event handler attribute using `onmouseover`. This showed me that XSS prevention must be context-aware because encoding only `<` and `>` is not enough when user input is placed inside attributes.

These labs helped me understand that XSS payloads depend heavily on where the input appears in the page. The correct payload changes depending on whether the input is placed inside HTML body content, an HTML attribute, a URL attribute, or a JavaScript-controlled DOM sink.

## Current Progress

* Completed Authentication Vulnerabilities labs and documentation
* Completed Access Control Vulnerabilities labs and documentation
* Completed SQL Injection labs and documentation
* Started Cross-Site Scripting learning path
* Completed first 7 XSS labs
* Documented reflected XSS, stored XSS, DOM XSS, jQuery sink issues, and attribute context injection
* Continued daily progress documentation
* Continued updating XSS notes

## Key Takeaways

* DOM XSS occurs when unsafe client-side JavaScript handles user-controlled data.
* `location.search` and `location.hash` can both be dangerous sources.
* jQuery sinks such as `.attr()` and `$()` can become dangerous when used with untrusted input.
* The `href` attribute can execute JavaScript if it is assigned a `javascript:` URL.
* URL encoding may be needed when placing payloads inside query parameters.
* The `hashchange` event can trigger vulnerable JavaScript when the URL hash changes.
* Exploit servers can be used to deliver XSS payloads to a victim.
* iframes can be used to load vulnerable pages and trigger browser events automatically.
* Encoding angle brackets is not always enough to prevent XSS.
* If input is reflected inside an HTML attribute, quotation marks must also be handled safely.
* Event handlers such as `onmouseover`, `onload`, and `onerror` can execute JavaScript.
* XSS testing requires understanding the exact context where input is reflected.

## Milestone Achieved

Completed additional beginner XSS labs covering:

* DOM XSS with jQuery `href` attribute sink
* DOM XSS with jQuery selector sink
* DOM XSS using `location.hash`
* XSS delivery using an exploit server
* Reflected XSS in HTML attribute context
* Attribute breakout and event handler injection

## Topics Strengthened

* DOM-based XSS
* Reflected XSS
* JavaScript sources and sinks
* jQuery security risks
* URL and hash-based payload delivery
* Attribute context injection
* Event handler payloads
* Browser DOM inspection
* Context-aware XSS testing

## Next Step

Continue the Cross-Site Scripting learning path, focusing on additional XSS contexts such as JavaScript string injection, attribute injection with different encodings, template literal contexts, and XSS filter bypass techniques.
