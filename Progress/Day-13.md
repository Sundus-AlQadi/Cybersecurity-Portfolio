# Day 13 Progress - SQL Injection Completion and Introduction to Cross-Site Scripting

## Completed Labs

### SQL Injection

* SQL Injection Lab 18: SQL Injection with Filter Bypass via XML Encoding

### Cross-Site Scripting

* XSS Lab 01: Reflected XSS into HTML Context with Nothing Encoded
* XSS Lab 02: Stored XSS into HTML Context with Nothing Encoded
* XSS Lab 03: DOM XSS in `document.write` Sink Using Source `location.search`
* XSS Lab 04: DOM XSS in `innerHTML` Sink Using Source `location.search`

## Labs Reviewed / Pending

* SQL Injection Lab 16: Blind SQL Injection with Out-of-Band Interaction
  Status: Pending - requires Burp Collaborator access.

* SQL Injection Lab 17: Blind SQL Injection with Out-of-Band Data Exfiltration
  Status: Pending - requires Burp Collaborator access.

These labs were reviewed but marked as pending because they require Burp Collaborator to verify DNS/HTTP out-of-band interactions. Since my current setup uses Burp Suite Community Edition, I will revisit these labs later when Collaborator access is available.

## Topics Covered

* SQL Injection
* UNION-based SQL Injection
* XML-based input injection
* XML entity encoding
* Filter bypass techniques
* Weak keyword-based filtering
* Retrieving usernames and passwords from database tables
* String concatenation for data extraction
* Cross-Site Scripting
* Reflected XSS
* Stored XSS
* DOM-based XSS
* HTML context injection
* JavaScript execution in the browser
* `location.search` as a DOM XSS source
* `document.write()` as a dangerous sink
* `innerHTML` as a dangerous sink
* HTML attribute breakout
* Event handler payloads
* `onload` and `onerror` execution
* Output encoding and sanitization concepts

## Tools Used

* PortSwigger Web Security Academy
* Burp Suite Community Edition
* Burp Proxy
* Burp Repeater
* Web Browser
* Browser Developer Tools
* GitHub

## Reflection

Today I completed the remaining non-Collaborator SQL Injection lab and started a new topic: Cross-Site Scripting.

In the SQL Injection lab, I worked on filter bypass using XML encoding. The vulnerable feature was the stock check functionality, which sent `productId` and `storeId` values in XML format. A normal UNION-based SQL injection payload was blocked by the application's filter. I learned how XML entity encoding can hide SQL keywords from weak filters, allowing the application to decode and execute the payload later. This helped me understand that keyword-based filtering is not a reliable defense against SQL injection.

I also reviewed two out-of-band SQL Injection labs. These labs require Burp Collaborator to detect external DNS or HTTP interactions. Since Burp Collaborator is not available in my current Burp Suite Community Edition setup, I marked these labs as pending and documented the reason professionally.

After that, I started the Cross-Site Scripting learning path and completed four beginner labs. In the first lab, I practiced reflected XSS, where user input from the search functionality was immediately reflected into the HTML page without encoding. In the second lab, I practiced stored XSS, where a malicious comment was saved by the application and executed when the blog post was viewed.

I then moved into DOM-based XSS. In the third lab, I learned how client-side JavaScript can create XSS when it reads data from `location.search` and writes it into the page using `document.write()`. The payload had to break out of an HTML attribute before injecting a new element. In the fourth lab, I learned how `innerHTML` can be dangerous when used with untrusted input because it interprets the input as HTML. I used an invalid image source with an `onerror` event handler to execute JavaScript.

These labs helped me understand the basic difference between reflected XSS, stored XSS, and DOM-based XSS. They also showed that XSS is not only about inserting `<script>` tags. The execution depends on where the input appears in the page and how the browser interprets it.

## Current Progress

* Completed Authentication Vulnerabilities labs and documentation
* Completed Access Control Vulnerabilities labs and documentation
* Completed SQL Injection labs and documentation
* Marked out-of-band SQL Injection labs as pending due to Burp Collaborator tool requirements
* Started Cross-Site Scripting learning path
* Completed first 4 XSS labs
* Continued daily progress documentation
* Updated notes for SQL Injection and XSS topics

## Key Takeaways

* XML input can also be vulnerable to SQL injection.
* Weak keyword filters can be bypassed using encoding techniques.
* XML entity encoding can hide SQL keywords from filters.
* Secure applications should use parameterized queries instead of relying on filters.
* Out-of-band SQL injection requires external interaction monitoring, such as Burp Collaborator.
* XSS allows JavaScript execution in the victim's browser.
* Reflected XSS occurs when input is immediately returned in the response.
* Stored XSS occurs when input is saved and later displayed to users.
* DOM XSS occurs when unsafe client-side JavaScript handles user-controlled data.
* `location.search` can be a dangerous source if used without validation.
* `document.write()` and `innerHTML` are dangerous sinks when used with untrusted input.
* Output encoding is essential when displaying user-controlled data.
* The correct XSS payload depends on the context where the input appears.

## Milestone Achieved

Completed the main SQL Injection learning path labs that are possible with the current tool setup and started the Cross-Site Scripting learning path.

Topics strengthened:

* SQL Injection filter bypass
* XML encoding
* UNION-based data extraction
* Reflected XSS
* Stored XSS
* DOM-based XSS
* Browser-side JavaScript security
* Source and sink analysis
* Context-aware XSS payload construction

## Next Step

Continue the Cross-Site Scripting learning path, focusing on more DOM XSS labs, different XSS contexts, attribute injection, JavaScript string injection, and XSS mitigation techniques.
