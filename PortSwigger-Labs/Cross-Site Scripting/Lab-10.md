## Lab 10: DOM XSS in `document.write` Sink Using Source `location.search` Inside a `select` Element

### Platform

PortSwigger Web Security Academy

### Difficulty

Practitioner

### Topic

Cross-Site Scripting / DOM XSS / `document.write` / `location.search` / Select Element Context

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a DOM-based XSS vulnerability in the stock checker functionality by breaking out of a `select` element and executing the `alert()` function.

### Simple Explanation

In this lab, the vulnerability existed in the product stock checker functionality.

The page used JavaScript to read the `storeId` value from the URL using `location.search`.

Then, the page used `document.write()` to insert that value into the stock checker dropdown list.

Because the value was written into the page as HTML without safe handling, it was possible to break out of the `select` element and inject a new HTML element that executed JavaScript.

### Vulnerability Description

The application used `location.search` as a source of user-controlled input.

The `storeId` query parameter was read from the URL and inserted into the page using the dangerous sink `document.write()`.

The value was placed inside a `select` element used by the stock checker.

Because the input was not safely encoded or sanitized, an attacker could close the `select` element and inject an image element with an `onerror` event handler.

This created a DOM-based XSS vulnerability.

### Key Concept

DOM XSS occurs when client-side JavaScript reads user-controlled data and writes it into the DOM unsafely.

In this lab:

```text
Source: location.search
Sink: document.write()
Context: inside a select element
```

Because the payload was inserted inside a dropdown, the attack needed to break out of the `select` element before injecting executable HTML.

### Payload Used

The payload used was:

```html
"></select><img src=1 onerror=alert(1)>
```

The URL-encoded version was:

```text
%22%3E%3C%2Fselect%3E%3Cimg%20src%3D1%20onerror%3Dalert(1)%3E
```

### Payload Explanation

The payload was:

```html
"></select><img src=1 onerror=alert(1)>
```

It can be understood as follows:

```text
"
```

Closes the current quoted value or attribute.

```text
>
```

Closes the current HTML tag.

```html
</select>
```

Breaks out of the dropdown list.

```html
<img src=1 onerror=alert(1)>
```

Creates an image element with an invalid source.

Because the image source is invalid, the browser triggers the `onerror` event and executes:

```javascript
alert(1)
```

### Steps Taken

Opened a product page in the lab.

Added a `storeId` query parameter with a random value to the URL.

Confirmed that the random value appeared as an option in the stock checker dropdown list.

Inspected the dropdown element using browser developer tools.

Confirmed that the `storeId` value was inserted inside a `select` element.

Changed the `storeId` parameter to the XSS payload.

Used the URL-encoded version of the payload:

```text
%22%3E%3C%2Fselect%3E%3Cimg%20src%3D1%20onerror%3Dalert(1)%3E
```

Loaded the modified URL in the browser.

Observed that the browser executed JavaScript and displayed an alert.

Successfully solved the lab.

### Final URL Pattern

The final URL used this structure:

```text
/product?productId=1&storeId=%22%3E%3C%2Fselect%3E%3Cimg%20src%3D1%20onerror%3Dalert(1)%3E
```

### Result

The payload successfully broke out of the `select` element and executed JavaScript in the browser.

This confirmed that the application was vulnerable to DOM-based XSS through `document.write()` using data from `location.search`.

### What I Learned

DOM XSS can occur when client-side JavaScript writes URL parameters into the page unsafely.

`location.search` is a user-controllable source.

`document.write()` is a dangerous sink when used with untrusted input.

XSS payloads must be adapted based on the HTML context.

When input is placed inside a `select` element, it may be necessary to close the `select` before injecting executable HTML.

The `<img>` tag with an invalid `src` can trigger JavaScript through the `onerror` event.

URL encoding can help deliver payloads safely through query parameters.

### Security Impact

In a real-world application, this vulnerability could allow attackers to craft a malicious URL that executes JavaScript in a victim's browser.

This could lead to unauthorized actions, phishing, page manipulation, or abuse of the victim's authenticated session.

### Mitigation

To prevent this vulnerability, developers should:

Avoid using `document.write()` with user-controlled input.

Use safe DOM APIs such as `textContent`.

Apply context-aware output encoding.

Sanitize user-controlled input before inserting it into the DOM.

Avoid placing untrusted data directly inside HTML structures such as `select` elements.

Validate expected values such as store IDs using an allowlist.

Use secure frameworks that automatically escape output.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
