## Lab 18: Reflected XSS into a JavaScript String with Single Quote and Backslash Escaped

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / Reflected XSS / JavaScript String Context / Script Block Breakout

### Lab Status

Solved

### Objective

The goal of this lab was to exploit reflected XSS where the input was reflected inside a JavaScript string, while single quotes and backslashes were escaped.

### Simple Explanation

The search input was reflected inside a JavaScript string.

A normal JavaScript string breakout using a single quote did not work because the application escaped single quotes and backslashes.

Instead of breaking out of the string using quotes, the payload closed the current script block and opened a new script block that executed `alert(1)`.

### Vulnerability Description

The application reflected the search query inside an inline JavaScript string.

Single quotes and backslashes were escaped, which prevented a basic string breakout.

However, the application still allowed angle brackets and script tags to be interpreted by the browser.

This made it possible to close the existing `<script>` tag and inject a new script tag.

### Key Concept

When input appears inside a JavaScript string, escaping quotes can prevent simple string breakout payloads.

However, if the input is inside an HTML `<script>` block and angle brackets are not safely encoded, an attacker may be able to break out of the script block itself.

### Payload Used

The payload used was:

```html
</script><script>alert(1)</script>
```

URL-encoded version:

```text
%3C%2Fscript%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E
```

Final URL pattern:

```text
/?search=%3C%2Fscript%3E%3Cscript%3Ealert(1)%3C%2Fscript%3E
```

### Payload Explanation

The payload was:

```html
</script><script>alert(1)</script>
```

It can be understood as follows:

```html
</script>
```

Closes the existing script block.

```html
<script>alert(1)</script>
```

Creates a new script block and executes JavaScript.

This worked because the browser treats `</script>` as the end of the script block, even if it appears inside a JavaScript string.

### Steps Taken

Opened the lab in the browser.

Submitted a random search value to identify where the input was reflected.

Observed that the value was reflected inside a JavaScript string.

Tested a single quote payload and confirmed that the single quote was escaped.

Used the following payload in the search parameter:

```html
</script><script>alert(1)</script>
```

The browser closed the original script block, executed the injected script, and displayed an alert.

Successfully solved the lab.

### Result

The payload executed successfully and triggered `alert(1)`.

This confirmed that the application was vulnerable to reflected XSS through script block breakout.

### What I Learned

I learned that escaping quotes is not enough if user input inside a script block can still inject `</script>`.

### Security Impact

An attacker could craft a malicious URL that executes JavaScript when opened by a victim.

### Mitigation

Encode user input safely before placing it inside script blocks, and avoid inserting raw user input into inline JavaScript.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Repeater

Web Browser

Browser Developer Tools
