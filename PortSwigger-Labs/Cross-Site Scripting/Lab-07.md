## Lab 07: Reflected XSS into Attribute with Angle Brackets HTML-Encoded

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / Reflected XSS / HTML Attribute Context / Event Handler Injection

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a reflected XSS vulnerability in the search functionality by injecting an HTML attribute that executes JavaScript.

### Simple Explanation

In this lab, the search input was reflected back into the page inside a quoted HTML attribute.

Angle brackets such as `<` and `>` were HTML-encoded, so injecting a normal HTML tag like `<script>` would not work.

Instead of creating a new tag, the payload escaped from the existing quoted attribute and injected a new event handler attribute.

When the mouse moved over the affected element, the injected JavaScript executed.

### Vulnerability Description

The application reflected user-controlled search input inside an HTML attribute.

Although angle brackets were encoded, quotation marks were not handled securely enough.

This allowed the injected input to break out of the existing attribute value and add a new attribute:

```html
onmouseover="alert(1)"
```

This created a reflected XSS vulnerability in an HTML attribute context.

### Key Concept

XSS payloads depend on the context where the input is reflected.

In this lab, the input was not reflected directly into the HTML body. It was reflected inside a quoted attribute.

Because `<` and `>` were encoded, the attack did not rely on injecting a new HTML tag.

Instead, the payload injected a new event handler attribute.

The payload used was:

```html
"onmouseover="alert(1)
```

The URL-encoded version used in the request was:

```text
%22onmouseover=%22alert(1)
```

### Payload Explanation

The payload was:

```html
"onmouseover="alert(1)
```

It can be understood as follows:

```text
"
```

Closes the original quoted attribute value.

```html
onmouseover="alert(1)
```

Injects a new event handler attribute.

When the user moves the mouse over the affected element, the browser executes:

```javascript
alert(1)
```

### Example

If the application originally generated something like:

```html
<input value="test123">
```

After injecting the payload, the HTML could become:

```html
<input value="" onmouseover="alert(1)">
```

The JavaScript executes when the mouse is moved over the element.

### Steps Taken

Opened the lab in the browser.

Submitted a random search value to identify where the input was reflected.

Intercepted the search request using Burp Suite.

Sent the request to Burp Repeater.

Observed that the search input was reflected inside a quoted HTML attribute.

Noted that angle brackets were HTML-encoded, preventing normal tag injection.

Replaced the search value with the encoded payload:

```text
%22onmouseover=%22alert(1)
```

Sent the modified request.

Copied the URL and opened it in the browser.

Moved the mouse over the affected element.

Observed that the browser executed the JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The payload successfully injected an event handler attribute and executed JavaScript when the mouse moved over the affected element.

This confirmed that the application was vulnerable to reflected XSS in an HTML attribute context.

### What I Learned

XSS payloads must be adapted based on where the input is reflected.

If input appears inside an HTML attribute, it may be necessary to break out of the attribute first.

Encoding angle brackets can prevent tag injection, but it does not fully prevent XSS.

Event handler attributes such as `onmouseover` can execute JavaScript.

Quotation marks must be handled securely when user input is placed inside attributes.

Context-aware output encoding is required to prevent XSS.

### Security Impact

In a real-world application, this vulnerability could allow attackers to craft malicious links that execute JavaScript when a victim interacts with the affected page.

This could lead to session theft, unauthorized actions, phishing, page manipulation, or abuse of the victim's authenticated session.

### Mitigation

To prevent this vulnerability, developers should:

Use context-aware output encoding.

Encode special characters based on where the input is inserted.

Encode quotation marks when placing user input inside HTML attributes.

Avoid inserting user-controlled input directly into attributes.

Use safe templating engines that automatically escape output.

Validate input where appropriate.

Avoid inline event handlers.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Burp Suite Community Edition

Burp Proxy

Burp Repeater

Web Browser

Browser Developer Tools
