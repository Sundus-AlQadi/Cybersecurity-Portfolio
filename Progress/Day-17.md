# Day 17 Progress - XSS Filter Bypass and JavaScript String Escaping

## Completed Labs

### Cross-Site Scripting

* XSS Lab 15: Reflected XSS into HTML Context with All Tags Blocked Except Custom Ones
* XSS Lab 16: Reflected XSS with Some SVG Markup Allowed
* XSS Lab 17: Reflected XSS in Canonical Link Tag
* XSS Lab 18: Reflected XSS into a JavaScript String with Single Quote and Backslash Escaped
* XSS Lab 19: Reflected XSS into a JavaScript String with Angle Brackets and Double Quotes HTML-Encoded and Single Quotes Escaped

## Topics Covered

* Cross-Site Scripting
* Reflected XSS
* WAF bypass
* Custom tag injection
* Event handler injection
* `onfocus` event
* `tabindex` attribute
* URL fragment targeting
* `document.cookie`
* SVG-based XSS
* SVG animation tags
* `animatetransform` tag
* `onbegin` event
* Canonical link tag injection
* HTML attribute injection
* `accesskey` attribute
* `onclick` event handler
* JavaScript string context
* Script block breakout
* Escaping bypass
* Single quote escaping
* Backslash escaping weakness
* URL encoding
* Context-aware payload construction

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Burp Intruder
* XSS Cheat Sheet
* Exploit Server
* Chrome Browser
* Browser Developer Tools
* GitHub

## Reflection

Today I continued the Cross-Site Scripting learning path and completed five practitioner-level labs focused on filter bypasses, custom tags, SVG markup, canonical link tag injection, and JavaScript string escaping.

In the first lab, I learned how XSS can still be possible even when all standard HTML tags are blocked. The application allowed custom tags, so I used a custom `<xss>` element with `onfocus`, `tabindex`, and a URL fragment to trigger `alert(document.cookie)` automatically. This helped me understand how browser behavior can be used to trigger an event without direct user interaction.

In the second lab, I practiced bypassing filters using SVG markup. A normal payload such as `<img src=1 onerror=alert(1)>` was blocked, but Burp Intruder helped identify that some SVG tags and events were allowed. By combining `<svg>`, `<animatetransform>`, and `onbegin`, I was able to execute `alert(1)`. This showed me that filters must handle SVG-specific tags and events, not only common HTML tags.

In the third lab, I worked with XSS inside a canonical link tag. Since angle brackets were escaped, I could not inject a new HTML tag. Instead, I injected attributes into the existing canonical link tag using `accesskey` and `onclick`. This showed me that XSS can occur inside hidden or non-visible HTML tags if user input is reflected inside an attribute.

In the fourth lab, I learned how to break out of a JavaScript string when single quotes and backslashes were escaped. Since a normal quote breakout did not work, I used `</script><script>alert(1)</script>` to close the existing script block and inject a new one. This demonstrated that escaping quotes is not enough if angle brackets are still allowed inside a script block.

In the fifth lab, I practiced a more restricted JavaScript string context. Angle brackets and double quotes were HTML-encoded, and single quotes were escaped, but backslashes were not escaped. By using the payload `\'-alert(1)//`, I was able to bypass the escaping and execute JavaScript. This helped me understand how one unescaped character can break the whole protection logic.

Overall, these labs showed me that XSS testing requires understanding the exact context, the filtering behavior, and how the browser interprets the final output.

## Current Progress

* Completed Authentication Vulnerabilities labs and documentation
* Completed Access Control Vulnerabilities labs and documentation
* Completed SQL Injection labs and documentation
* Continued Cross-Site Scripting learning path
* Completed first 19 XSS labs
* Documented reflected XSS, stored XSS, DOM XSS, AngularJS expression injection, WAF bypass, SVG-based XSS, canonical link tag injection, and JavaScript string escaping bypasses
* Continued updating XSS notes with payload explanations and comparison tables
* Continued daily progress documentation

## Key Takeaways

* Blocking common HTML tags does not fully prevent XSS.
* Custom tags can be dangerous if event handlers are still allowed.
* `tabindex` can make custom elements focusable.
* URL fragments such as `#x` can target elements by ID.
* SVG markup can introduce XSS through animation tags and events.
* Burp Intruder is useful for identifying allowed tags and attributes.
* XSS can occur inside hidden tags such as canonical link tags.
* Attribute injection can work even when angle brackets are escaped.
* JavaScript string XSS depends heavily on how quotes and backslashes are escaped.
* `</script>` can break out of a script block if angle brackets are not encoded.
* Escaping single quotes is not enough if backslashes are not handled correctly.
* URL encoding is important when delivering XSS payloads through query parameters.

## Milestone Achieved

Completed advanced reflected XSS labs covering:

* Custom tag injection
* Automatic event triggering with `onfocus`
* SVG-based XSS
* SVG event handler execution
* Canonical link tag attribute injection
* JavaScript string breakout
* Script block breakout
* Backslash escaping bypass
* WAF and filter bypass testing

## Topics Strengthened

* Reflected XSS
* WAF bypass methodology
* HTML attribute context
* SVG markup security
* Event handler payloads
* JavaScript string context
* Escaping behavior
* Burp Intruder testing
* Context-aware XSS exploitation

## Next Step

Continue the Cross-Site Scripting learning path by focusing on the remaining practitioner labs related to event-handler contexts, template literals, and real-world XSS impact such as stealing cookies, capturing passwords, and bypassing CSRF defenses.
