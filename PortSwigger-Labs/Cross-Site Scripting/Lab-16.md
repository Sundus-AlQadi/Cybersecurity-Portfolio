## Lab 16: Reflected XSS with Some SVG Markup Allowed

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / SVG Markup / WAF Bypass / Event Handler Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected XSS vulnerability where common HTML tags were blocked, but some SVG tags and events were still allowed.

### Simple Explanation

This lab contained a reflected XSS vulnerability in the search functionality.

The application blocked common XSS payloads such as:

```html
<img src=1 onerror=alert(1)>
```

However, the filter was incomplete because it allowed some SVG-related tags and events.

By testing different tags and attributes, I found that the application allowed the SVG context and the `animatetransform` tag with the `onbegin` event.

This made it possible to execute JavaScript using an SVG animation event.

### Vulnerability Description

The application reflected the search input into the HTML response.

A filtering mechanism blocked many common HTML tags and attributes, but it did not block all SVG markup.

The `svg` tag and the `animatetransform` tag were allowed.

The `onbegin` event was also allowed.

Because `onbegin` can execute JavaScript when the SVG animation begins, the payload was able to call `alert(1)`.

### Key Concept

When common HTML tags are blocked, SVG tags can still be dangerous.

SVG supports animation elements and event handlers.

In this lab:

```text
Allowed SVG context: svg
Allowed SVG tag: animatetransform
Allowed event handler: onbegin
Executed function: alert(1)
```

The attack worked because the browser executed the `onbegin` event when the SVG animation element started.

### Payload Used

The final payload used in the URL was:

```text
/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin%3Dalert(1)%3E
```

After URL decoding, the payload becomes:

```html
"><svg><animatetransform onbegin=alert(1)>
```

### Payload Explanation

The payload was:

```html
"><svg><animatetransform onbegin=alert(1)>
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
<svg>
```

Creates an SVG context.

```html
<animatetransform
```

Creates an SVG animation-related element that was allowed by the filter.

```html
onbegin=alert(1)
```

Executes JavaScript when the SVG animation begins.

```text
>
```

Closes the injected element.

### Steps Taken

Opened the lab and tested a standard XSS payload:

```html
<img src=1 onerror=alert(1)>
```

Observed that the payload was blocked.

Used Burp Intruder to test which HTML tags were allowed.

The search value was changed to:

```html
<§§>
```

A list of tags from the XSS Cheat Sheet was used as the payload list.

After reviewing the responses, most tags returned blocked responses, but some SVG-related tags were allowed.

The allowed tags included:

```html
<svg>
<animatetransform>
<title>
<image>
```

Then I tested event attributes using:

```html
<svg><animatetransform §=1>
```

A list of event attributes from the XSS Cheat Sheet was used.

The `onbegin` event returned a successful response.

Finally, I used the following payload in the URL:

```text
/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin%3Dalert(1)%3E
```

The browser executed the payload and displayed an alert.

Successfully solved the lab.

### Result

The payload successfully bypassed the filtering and executed:

```javascript
alert(1)
```

This confirmed that the application was vulnerable to reflected XSS through allowed SVG markup and the `onbegin` event handler.

### What I Learned

Blocking common XSS tags is not enough to prevent XSS.

SVG markup can introduce additional XSS vectors.

Some SVG animation elements can execute JavaScript using event handlers.

The `onbegin` event can trigger JavaScript when an SVG animation starts.

Burp Intruder can be used to identify which tags and attributes are allowed by a filter.

XSS filters must handle HTML tags, SVG tags, attributes, and browser events.

### Security Impact

In a real-world application, an attacker could craft a malicious URL that executes JavaScript in a victim's browser.

This could lead to page manipulation, phishing, unauthorized actions, or abuse of the victim's session depending on the application's security controls.

### Mitigation

To prevent this vulnerability, developers should:

Apply context-aware output encoding.

Avoid reflecting raw user input into HTML responses.

Sanitize or block user-controlled SVG markup.

Block dangerous event handler attributes such as `onbegin`.

Validate input based on expected values.

Apply Content Security Policy as an additional defense.

Test filters against HTML tags, SVG tags, attributes, and browser events.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Intruder

XSS Cheat Sheet

Web Browser

Browser Developer Tools
