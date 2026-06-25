## Lab 04: DOM XSS in `innerHTML` Sink Using Source `location.search`

### Platform

PortSwigger Web Security Academy

### Difficulty

Apprentice

### Topic

Cross-Site Scripting / DOM XSS / `innerHTML` / `location.search`

### Lab Status

Solved

### Objective

The goal of this lab was to exploit a DOM-based XSS vulnerability by injecting a payload into the search functionality that executes JavaScript in the browser.

### Simple Explanation

In this lab, the vulnerability happened in client-side JavaScript.

The page used data from the URL through `location.search`, then inserted that data into the page using `innerHTML`.

Because `innerHTML` treats input as HTML instead of plain text, it was possible to inject an HTML element with a JavaScript event handler.

The injected image had an invalid `src` value, which caused an error and triggered the `onerror` event.

### Vulnerability Description

The application used client-side JavaScript to read the search query from the URL.

The value from `location.search` was passed into the dangerous sink `innerHTML`.

Since the input was not sanitized or encoded before being written into the page, the browser interpreted the injected input as HTML.

This created a DOM-based XSS vulnerability.

### Key Concept

DOM XSS occurs when client-side JavaScript handles user-controlled input unsafely.

In this lab:

```text
Source: location.search
Sink: innerHTML
```

The source was controllable through the URL search query.

The sink inserted the value into the DOM as HTML.

The payload used was:

```html
<img src=1 onerror=alert(1)>
```

### Payload Explanation

The payload was:

```html
<img src=1 onerror=alert(1)>
```

It can be understood as follows:

```html
<img
```

Creates an image element.

```html
src=1
```

Sets the image source to an invalid value.

```html
onerror=alert(1)
```

Executes JavaScript when the image fails to load.

Because the image source was invalid, the browser triggered the `onerror` event and executed:

```javascript
alert(1)
```

### Steps Taken

Opened the lab in the browser.

Located the search functionality.

Entered the following payload into the search box:

```html
<img src=1 onerror=alert(1)>
```

Clicked the Search button.

Observed that the browser attempted to load the invalid image.

The image failed to load, which triggered the `onerror` event.

The JavaScript executed and displayed an alert.

Successfully solved the lab.

### Result

The payload executed successfully in the browser.

This confirmed that the application was vulnerable to DOM-based XSS through the `innerHTML` sink using data from `location.search`.

### What I Learned

DOM XSS happens in client-side JavaScript.

`location.search` can be dangerous when used as a source of user-controlled data.

`innerHTML` is dangerous when used with untrusted input because it interprets the input as HTML.

The `<script>` tag may not always execute when inserted using `innerHTML`.

HTML event handlers, such as `onerror`, can still execute JavaScript.

An invalid image source can trigger the `onerror` event.

Safe DOM APIs should be used when inserting user-controlled data into the page.

### Security Impact

In a real-world application, DOM XSS could allow attackers to execute JavaScript in a victim's browser.

This could lead to session theft, unauthorized actions, page manipulation, phishing, or abuse of the victim's authenticated session.

### Mitigation

To prevent this vulnerability, developers should:

Avoid assigning user-controlled input to `innerHTML`.

Use safer alternatives such as `textContent`.

Sanitize user-controlled input before inserting it into the DOM.

Apply context-aware output encoding.

Avoid using inline event handlers with untrusted content.

Use secure frameworks that automatically escape output.

Apply Content Security Policy as an additional defense.

### Tools Used

PortSwigger Web Security Academy

Web Browser

Browser Developer Tools
