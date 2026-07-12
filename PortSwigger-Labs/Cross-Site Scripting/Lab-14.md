## Lab 14: Reflected XSS into HTML Context with Most Tags and Attributes Blocked

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / WAF Bypass / Event Handler Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected XSS vulnerability in the search functionality, bypass the web application firewall, and execute the `print()` function without requiring user interaction.

### Simple Explanation

This lab contained a reflected XSS vulnerability in the search functionality.

However, the application used a web application firewall to block common XSS tags and attributes.

A normal payload such as:

```html
<img src=1 onerror=print()>
```

was blocked.

To solve the lab, I used Burp Intruder to identify which HTML tags and event attributes were still allowed by the WAF.

The testing showed that the `body` tag was allowed and the `onresize` event attribute was also allowed.

Using these two allowed parts, I created a payload that executed `print()` automatically when the page was loaded inside an iframe and resized.

### Vulnerability Description

The application reflected the search input into the HTML response.

Most common XSS payloads were blocked by the WAF.

However, the WAF did not block all possible HTML tags and event handlers.

The `body` tag and `onresize` event were allowed.

This allowed an attacker to inject a payload that executed JavaScript when the page resize event occurred.

Because the lab required no user interaction, the exploit was delivered using an iframe that automatically changed its width on load.

### Key Concept

When a WAF blocks common XSS payloads, the attacker can test which tags and attributes are still allowed.

In this lab:

```text
Allowed tag: body
Allowed event attribute: onresize
Required function: print()
Delivery method: iframe through Exploit Server
```

The exploit worked because resizing the iframe triggered the `onresize` event automatically.

### Payload Used

The final exploit server payload was:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

The reflected search payload was:

```html
"><body onresize=print()>
```

The URL-encoded version was:

```text
%22%3E%3Cbody%20onresize=print()%3E
```

### Payload Explanation

The payload inside the search parameter was:

```html
"><body onresize=print()>
```

It can be understood as follows:

```text
"
```

Breaks out of the current quoted context.

```text
>
```

Closes the current HTML tag.

```html
<body onresize=print()>
```

Injects a `body` tag with an `onresize` event handler.

```javascript
print()
```

Calls the required JavaScript function.

The iframe used:

```html
onload=this.style.width='100px'
```

This changed the iframe width after loading, which triggered the `resize` event and executed `print()` without user interaction.

### Steps Taken

Opened the lab and tested a standard XSS payload:

```html
<img src=1 onerror=print()>
```

Observed that the payload was blocked by the WAF.

Used Burp Suite to send the search request to Burp Intruder.

Tested different HTML tags using the payload position:

```html
<§§>
```

Copied a list of HTML tags from the XSS cheat sheet into Burp Intruder.

Started the attack and reviewed the responses.

Observed that most tags returned blocked responses, but the `body` tag returned a successful response.

Then tested event attributes using:

```html
<body §=1>
```

Copied a list of event attributes into Burp Intruder.

Started the second attack and reviewed the responses.

Observed that the `onresize` attribute was allowed.

Created an exploit using the Exploit Server with the following iframe payload:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

Stored the exploit and delivered it to the victim.

The iframe loaded the vulnerable page, changed size automatically, triggered the `onresize` event, and executed `print()`.

Successfully solved the lab.

### Result

The exploit successfully bypassed the WAF and executed the `print()` function automatically.

This confirmed that the application was vulnerable to reflected XSS despite blocking common tags and attributes.

### What I Learned

A WAF can block common XSS payloads, but it may still allow less common tags or event handlers.

Burp Intruder can be used to identify which tags and attributes are allowed.

The `body` tag can be useful if other tags are blocked.

The `onresize` event can execute JavaScript when the page or frame size changes.

An iframe can be used to trigger events automatically without requiring user interaction.

XSS testing requires understanding both browser behavior and filtering behavior.

Bypassing a WAF often depends on finding allowed HTML contexts and event handlers.

### Security Impact

In a real-world application, this vulnerability could allow an attacker to bypass weak WAF protections and execute JavaScript in a victim's browser.

This could lead to page manipulation, phishing, unauthorized actions, or abuse of the victim's session depending on the application's security controls.

### Mitigation

To prevent this vulnerability, developers should:

Apply proper context-aware output encoding.

Avoid relying only on WAF rules to prevent XSS.

Sanitize user-controlled input using a trusted sanitizer.

Block dangerous HTML tags and event attributes consistently.

Avoid reflecting raw user input into HTML responses.

Use safe templating frameworks that automatically escape output.

Apply Content Security Policy as an additional defense.

Validate input based on expected values.

Test XSS protections against different tags, attributes, and browser events.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Intruder

XSS Cheat Sheet

Exploit Server

Web Browser

Browser Developer Tools
::>

## Lab 14: Reflected XSS into HTML Context with Most Tags and Attributes Blocked

This lab focused on reflected XSS with WAF filtering.

The application reflected the search input into the HTML response, but most common XSS tags and attributes were blocked.

A standard payload such as:

```html
<img src=1 onerror=print()>
```

was blocked.

Using Burp Intruder, I tested which HTML tags were allowed by the WAF.

The allowed tag was:

```html
<body>
```

Then I tested event attributes and found that the allowed event was:

```html
onresize
```

The final reflected payload was:

```html
"><body onresize=print()>
```

The URL-encoded version was:

```text
%22%3E%3Cbody%20onresize=print()%3E
```

Because the lab required no user interaction, the exploit was delivered using an iframe:

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>
```

The iframe loaded the vulnerable page.

Then the iframe changed its width automatically using:

```html
onload=this.style.width='100px'
```

This triggered the `onresize` event and executed:

```javascript
print()
```

### Key Takeaway

WAFs may block common XSS payloads, but they are not a complete defense.

If one tag or attribute is blocked, testing can reveal another allowed tag or event handler.

In this lab, the bypass worked because the `body` tag and `onresize` event were allowed.

The iframe made the exploit automatic, so the victim did not need to interact with the page.
