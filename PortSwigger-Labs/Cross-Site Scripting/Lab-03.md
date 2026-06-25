## Lab 03: DOM XSS in `document.write` Sink Using Source `location.search`

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / DOM XSS / `document.write` / `location.search`

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a DOM-based XSS vulnerability by injecting a payload into the search functionality that executes JavaScript in the browser.

### Simple Explanation

In this lab, the vulnerability happened inside the browser, not mainly from the server response.

The page used JavaScript to read data from the URL using `location.search`.

Then, the JavaScript wrote that data into the page using `document.write()`.

Because the data was inserted into the page without proper sanitization or encoding, it was possible to break out of the existing HTML attribute and inject new HTML that executed JavaScript.

### Vulnerability Description

The application used client-side JavaScript to read the search query from the URL.

The value from `location.search` was passed into the dangerous sink `document.write()`.

The injected value was written inside an HTML `img` tag attribute.

Because the input was not safely handled, an attacker could break out of the attribute and inject an element with an event handler that executed JavaScript.

This created a DOM-based XSS vulnerability.

### Key Concept

DOM XSS occurs when client-side JavaScript takes user-controlled input and writes it into the page in an unsafe way.

In this lab:

```text
Source: location.search
Sink: document.write()
```

The source was controllable through the URL search parameter.

The sink wrote the value directly into the DOM.

The payload used was:

```html
"><svg onload=alert(1)>
```

### Payload Explanation

The payload was:

```html
"><svg onload=alert(1)>
```

It can be understood as follows:

```text
"
```

Closes the current HTML attribute value.

```text
>
```

Closes the existing HTML tag.

```html
<svg onload=alert(1)>
```

Creates a new SVG element with an `onload` event handler.

When the SVG element loads, the browser executes:

```javascript
alert(1)
```

### Steps Taken

Opened the lab in the browser.

Entered a random search value to observe where the input appeared.

Inspected the page using browser developer tools.

Confirmed that the search value was inserted inside an `img src` attribute.

Entered the following payload into the search box:

```html
"><svg onload=alert(1)>
```

Submitted the search.

Observed that the browser executed the JavaScript and displayed an alert.

Successfully solved the lab.

### Result

The payload executed successfully in the browser.

This confirmed that the application was vulnerable to DOM-based XSS through `document.write()` using data from `location.search`.

### What I Learned

DOM XSS occurs in client-side JavaScript.

The source is where user-controlled data comes from.

The sink is where the data is used in a dangerous way.

`location.search` can be dangerous if its value is written into the page without safe handling.

`document.write()` is a dangerous sink when used with untrusted input.

If input is placed inside an HTML attribute, the payload may need to break out of the attribute first.

Event handlers such as `onload` can execute JavaScript when attached to HTML elements.

### Security Impact

In a real-world application, DOM XSS could allow attackers to execute JavaScript in a victim's browser.

This could lead to session theft, unauthorized actions, page manipulation, phishing, or abuse of the victim's authenticated session.

### Mitigation

To prevent this vulnerability, developers should:

Avoid using `document.write()` with user-controlled input.

Use safe DOM APIs such as `textContent` instead of writing raw HTML.

Apply context-aware output encoding.

Sanitize user-controlled input before inserting it into the DOM.

Avoid placing untrusted data directly inside HTML attributes.

Use secure frameworks that automatically escape output.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
