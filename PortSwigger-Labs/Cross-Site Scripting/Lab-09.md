## Lab 09: Reflected XSS into a JavaScript String with Angle Brackets HTML-Encoded

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / Reflected XSS / JavaScript String Context

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected XSS vulnerability where the search input was reflected inside a JavaScript string and execute the `alert()` function.

### Simple Explanation

In this lab, the search input was not reflected directly inside the HTML body or an HTML attribute.

Instead, the input was reflected inside a JavaScript string.

Angle brackets such as `<` and `>` were HTML-encoded, so using a normal HTML payload like `<script>alert(1)</script>` would not work.

The correct approach was to break out of the JavaScript string and inject JavaScript code directly.

### Vulnerability Description

The application reflected the search query inside a JavaScript string without safely escaping JavaScript string characters.

Although angle brackets were encoded, the single quote character could still be used to break out of the string.

This allowed JavaScript execution inside the page.

### Key Concept

XSS payloads depend on the context where the input appears.

In this lab, the input appeared inside a JavaScript string, so the payload needed to escape the string and create valid JavaScript syntax.

The payload used was:

```javascript
'-alert(1)-'
```

### Payload Explanation

The payload was:

```javascript
'-alert(1)-'
```

It can be understood as follows:

```text
'
```

Closes the original JavaScript string.

```javascript
alert(1)
```

Executes JavaScript.

```text
-'
```

Helps keep the remaining JavaScript syntax valid and prevents the script from breaking.

### Example

If the application generated JavaScript similar to:

```javascript
var searchTerms = 'test123';
```

After injecting the payload, the JavaScript became similar to:

```javascript
var searchTerms = ''-alert(1)-'';
```

This caused `alert(1)` to execute when the page loaded.

### Steps Taken

Opened the lab in the browser.

Submitted a random search value to identify where the input was reflected.

Intercepted the search request using Burp Suite.

Sent the request to Burp Repeater.

Observed that the search input was reflected inside a JavaScript string.

Replaced the search value with the payload:

```http
GET /?search='-alert(1)-' HTTP/2
```

Sent the modified request.

The JavaScript executed immediately and displayed an alert.

Successfully solved the lab.

### Result

The payload executed successfully when the page loaded.

This confirmed that the application was vulnerable to reflected XSS inside a JavaScript string context.

### What I Learned

XSS payloads must be adapted to the reflection context.

Angle bracket encoding prevents direct HTML tag injection, but it does not always prevent XSS.

When input is reflected inside a JavaScript string, the attack can break out of the string and inject JavaScript directly.

Single quotes can be dangerous if user input is placed inside a single-quoted JavaScript string without proper escaping.

JavaScript string context requires different payloads from HTML body or attribute contexts.

### Security Impact

In a real-world application, this vulnerability could allow attackers to craft malicious URLs that execute JavaScript in a victim's browser.

This could lead to session abuse, unauthorized actions, page manipulation, phishing, or exposure of sensitive data depending on the application's security controls.

### Mitigation

To prevent this vulnerability, developers should:

Use context-aware output encoding.

Escape user input correctly before placing it inside JavaScript strings.

Avoid inserting user-controlled data directly into inline JavaScript.

Use safe data handling methods such as JSON encoding where appropriate.

Prefer separating data from executable JavaScript code.

Use secure templating engines that apply proper escaping.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Web Browser
