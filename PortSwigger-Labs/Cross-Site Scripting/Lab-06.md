## Lab 06: DOM XSS in jQuery Selector Sink Using a Hashchange Event

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / DOM XSS / jQuery Selector Sink / `location.hash` / `hashchange`

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a DOM-based XSS vulnerability on the home page and deliver an exploit to the victim that executes the `print()` function in their browser.

### Simple Explanation

In this lab, the vulnerability existed in client-side JavaScript on the home page.

The page used the value from `location.hash`, which is the part of the URL after the `#` symbol.

The application used this value with jQuery's `$()` selector function to auto-scroll to a blog post.

Because the hash value was user-controlled and not safely handled, it was possible to inject an HTML payload that executed JavaScript.

An exploit server was used to deliver the payload to the victim automatically.

### Vulnerability Description

The application used `location.hash` as a source of user-controlled input.

When the hash changed, the page triggered a `hashchange` event.

The value from `location.hash` was passed into jQuery's selector function.

Because the application trusted the hash value, an attacker could inject HTML instead of a normal selector value.

This created a DOM-based XSS vulnerability.

### Key Concept

DOM XSS can occur when client-side JavaScript reads user-controlled data and passes it into a dangerous sink.

In this lab:

```text
Source: location.hash
Sink: jQuery $() selector
Event: hashchange
```

The exploit used an iframe to load the vulnerable home page and then change the hash value after the page loaded.

Changing the hash triggered the vulnerable JavaScript code.

### Final Exploit

The exploit used was:

```html
<iframe src="https://0a9900f104e223f380a5037500a000be.web-security-academy.net/#" onload="this.src=this.src+'<img src=x onerror=print()>'"></iframe>
```

### Payload Explanation

The iframe first loaded the vulnerable home page with an empty hash:

```text
https://0a9900f104e223f380a5037500a000be.web-security-academy.net/#
```

After the iframe loaded, the `onload` event executed:

```javascript
this.src=this.src+'<img src=x onerror=print()>'
```

This changed the iframe URL to include the malicious hash:

```html
#<img src=x onerror=print()>
```

The hash change triggered the page's `hashchange` event.

The injected payload was:

```html
<img src=x onerror=print()>
```

The `src=x` value caused the image to fail loading.

When the image failed to load, the `onerror` event executed:

```javascript
print()
```

This opened the print dialog in the victim's browser.

### Steps Taken

Opened the lab home page.

Reviewed the lab behavior and identified that the vulnerability involved `location.hash`.

Opened the exploit server from the lab banner.

Placed the malicious iframe payload in the Body section of the exploit server.

Stored the exploit.

Clicked View exploit to confirm that the `print()` function was triggered.

Returned to the exploit server.

Clicked Deliver exploit to victim.

Successfully solved the lab.

### Result

The exploit successfully triggered the `print()` function in the victim's browser.

This confirmed that the application was vulnerable to DOM-based XSS through a jQuery selector sink using the `hashchange` event.

### What I Learned

`location.hash` is user-controllable because it comes from the part of the URL after `#`.

The `hashchange` event fires when the hash part of the URL changes.

jQuery's `$()` selector can become dangerous when it receives untrusted input.

A selector should be used to find existing elements, but unsafe input may cause injected HTML to be interpreted in a dangerous way.

An iframe can be used to load a vulnerable page and trigger a hash change automatically.

The `onload` event can be used to modify the iframe URL after it loads.

The `<img>` tag with an invalid `src` can trigger JavaScript through the `onerror` event.

Some XSS labs require delivering the payload to a victim using the exploit server.

### Security Impact

In a real-world application, this type of DOM XSS could allow attackers to execute JavaScript in a victim's browser.

This could lead to unauthorized actions, session abuse, phishing, page manipulation, or exposure of sensitive information depending on the application's security controls.

### Mitigation

To prevent this vulnerability, developers should:

Avoid passing user-controlled data directly into jQuery selector functions.

Validate and restrict values taken from `location.hash`.

Use safe allowlists for expected hash values.

Avoid interpreting user-controlled data as HTML or selectors.

Use safe DOM APIs instead of dangerous sinks.

Sanitize untrusted input before using it in client-side JavaScript.

Keep JavaScript libraries such as jQuery updated.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Exploit Server

Web Browser

Browser Developer Tools
